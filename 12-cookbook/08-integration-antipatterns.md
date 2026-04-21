# 08 — Integration Anti-Patterns

Straight from the [official Integration Practices guide](https://sdk.myinvois.hasil.gov.my/integration-practices/),
these are the five things NOT to do. Each one will either get you throttled,
rate-limited, or flagged, even if your payloads are perfect.

## 1. Don't acquire a new auth token per API call

**Tokens are valid for 60 minutes.** Cache them.

```python
# BAD
def submit_invoice(doc):
    token = login()                      # every call re-auths — wastes login quota
    return api.submit(doc, token)
    
# GOOD
class Client:
    def __init__(self):
        self._token = None
        self._expires_at = 0
    
    def _get_token(self):
        if self._token and time.time() < self._expires_at - 60:  # 60s buffer
            return self._token
        resp = login()
        self._token = resp["access_token"]
        self._expires_at = time.time() + resp["expires_in"]
        return self._token
```

**Why it matters:** Login as Taxpayer is capped at 12 rpm. If each document
submission re-auths, you'll hit that cap with just 12 submissions per minute —
and your actual submit quota is **100 rpm**. You'd be hitting a ceiling 8× too
low than what you've actually paid for.

## 2. Don't poll submission status via "Get Recent" or "Search"

These APIs are **not** for polling. Use **`Get Submission`** with the UID you
received from `Submit Documents`.

| API | Purpose |
|---|---|
| `Get Recent Documents` | Browsing recently-validated invoices (UI use case) |
| `Search Documents` | Filtered search (UI use case) |
| `Get Submission` | **Check status of a submission you just made** |
| `Get Document` | Retrieve a specific validated document by ID |

```python
# BAD
submit_result = submit_documents(docs)
sleep(10)
while True:
    recents = get_recent_documents()  # throttled at 12 rpm!
    if submit_result.id in [d.id for d in recents]:
        break
    sleep(5)

# GOOD
submit_result = submit_documents(docs)
while True:
    status = get_submission(submit_result.submissionUID)  # 300 rpm, intended for this
    if status.overallStatus not in ["InProgress"]:
        break
    sleep(5)
```

**Why it matters:** `Get Recent` is 12 rpm. `Get Submission` is 300 rpm — it
was built for polling. Using the wrong one wastes quota and scales badly.

## 3. Don't resubmit on 20x responses

If `Submit Documents` returned HTTP 20x, **the submission was accepted.**
Do not resend.

Resending:
- Gets flagged as a duplicate (→ `DS302`)
- Can trigger submission-API throttling on top

The right pattern:

```
If you got 2xx → submission accepted → poll Get Submission for final status.
If you got 4xx → fix the request, submit corrected version.
If you got 5xx → retry the same payload after a short delay.
If you got 429 → wait the Retry-After period, then retry.
```

## 4. Don't use `Get Recent Documents` as a browse API

It's throttled at 12 rpm. For any real browsing use case, use **`Search
Documents`** (also 12 rpm but supports filtering, so you do 1 call with a date
range instead of multiple `Get Recent` calls).

## 5. Don't call `/connect/token` with malformed params

These are blocked at the edge (doesn't even count toward your rate limit, but
clutters logs):

- `client_id` = empty / blank
- `client_id` = literal `'0'`
- `client_id` = literal `'YOUR_CLIENT_ID'` (the Postman default — someone forgot to set the env var)
- Missing `grant_type` param
- Missing `client_secret`

**Practical fix:** have your config loader assert these are non-empty real
values before your code can even reach the API. Example:

```python
import os

def load_config():
    cfg = {
        "client_id": os.environ["MYINVOIS_CLIENT_ID"],
        "client_secret": os.environ["MYINVOIS_CLIENT_SECRET"],
    }
    forbidden = ["", "0", "YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET", "change-me"]
    for k, v in cfg.items():
        if v.strip() in forbidden:
            raise RuntimeError(f"Config {k} is a placeholder: {v!r}")
    return cfg
```

---

## Beyond the official list — additional anti-patterns from real SDKs

### 6. Don't store access tokens in the global scope of a long-running process without invalidation

Tokens expire after 60 minutes. If your process runs for hours and you cache
the token in a module-level variable with no expiry check, the first call
after the 60-minute mark will fail with 401. Always check expiry before use.

### 7. Don't encode the `documentHash` before hashing

The submission payload requires:

```json
{
  "documents": [{
    "format": "JSON",
    "documentHash": "<SHA-256 of signed document content>",
    "codeNumber": "INV-123",
    "document": "<base64 of signed document>"
  }]
}
```

- `documentHash` = SHA-256 of the **signed document content** (raw bytes,
  before base64), then base64-encoded.
- `document` = base64 of the same content.

A common bug: base64-encoding the content first, then SHA-256'ing the base64
string. The hash won't match what the server computes.

```javascript
// CORRECT
const content = fs.readFileSync("signed-invoice.xml");  // raw bytes
const documentHash = crypto.createHash("sha256").update(content).digest("base64");
const document = content.toString("base64");

// WRONG (hash of base64, not of content)
const document = content.toString("base64");
const documentHash = crypto.createHash("sha256").update(document).digest("base64");
```

### 8. Don't build UBL from strings

Hand-crafting XML via template strings produces hard-to-debug structure errors
and order violations. Use a real UBL library:

- PHP: `klsheng/myinvois-php-sdk` (includes a re-created UBL-Invoice package)
- Python: `ubl-invoice` or the Frappe app's builders
- Node.js: the `syukranDev/e-invoice-sdk-nodejs` helpers
- .NET: see the [mokth/einvoice](https://github.com/mokth/einvoice) C# project

These libraries maintain element order, handle namespaces, and escape special
characters automatically.

### 9. Don't set the same `onbehalfof` across multiple concurrent requests as an intermediary

If you're an intermediary submitting for multiple taxpayers in parallel, each
request's `onbehalfof` value must match the TIN for that specific document.
Mixing them up causes signature validator failures (`DS311`) because the
authenticated submitter TIN won't match the document's supplier TIN.

### 10. Don't hardcode `preprod-api` or `api` base URL switching at multiple layers

Have one config knob for environment. Common bug: the SDK defaults to sandbox,
your code explicitly overrides to production at one layer but not another, and
you accidentally send real invoices into sandbox (or worse, test invoices into
production).

## Summary card

| Do | Don't |
|---|---|
| Cache tokens for their `expires_in` lifetime | Call `/connect/token` per request |
| Poll with `Get Submission` (300 rpm) | Poll with `Get Recent` / `Search` (12 rpm each) |
| Check Submit response → status → poll | Resend submission on any hiccup |
| Use `Retry-After` on 429 | Tight-loop retry on 429 |
| Validate config at startup | Ship with placeholder `client_id` defaults |
| Hash the raw signed bytes | Hash the base64 representation |
| Use a real UBL builder | Concatenate XML strings |

# 04 — Rate Limits & `429 Too Many Requests`

MyInvois enforces per-API, per-client-ID rate limits. Exceed the limit and you
get **HTTP 429 `TooManyRequests`** with a `Retry-After` response header telling
you how many seconds to wait.

## Per-API rate limits (production)

| API | Requests per minute / client ID |
|---|---|
| Login as Taxpayer System | **12** |
| Login as Intermediary System | **12** |
| Submit Documents | **100** |
| Get Submission | **300** |
| Cancel Document | 12 |
| Reject Document | 12 |
| Get Document | 60 |
| Get Document Details | 125 |
| Get Recent Documents | 12 |
| Search Documents | 12 |
| Search Taxpayer's TIN | 60 |
| Taxpayer's QR Code | 60 |

### Observations

- **`Get Submission` is by far the most permissive** (300 rpm) because the
  official integration pattern requires polling. Build your poller around it.
- **`Get Recent Documents` and `Search Documents` are tightly limited** (12
  rpm). Use `Get Submission` whenever possible, not these.
- **Login APIs are limited to 12 rpm** — because the **token is valid for 60
  minutes**, you should only call Login when your cached token is near expiry.
  Frequent re-login is the #1 way to get throttled for free.

## Sandbox limits are lower

Per the integration-practices page (effective 28 April 2025):

> The sandbox environment is intended for functional testing and has a **lower
> API rate limit** compared to the production environment.

LHDN doesn't publish the sandbox numbers, but assume they're 25–50% of
production. Don't load-test in sandbox.

## Handling 429 correctly

When you get HTTP 429:

1. **Read the `Retry-After` header** — it contains the number of seconds to wait.
2. **Sleep at least that long.**
3. **Retry the same request** — do not change anything, the system will accept
   it once you're back under the limit.

```python
import requests
import time

def request_with_retry(method, url, headers, max_retries=3, **kwargs):
    for attempt in range(max_retries):
        resp = requests.request(method, url, headers=headers, **kwargs)
        if resp.status_code == 429:
            wait = int(resp.headers.get("Retry-After", "30"))
            logger.warning(f"Rate limited, sleeping {wait}s (attempt {attempt + 1})")
            time.sleep(wait)
            continue
        return resp
    raise RuntimeError(f"Rate-limited after {max_retries} retries")
```

### Don't do this

```python
# BAD: tight retry loop — wastes your next-minute budget too
while True:
    resp = requests.post(url, ...)
    if resp.status_code == 429:
        continue  # immediately tries again
    break
```

This will exhaust your quota in a different minute as well — you effectively
DDoS yourself.

## Polling pattern for `Submit Documents`

The official integration pattern is **polling**:

```
1. Login → cache token for 60 min
2. SubmitDocuments → get submissionUID
3. Wait for a short period (proportional to batch size)
4. GetSubmission(submissionUID) → check overall status
5. If still in progress, sleep, repeat step 4
6. Once valid/invalid, process results
```

**Cadence recommendation:**
- Small batch (1–10 docs): first poll after 5 seconds, then every 5 seconds.
- Large batch (50+ docs): first poll after 30 seconds, then every 10 seconds.
- Use exponential backoff if repeatedly "still in progress" — don't hammer it.

At 300 rpm for Get Submission, you can safely poll every 200ms per submission
— but you rarely need to. Longer intervals are kinder to the system and don't
change your result time.

## Other 429 causes beyond raw call count

From the integration-practices guide — these behaviors specifically trigger
throttling even if you're technically under the per-minute limit:

1. **Acquiring a new auth token with every API call.** Cache tokens for their
   stated `expires_in` lifetime. Default is 60 minutes — use it.

2. **Calling `Get Recent Documents` / `Search Documents` to check submission
   status.** These APIs are for *browsing* validated documents, not for polling
   status. Use `Get Submission` during submission flows.

3. **Resubmitting the same submission repeatedly.** If your first call got a
   `20x`, the submission was accepted. Don't resend — check its status. Repeat
   resubmissions get flagged as duplicates and can trigger throttling on top.

4. **Calling `/connect/token` with malformed params.** Empty `client_id` or
   literal `'YOUR_CLIENT_ID'` placeholder values get blocked at the edge. Make
   sure your config loader actually loaded the env vars before calling.

## Correlation with `Retry-After` format

The `Retry-After` header can be either:
- An integer (seconds): `Retry-After: 30`
- An HTTP date: `Retry-After: Wed, 21 Oct 2025 07:28:00 GMT`

Most integrations only see the integer form from MyInvois. Handle both just in case:

```javascript
function parseRetryAfter(header) {
  if (!header) return 30; // default
  const asNumber = parseInt(header, 10);
  if (!isNaN(asNumber)) return asNumber;
  const asDate = new Date(header);
  if (!isNaN(asDate.getTime())) {
    return Math.max(0, (asDate.getTime() - Date.now()) / 1000);
  }
  return 30;
}
```

## SDK-level auto-retry

The [syukranDev/e-invoice-sdk-nodejs](https://github.com/syukranDev/e-invoice-sdk-nodejs)
advertises "Automatic API recall in case of hitting the API rate limit" as a
feature — useful reference for implementing this in your own wrapper.

The [klsheng PHP SDK](https://github.com/klsheng/myinvois-php-sdk) does not
auto-retry — it surfaces the 429 to the caller, which is the more correct
approach for library code (let the application decide retry policy).

## Reference

- [Integration Practices → Rate Limit](https://sdk.myinvois.hasil.gov.my/integration-practices/#rate-limit)
- [Standard Error Response → 429](https://sdk.myinvois.hasil.gov.my/standard-error-response/)

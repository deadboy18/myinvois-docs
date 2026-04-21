# 01 — HTTP Status Codes & Standard Error Response

All MyInvois APIs return errors in a consistent envelope. Knowing the shape
before you parse saves a lot of time.

## Standard error response envelope

Errors come back as a single JSON object with a top-level `error` key. Each
`Error` object can nest additional `Error` objects in `innerError`, so you may
get a tree of errors back in a single response.

```json
{
  "status": "Invalid",
  "error": {
    "propertyName": null,
    "propertyPath": null,
    "errorCode": "Error03",
    "error": "Duplicated Submission Validator",
    "errorMS": "Penduaan Submission Validator",
    "innerError": [
      {
        "propertyName": "document",
        "propertyPath": "document",
        "errorCode": "DS302",
        "error": "Duplicated submitted document with UUID 132YBP1HE6GHJ0XSVNW0141J10 where the values of configured attributes match with values for the document with UUID: GJCGP9RWJF7961J8XNW0141J10",
        "errorMs": "MS-Duplicated submitted document ...",
        "innerError": null
      }
    ]
  },
  "name": "Step03-Duplicated Submission Validator"
}
```

### Field-by-field

| Field | Type | Meaning |
|---|---|---|
| `propertyName` | string\|null | Name of the parameter / field that caused the error (e.g. `document`) |
| `propertyPath` | string\|null | JSON-path or XPath to the offending value (e.g. `$.InvoiceLineItem[*].InvoicedQuantity.unitCode`) |
| `errorCode` | string | Machine-readable error code. Programmatic handling should key on this. |
| `error` | string | English error message |
| `errorMS` | string | Malay error message (note: official docs spell this both `errorMS` and `errorMs` — handle either) |
| `target` | string\|null | Optional — the target of the error (e.g. `password`) |
| `innerError` | Error[]\|null | Nested errors, same shape recursively. Used so callers can fix everything in one pass. |

## HTTP status codes

The official status-code table from the SDK:

| HTTP | Internal code | When you'll see it | Action |
|---|---|---|---|
| `400` | `BadRequest` | Supplied parameters are invalid in shape / structure. Don't retry without changing the request. | Fix payload, resubmit. |
| `400` | `BadArgument` | A specific parameter is wrong. Offending arg name is in `target`. | Fix the named field. Common cause: TIN format. |
| `401` | `Unauthorized` | Token missing, malformed, or **expired (1 hr lifetime)**. | Re-login via `Login as Taxpayer/Intermediary`. Cache new token. |
| `403` | `Forbidden` | You authenticated but don't have access to this API/data. | See [pitfall: sandbox credentials](05-common-pitfalls.md#403-forbidden-in-sandbox) |
| `404` | `NotFound` | Resource doesn't exist (e.g. UUID not found). | Check the ID. Often means "not processed yet" if right after submission. |
| `429` | `TooManyRequests` | Rate limit exceeded. `Retry-After` response header contains seconds to wait. | See [04-rate-limits.md](04-rate-limits.md). |
| `500` | `InternalServerError` | Server-side error, not your request. | Retry with backoff. Include `correlationId` header when reporting. |
| `501` | `NotImplemented` | The method/API isn't implemented or supported. | Don't retry. Check the SDK version. |
| `503` | `ServiceUnavailable` | System is up but degraded. | Retry with backoff. |

## The `correlationId` header

**Every error response carries a `correlationId` response header.** Capture
and log it. When you raise a support ticket with LHDN / IRBM, include it — they
can find your exact request with it.

```python
# Always log correlation IDs alongside errors
resp = requests.post(url, ...)
if resp.status_code >= 400:
    logger.error(
        "MyInvois error",
        status=resp.status_code,
        correlation_id=resp.headers.get("correlationId"),
        body=resp.text,
    )
```

## Two flavours of "error" to keep separate

1. **Transport / request-level errors** — HTTP 4xx / 5xx. Handled by your HTTP
   client. Means "the call itself failed".
2. **Validation / business errors** — the request succeeded (HTTP 200),
   but `status: "Invalid"` in the body, often with a chain of `innerError`
   entries. This is what happens when `Submit Documents` accepts your upload
   but the validators reject individual documents.

Your code needs both paths. A common community-SDK mistake is treating every
non-2xx as a validation error and vice-versa.

## Worked example: parsing nested errors

```javascript
function flattenErrors(err, path = []) {
  const results = [];
  if (!err) return results;
  results.push({
    code: err.errorCode,
    message: err.error,
    propertyPath: err.propertyPath,
    chain: [...path, err.errorCode].join(' → '),
  });
  if (Array.isArray(err.innerError)) {
    for (const inner of err.innerError) {
      results.push(...flattenErrors(inner, [...path, err.errorCode]));
    }
  }
  return results;
}

// Usage:
const errors = flattenErrors(response.error);
// → [
//   { code: 'Error03', chain: 'Error03' },
//   { code: 'DS302',   chain: 'Error03 → DS302' }
// ]
```

---

## Validator-step naming convention

The `name` field on the outer error uses the format
`Step{NN}-{Validator Name}`. The `NN` isn't an error code itself, it's just
the execution order of validators. The 7 validators and their numbering:

| Step | Validator | Ref |
|---|---|---|
| Step01 | Structure Validator | [03-submission-errors.md](03-submission-errors.md#structure-validator) |
| Step02 | Core Fields Validator | [03-submission-errors.md](03-submission-errors.md#core-fields-validator) |
| Step03 | Duplicated Submission Validator | [09-duplicate-detection.md](09-duplicate-detection.md) |
| Step04 | Signature Validator | [02-signature-errors.md](02-signature-errors.md) |
| Step05 | Taxpayer Validator | [03-submission-errors.md](03-submission-errors.md#taxpayer-validator) |
| Step06 | Referenced Documents Validator | [03-submission-errors.md](03-submission-errors.md#referenced-documents-validator) |
| Step07 | Code Validator | [03-submission-errors.md](03-submission-errors.md#code-validator) |

Note: "Currency Validator" is documented as the 8th but doesn't appear to have
its own Step number — it runs as part of Code Validator in practice.

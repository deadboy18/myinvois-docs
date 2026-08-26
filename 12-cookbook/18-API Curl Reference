
# 18 - API Curl Reference

Every MyInvois API endpoint as a ready-to-paste `curl` command. Useful for
debugging when your SDK is sending something wrong — compare against these
known-good calls.

**Source:** Official Postman collection + SDK portal endpoint documentation.

## Setup

```bash
# Sandbox (default for testing)
BASE=https://preprod-api.myinvois.hasil.gov.my

# Production (switch when going live)
# BASE=https://api.myinvois.hasil.gov.my
```

## Authentication

```bash
# Login as Taxpayer System → access_token (valid ~60 min, cache it!)
curl -s -X POST "$BASE/connect/token" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "client_id=$CLIENT_ID&client_secret=$CLIENT_SECRET&grant_type=client_credentials&scope=InvoicingAPI"

# Login as Intermediary System (acting on behalf of a taxpayer)
# Same as above, add header:
#   -H "onbehalfof: <taxpayer TIN>"
```

**Rate limit:** 12 requests per minute on the Login endpoint. Cache your
token — don't re-auth per API call.

## Platform API

```bash
# Get all document types
curl -s "$BASE/api/v1.0/documenttypes" \
  -H "Authorization: Bearer $TOKEN"

# Get one document type (structure definitions)
curl -s "$BASE/api/v1.0/documenttypes/{id}" \
  -H "Authorization: Bearer $TOKEN"

# Get document type version
curl -s "$BASE/api/v1.0/documenttypes/{id}/versions/{vid}" \
  -H "Authorization: Bearer $TOKEN"

# Get notifications
curl -s "$BASE/api/v1.0/notifications/taxpayer?dateFrom=&dateTo=&type=&language=&status=&channel=&pageNo=&pageSize=" \
  -H "Authorization: Bearer $TOKEN"
```

## TIN Validation & Search

```bash
# Validate taxpayer TIN (200 = valid, 404 = not found)
curl -s -o /dev/null -w "%{http_code}" \
  "$BASE/api/v1.0/taxpayer/validate/{tin}?idType=BRN&idValue={brn}" \
  -H "Authorization: Bearer $TOKEN"
# idType options: NRIC, BRN, PASSPORT, ARMY

# Search taxpayer TIN (reverse lookup: ID → TIN)
curl -s "$BASE/api/v1.0/taxpayer/search/tin?idType=NRIC&idValue=770625015324" \
  -H "Authorization: Bearer $TOKEN"
# Optional params: &taxpayerName=...&fileType=1 (1=individual, 2=company)
# Rate limit: 60 requests per minute
```

## Document Submission

```bash
# Submit documents (JSON format)
# documentHash = SHA-256 hex of exact document bytes (before base64)
# document = base64-encoded UBL JSON
curl -s -X POST "$BASE/api/v1.0/documentsubmissions" \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "documents": [{
      "format": "JSON",
      "codeNumber": "INV-2026-0001",
      "documentHash": "<sha256hex>",
      "document": "<base64>"
    }]
  }'

# Poll submission status (repeat until overallStatus != "InProgress")
curl -s "$BASE/api/v1.0/documentsubmissions/{submissionUid}?pageSize=100" \
  -H "Authorization: Bearer $TOKEN"
# overallStatus values: InProgress, Valid, Partially Valid, Invalid
```

### Generating the hash and base64

```bash
# From a file:
HASH=$(sha256sum invoice.json | cut -d' ' -f1)
DOC=$(base64 -w0 invoice.json)

# Then use in the submission payload:
# "documentHash": "$HASH", "document": "$DOC"
```

```python
# From Python:
import hashlib, base64, json

raw = json.dumps(doc).encode("utf-8")
doc_hash = hashlib.sha256(raw).hexdigest()
doc_b64 = base64.b64encode(raw).decode("ascii")
```

## Document Retrieval

```bash
# Get document details (validation results, longId for share link)
curl -s "$BASE/api/v1.0/documents/{uuid}/details" \
  -H "Authorization: Bearer $TOKEN"

# Get raw document source + metadata
curl -s "$BASE/api/v1.0/documents/{uuid}/raw" \
  -H "Authorization: Bearer $TOKEN"

# Get recent documents (last 31 days)
curl -s "$BASE/api/v1.0/documents/recent?pageNo=1&pageSize=20&direction=Sent&status=Valid" \
  -H "Authorization: Bearer $TOKEN"
# More filters: submissionDateFrom/To, issueDateFrom/To, documentType,
# receiverIdType/receiverId/receiverTin, issuerTin/issuerIdType/issuerId

# Search documents (wider than 31 days, filterable)
curl -s "$BASE/api/v1.0/documents/search?pageNo=1&pageSize=20&submissionDateFrom=2026-06-01T00:00:00Z&submissionDateTo=2026-07-01T00:00:00Z" \
  -H "Authorization: Bearer $TOKEN"
```

## Document State Changes

```bash
# Cancel document (issuer, within 72h of validation)
curl -s -X PUT "$BASE/api/v1.0/documents/state/{uuid}/state" \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"status": "cancelled", "reason": "wrong amount"}'

# Reject document (buyer requests cancellation, within 72h)
curl -s -X PUT "$BASE/api/v1.0/documents/state/{uuid}/state" \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"status": "rejected", "reason": "not my purchase"}'
```

## Share / Validation Link

After getting `longId` from the document details response:

```
# Sandbox
https://preprod.myinvois.hasil.gov.my/{uuid}/share/{longId}

# Production
https://myinvois.hasil.gov.my/{uuid}/share/{longId}
```

## QR Code

```bash
# Decode a scanned taxpayer QR code
curl -s "$BASE/api/v1.0/taxpayers/qrcodeinfo/{qrCodeText}" \
  -H "Authorization: Bearer $TOKEN"
```

## Quick reference: document type codes

| Code | Type | Self-billed |
|---|---|---|
| 01 | Invoice | 11 |
| 02 | Credit Note | 12 |
| 03 | Debit Note | 13 |
| 04 | Refund Note | 14 |

`listVersionID`: `1.0` (no signature required) or `1.1` (XAdES digital
signature required).

## Environment URLs

| Env | API Base | Portal | Login via |
|---|---|---|---|
| Sandbox | `https://preprod-api.myinvois.hasil.gov.my` | `https://preprod.myinvois.hasil.gov.my` | `https://preprod-mytax.hasil.gov.my` |
| Production | `https://api.myinvois.hasil.gov.my` | `https://myinvois.hasil.gov.my` | `https://mytax.hasil.gov.my` |

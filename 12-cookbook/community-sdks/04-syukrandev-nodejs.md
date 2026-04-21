# syukranDev/e-invoice-sdk-nodejs

**Language:** Node.js / JavaScript  
**License:** ISC  
**npm:** [`einvoice-sdk-nodejs`](https://www.npmjs.com/package/einvoice-sdk-nodejs)  
**Repo:** <https://github.com/syukranDev/e-invoice-sdk-nodejs>  
**Author:** Syukran Soleh  
**Claim:** "Pioneered the first Node.js SDK for Malaysia LHDN e-Invoicing (early adoption)"

A lightweight, JSON-focused Node.js SDK. Single-file implementation
(`einvoice-sdk.js`) — easy to audit, easy to modify, good starting point
if you're building something Node-based.

## Install

```bash
npm install einvoice-sdk-nodejs
```

Or clone the repo and require the file directly for customization.

## Configuration (.env)

The SDK reads configuration from environment variables:

```env
CLIENT_ID_VALUE=your-client-id
CLIENT_SECRET_1_VALUE=your-client-secret
PREPROD_BASE_URL=https://preprod-api.myinvois.hasil.gov.my
X509Certificate_VALUE=your-x509-certificate
X509SubjectName_VALUE=your-x509-subject-name
X509IssuerName_VALUE=your-x509-issuer-name
X509SerialNumber_VALUE=your-x509-serial-number
PRIVATE_KEY_FILE_PATH=example.key
PRIVATE_CERT_FILE_PATH=exampleCert.crt
```

> Note: config is pre-loaded at module import. Changing `.env` requires
> restart. If you need dynamic per-tenant config, fork the module.

## Basic usage

```javascript
const einvois = require('einvoice-sdk-nodejs');

try {
  // 1. Obtain token as taxpayer
  const token = await einvois.getTokenAsTaxPayer();
  
  // 2. Prepare the hashed payload
  //    (see getCertificatesHashedParams() for how to generate signed docs)
  const hashed_payload = {
    documents: [{
      format: "JSON",
      documentHash: "<sha256_encoded_signed_document>",
      codeNumber: "INV-2024-001",
      document: "<base64_encoded_signed_document>"
    }]
  };
  
  // 3. Submit
  const resp = await einvois.submitDocument(
    hashed_payload.documents,
    token.access_token
  );
  console.log(resp);
} catch (error) {
  console.error(error);
}
```

## Advertised feature set

- Obtain tokens as taxpayer OR intermediary
- Validate TIN using different ID types (NRIC / PASSPORT / BRN / ARMY)
- Submit documents
- Get document details
- Cancel valid documents (supplier-initiated)
- Utility functions:
  - JSON to Base64 conversion
  - SHA-256 hash calculation
  - Generating certificate-hashed parameters and hashed documents
- **Automatic API recall on rate-limit hit** (429 Too Many Requests)

### About the auto-retry

The README specifically calls out auto-retry on 429. This is unusual among
community SDKs (the PHP SDK surfaces 429 to the caller). Can be a feature or
a footgun:

- **Pro:** fire-and-forget calls just work.
- **Con:** if your caller doesn't know the call was rate-limited, you can't
  adjust your upstream throttle. Hidden delays compound.

If you fork, consider making it a configurable option rather than always-on.

## Supported endpoints (smaller than klsheng PHP)

| Endpoint | Supported |
|---|---|
| Login as Taxpayer | ✅ |
| Login as Intermediary | ✅ |
| Validate Taxpayer TIN | ✅ |
| Submit Documents (JSON) | ✅ |
| Submit Documents (XML) | ❌ (JSON only) |
| Get Document Details | ✅ |
| Get Submission | ✅ |
| Cancel Document | ✅ |
| Reject Document | ❌ |
| Search Documents | ❌ |
| Get Recent Documents | ❌ |
| Notifications | ❌ |
| QR code URL | ❌ |
| Digital signature utilities | ✅ (exposes `getCertificatesHashedParams`) |

If you need the unimplemented endpoints, you can either:
1. Wrap them yourself (the existing code is a clear reference pattern), or
2. Fork and contribute back.

## When to pick this

✅ You're building a **Node.js backend** or serverless function
✅ You only deal with **JSON** invoices (not XML)
✅ You want something **small and auditable** (single file)
✅ You need **auto-retry** and don't want to write the rate-limit handler yourself

## When to skip this

❌ You need XML submission — not supported
❌ You need the full endpoint suite (Reject, Search, Notifications, etc.)
❌ You want TypeScript typings (not provided)
❌ You're in a compliance-critical environment and want a more-audited SDK

## Strengths

- ✅ Lightweight, readable, single-file
- ✅ Published to npm
- ✅ Auto-retry on 429 built in
- ✅ ISC license (permissive)
- ✅ Stated early-adopter pedigree ("first Node.js SDK")

## Weaknesses

- ❌ JSON-only (no XML support)
- ❌ Partial endpoint coverage
- ❌ No TypeScript types
- ❌ Configuration via env vars only — no programmatic override
- ❌ Low activity (25 commits as of April 2026)
- ❌ Limited test coverage visible
- ❌ Documentation is sparse — key function signatures aren't in the README

## Reference helper: generating the hashed document

The SDK exposes `getCertificatesHashedParams()` to generate the parameters
needed for the submission payload. You'll need to read the source to see the
exact signature — it handles:

1. Canonicalizing / minifying the document
2. Computing `DocDigest` (SHA-256 base64 of document)
3. Signing via the private key in `PRIVATE_KEY_FILE_PATH`
4. Generating `CertDigest`
5. Returning the signed document + hash pair

If you've read [02-signature-errors.md](../02-signature-errors.md) you'll
recognize this as steps 3–7 of the official signing pipeline.

## Reference

- [Repo](https://github.com/syukranDev/e-invoice-sdk-nodejs)
- [npm package](https://www.npmjs.com/package/einvoice-sdk-nodejs)

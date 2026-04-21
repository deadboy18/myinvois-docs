# 02 — Signature Errors (DS300–DS338)

**This is the single most practically useful section of the cookbook.**
When a submission fails the Signature Validator, the error code will be in the
`DS3xx` range. The official MyInvois SDK doesn't enumerate these — but
PosDigicert (one of the certificate authorities licensed for MyInvois) published
the full table in their [Signature Creation Guideline v1.2](https://www.posdigicert.com.my/public/uploads/files/PosDigicert-Document_Signature_Creation_Guideline_Rev1.pdf).

The list below is reproduced with added fixes / likely causes per code.

## Quick index by category

- **DS300–DS303** — Document parsing & signature presence
- **DS304–DS312** — Certificate validity & identity matching
- **DS313–DS316** — Algorithm mismatches (SHA-256 / RSA)
- **DS317–DS323** — Digest value mismatches (hash of doc / props / cert)
- **DS324–DS328** — Issuer name / serial number mismatches
- **DS329–DS331** — Chain of trust, TIN lookup
- **DS332–DS338** — Signature value validation & parsing failures

## The full table

| Code | Official description | Likely cause / fix |
|---|---|---|
| `DS300` | Failed to parse input document. | Malformed XML/JSON. Run it through a validator first. Check UTF-8 encoding, unescaped entities. |
| `DS301` | Signature not found. | The `UBLExtensions` → `UBLDocumentSignatures` block is missing. You're submitting v1.0 with v1.1 required, or didn't sign at all. |
| `DS302` | Signature count not equal to 1. | Exactly one `<ds:Signature>` is required. You either have zero or multiple. |
| `DS303` | Signing time not found. | `xades:SigningTime` is missing from `SignedSignatureProperties`. |
| `DS304` | Signing time is not within validity period. | Certificate has expired, or `SigningTime` is before `NotBefore` / after `NotAfter`. **Renew your cert.** |
| `DS305` | X509Certificate is not valid. | Cert is malformed, revoked, or can't be parsed. |
| `DS306` | SERIALNUMBER is empty in the certificate. | The X.509 Subject's `SERIALNUMBER` field is blank. Get a proper commercial cert — self-signed won't work. |
| `DS307` | OI is empty in the certificate. | Same as above, for `OI` (Organization Identifier). Required for organizational certs. |
| `DS308` | Incompatible certificate for the Submission Channel. **Organizational certificate cannot be used for Portal/Mobile submission.** | You're signing a Portal/Mobile submission with an org cert. Use a personal cert for Portal, org cert for ERP/API. |
| `DS309` | Incompatible certificate for the Submission Channel. **Personal certificate cannot be used for ERP submission.** | You're calling the API with a personal cert. **API submissions require an organizational cert.** |
| `DS310` | Signer of invoice doesn't match the submitter. UserLoginId doesn't match with the SERIALNUMBER. | The portal user and the cert Subject's SERIALNUMBER don't match. |
| `DS311` | Signer of invoice doesn't match the submitter. TIN doesn't match with the OI. | The authenticated TIN (from your client_id) and the TIN embedded in the cert's OI field differ. |
| `DS312` | Submitter registration/identity number doesn't match with the certificate SERIALNUMBER. | Portal / Mobile submission: logged-in user's IC doesn't match the cert's SERIALNUMBER. |
| `DS313` | Certificate digest algorithm doesn't match expected value of `http://www.w3.org/2001/04/xmlenc#sha256`. | You used SHA-1 or similar. MyInvois **requires SHA-256** for all digests. |
| `DS314` | Document digest method doesn't match expected value of `http://www.w3.org/2001/04/xmlenc#sha256`. | Same — SHA-256 only for the document digest. |
| `DS315` | Signed properties digest method doesn't match expected value of `http://www.w3.org/2001/04/xmlenc#sha256`. | Same — SHA-256 only for the signed-properties digest. |
| `DS316` | Signature method doesn't match supported method of `http://www.w3.org/2001/04/xmldsig-more#rsa-sha256`. | You used DSA, ECDSA, or RSA-SHA1. **Only RSA-SHA256 is accepted.** |
| `DS317` | Certificate digest is missing or empty. | `xades:CertDigest` → `ds:DigestValue` is blank. |
| `DS318` | Certificate digest value doesn't match digest calculated value from provided certificate. | You hashed the cert wrong, or swapped certs between sign-and-populate. Re-hash the exact DER bytes of the cert you're embedding. |
| `DS319` | Signed properties section digest is missing or empty. | The `<ds:Reference>` targeting `#id-xades-signed-props` has no `DigestValue`. |
| `DS320` | Signed properties digest value doesn't match digest calculated value from provided signed properties section where ID is id-xades-signed-props. | You hashed the signed-properties after changing it, or before linearizing/removing whitespace. Re-do Step 7. |
| `DS321` | Document digest is missing or empty. | The main document `<ds:Reference>` has no `DigestValue`. |
| `DS322` | Document digest value doesn't match digest calculated value from existing document content. Document content might have changed. | **The most common DS error after DS326.** You modified the document after hashing, or your canonicalization doesn't match. Apply c14n11 to XML / minify JSON, then hash, then don't touch the document. |
| `DS323` | Signature method used doesn't match the expected value of `urn:oasis:names:specification:ubl:dsig:enveloped:xades`. | Wrong `SignatureMethod` namespace. Must be the XAdES one above. |
| `DS324` | Certificate X509IssuerName is missing or empty in the signed properties section. | `xades:IssuerSerial` → `ds:X509IssuerName` is blank. |
| `DS325` | Certificate X509SerialNumber is missing or empty in the signed properties section. | Same, for the serial number. |
| `DS326` | Certificate X509IssuerName doesn't match the X509IssuerName value provided in the signed properties section. | **The single most reported DS error in community SDKs.** The issuer RDN sequence you wrote doesn't match what the cert actually has. See [DS326 deep-dive](#ds326-deep-dive) below. |
| `DS327` | Cannot parse the certificate X509SerialNumber value from the signed properties section. | You wrote the serial number in an unexpected format (hex vs decimal). |
| `DS328` | Certificate X509SerialNumber doesn't match the X509SerialNumber value provided in the signed properties section. | Value exists and parses, but is wrong. Copy it from the cert, not from memory. |
| `DS329` | Certificate is not valid according to the chain of trust validation or has been issued by an untrusted certificate authority. | Your CA isn't on the [MCMC licensed list](https://www.mcmc.gov.my/en/sectors/digital-signature/list-of-licensees). Only Malaysian-licensed CAs are accepted. |
| `DS330` | TIN provided cannot be found in Core system. | The TIN in the cert's OI isn't registered in IRBM's system. |
| `DS331` | Failed to parse input document. | Duplicate of DS300 with a slightly different trigger path. Same fix. |
| `DS332` | Document signature value is empty or doesn't exist. | `<ds:SignatureValue>` is blank. You ran Step 6 without Step 4. |
| `DS333` | Document signature value is not a valid signature of the document digest using the public key of the certificate provided. | Signed with one key, embedded a different cert's public key. Or the doc digest used for signing differs from the one in the final document. |
| `DS334` | Certificate cannot be used to validate the document signature value. | Cert is structurally valid but can't verify the signature (key usage restrictions, algorithm mismatch). |
| `DS335` | Failed to validate the document signature value. | Generic fallback when DS333/DS334 don't fit. Check the whole signing pipeline end-to-end. |
| `DS336` | Unable to parse Signing Time. Signing Time format is invalid. | `SigningTime` must be UTC ISO-8601 with `Z` suffix, e.g. `2024-04-01T00:41:21Z`. No offsets, no milliseconds variations that break parsers. |
| `DS337` | Unable to parse certificate SERIALNUMBER. Certificate SERIALNUMBER is invalid. | The Subject's SERIALNUMBER field has unexpected characters. |
| `DS338` | Unable to parse certificate OI. Certificate OI is invalid. | The Subject's OI field has unexpected characters. |

---

## DS326 deep-dive

DS326 is so common that the [klsheng PHP SDK](https://github.com/klsheng/myinvois-php-sdk)
has it in the main README as a known workaround.

### What's happening

XAdES requires the `X509IssuerName` in the signed properties to exactly match
the issuer DN of the embedded certificate. **The match is done by string, not
by structured RDN comparison**, so the order of the relative distinguished
names (RDNs) matters.

Different CAs and different libraries serialize issuer DNs in different orders.
For example:

- **Order A (common, e.g. PosDigicert sample):** `CN, E, OU, O, C`
- **Order B (other CAs / OpenSSL):** `C, O, OU, E, CN`

If your library renders the DN in order B but you hardcoded order A (or
vice versa), you get DS326 even though the *values* are identical.

### Fix (PHP SDK example)

The klsheng PHP SDK takes an optional 10th argument to `createJsonDocument` /
`createXmlDocument` that overrides the RDN sequence:

```php
// Default sequence → ['CN', 'E', 'OU', 'O', 'C']
$invoice = $example->createJsonDocument(
    InvoiceTypeCodes::INVOICE, $id, $supplier, $customer, $delivery,
    true, '/path/to/eInvoice.p12', null, 'passphrase',
    ['C', 'O', 'OU', 'E', 'CN']  // try this order if you get DS326
);
```

### Fix (general principle)

1. **Read the issuer DN directly from the cert**, don't reconstruct it.
2. Match the *exact* string format including spaces around `=` (some libs emit
   `CN=Foo`, others emit `CN = Foo` — MyInvois expects `CN = Foo` with spaces,
   per the [official sample](https://sdk.myinvois.hasil.gov.my/files/one-doc-signed.xml)).
3. If your lib won't let you control the order, try reversing it — most CAs
   publish in one of two orders and one of them will match.

### Example issuer name from the official sample
```
CN = Contoso Malaysia Sdn Bhd, E = noemail@contoso.com, OU = Contoso Malaysia Sdn Bhd, O = Contoso Malaysia Sdn Bhd, C = MY
```

Notice: **spaces around `=`**, **comma + space between RDNs**, and order is
`CN, E, OU, O, C`.

---

## Quick debug checklist (in order of "most likely")

When a DS3xx error hits, go through this list:

1. **DS322** (document digest mismatch) — 90% of first-time integrations.
   Check canonicalization: XML needs `xml-c14n11`, JSON needs minification
   (remove all whitespace, line breaks, comments).
2. **DS326** (issuer name mismatch) — try the reversed RDN order.
3. **DS309** — you used a personal cert for an API submission. Get an
   organizational cert.
4. **DS316** — algorithm namespace typo. Copy the exact URI:
   `http://www.w3.org/2001/04/xmldsig-more#rsa-sha256`
5. **DS320** — you modified the SignedProperties element after hashing it.
6. **DS311** — the TIN in your `client_id` doesn't match the cert's OI.
   Cert was issued to a different entity than the one you're authenticating as.
7. **DS304** — cert expired. Check `NotAfter` date.

## Canonicalization rules (for DS322 avoidance)

### XML
- Algorithm: `http://www.w3.org/2006/12/xml-c14n11`
- Apply to: the document *with* `UBLExtensions` and `cac:Signature` removed
  (per the two `not(//ancestor-or-self::...)` XPath transforms)
- Source must be UTF-8, no BOM, no XML declaration (`<?xml version="1.0"?>` removed)

### JSON
- Remove all whitespace (spaces, tabs, newlines)
- Remove all comments (`//`, `/* */`) — JSON doesn't have them but some libs embed them
- Do not reorder keys (order preservation is assumed)
- Use UTF-8 encoding

After canonicalization, hash with SHA-256, then base64-encode the **hash bytes**
(not hex-encode). A common mistake is hex-encoding first then base64 — that
produces a 88-char base64 of the 64-char hex string instead of the correct
44-char base64 of the 32-byte hash.

## Signing pipeline (8-step summary)

For reference, the full MyInvois signing sequence per the
[official guideline](https://sdk.myinvois.hasil.gov.my/signature/):

1. Build the UBL 2.1 document (no signature elements yet)
2. Apply transformations: remove `UBLExtensions` and `cac:Signature`
3. Canonicalize → hash → `DocDigest`
4. Sign the digest with RSA-SHA256 using your private key → `Sig`
5. Hash the signing certificate (the DER bytes) → `CertDigest`
6. Populate signed-properties (CertDigest, SigningTime, IssuerName, SerialNumber)
7. Linearize + hash signed-properties → `PropsDigest`
8. Write `SignatureValue`, `X509Certificate`, `PropsDigest`, `DocDigest` into the document

## Reference sample

The official sample signed document is at
<https://sdk.myinvois.hasil.gov.my/files/one-doc-signed.xml>.
Diff your output against it (after substituting your own cert/values) — if
your structure differs, fix structure before hashing.

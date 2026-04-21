# 11 — Digital Certificates

Every e-Invoice submitted via API must be digitally signed. This section
covers everything **before** the signing pipeline — how you acquire a cert,
which one to buy, what it must contain, and how to manage its lifecycle.

For signing errors (DS300–DS338), see [02-signature-errors.md](02-signature-errors.md).

## Why you need one

Per the General Guideline v4.6, Section 2.4.1.1:

> A digital certificate is a document (i.e., `.cer` or `.pfx`) that helps to
> verify the identity of the issuer issuing the e-Invoice. The digital
> signature will verify that the submitted e-Invoice originates from a specific
> taxpayer. The hashed value of the digital signature will be part of the
> e-Invoice API submission request body.

In plain English: **without a valid cert, you cannot submit via API.** MyInvois
Portal submissions don't require one (LHDN signs them for you) — but the
moment you go API, you need a cert.

## What type of certificate do I need?

**For API submissions: an organizational (business) digital certificate.**

| Certificate type | Allowed for API? |
|---|---|
| Organizational / Business cert | ✅ **Required** for API submission |
| Personal / Individual cert | ❌ Rejected with `DS309` on API |

From the official FAQ:
> Which digital certificate is required for document submission via API: an
> individual's or an organisation's? — **For submission through API, an
> organisation digital certificate is required.**

Personal certs are only for Portal/Mobile submission (and will be rejected
with `DS308` if used in the other direction).

## Soft vs Roaming certificates

Both types are accepted for MyInvois. Choose based on your deployment.

| Aspect | Soft Certificate | Roaming Certificate |
|---|---|---|
| **Where the key lives** | `.p12` / `.pfx` file on your ERP / middleware server | CA's Hardware Security Module (HSM) |
| **How you sign** | Your code loads the file and signs locally | Your code calls the CA's REST API to sign remotely |
| **Integration required?** | No — your ERP just reads the file | Yes — REST API integration with the CA |
| **Best for** | Direct taxpayers, in-house ERP | Service providers / intermediaries signing for many clients |
| **Security posture** | You protect the key file | CA protects the key, you hold API credentials |
| **If the key leaks** | Your responsibility; revoke + reissue | CA's problem primarily; still revoke |

### Rule of thumb

- **Just one company, one ERP, doing your own invoices** → **Soft cert**
- **You're a software vendor signing invoices on behalf of many clients** → **Roaming cert**
- **You're a large enterprise with strict key management policies** → **Roaming cert** (keys never touch your infra)

## Where to buy one — licensed Malaysian CAs

**You must use a CA licensed by MCMC** under the Digital Signature Act 1997.
A cert from a non-Malaysian CA (or Let's Encrypt, or self-signed) will fail
with **`DS329`** (chain of trust validation).

The official licensee list: <https://www.mcmc.gov.my/en/sectors/digital-signature/list-of-licensees>

The major CAs active in the e-Invoice space are below, with pricing confirmed
from their public pages (as of this writing, April 2026).

### Pos Digicert Sdn Bhd

Webpage: <https://www.posdigicert.com.my/digital-certificate-for-einvoice>

| Product | Validity | Price (excl SST) | Price (incl 8% SST) |
|---|---|---|---|
| Soft Certificate | 1 year | RM 1,500 | **RM 1,620** |
| Roaming Certificate | 1 year | RM 15,000 | **RM 16,200** |

**Process:**
1. Email `einvoice@posdigicert.com.my` for a quote.
2. Receive quote → issue PO / payment advice → fill application form.
3. Submit docs to Pos Digicert. Processing takes **3–5 working days**.
4. Admin contact on the application form will be contacted for verification.
5. Cert is issued and sent to the admin contact.
6. You configure it into your ERP.

**Support phone:** 03 8800 6009 / 6011 / 6013 / 6015 / 6035

### MSC Trustgate.com Sdn Bhd

Webpage: <https://www.msctrustgate.com/e-invoice>

| Product | Validity | Price |
|---|---|---|
| Soft Certificate | 1 year | **RM 1,500** |
| Soft Certificate | 2 years | **RM 3,000** (most popular per their page) |
| Roaming Certificate | 1 year | **RM 15,000** |

MSC Trustgate is the only CA publicly offering a **2-year soft cert** at a
flat double-the-1-year price — useful if you want to minimize renewal overhead.

**Contact:** `support@msctrustgate.com` / +603 8318 1800 / +603 8319 1800

**Resources published by MSC Trustgate:**
- User Guide: <https://www.msctrustgate.com/assets/pdf_msctrustgate/guide/einvoice/Signed_eInvoiceUserGuide_OrganizationCertificate_V1.5.pdf>
- Technical Doc for Roaming Certificate: <https://www.msctrustgate.com/assets/pdf_msctrustgate/guide/einvoice/eInvoice_TGAPI_RoamingCertificate.pdf>
- Cert Revocation Form: <https://www.msctrustgate.com/assets/pdf_msctrustgate/forms/DigitalCertificate_Revocation_Form_V1.pdf>

### Raffcomm Technologies Sdn Bhd (Rafftech)

Webpage: <https://www.rafftech.my/einvoice>

| Product | Validity | Price |
|---|---|---|
| Soft Certificate | 1 year | **RM 1,500** |
| Roaming Certificate | 1 year | **RM 15,000** |
| Date Time Stamping (TSA) | 1 year | **RM 10,000** |

**Unique to Rafftech:** a paid Time Stamping Authority (TSA) service. Most
e-Invoice signing doesn't require a separate TSA — the `SigningTime` in
XAdES is self-asserted by the signer — but some internal compliance regimes
want a third-party timestamp. Skip unless you specifically need it.

**Renewal reminders** are sent automatically: 2 months, 1 month, and 7 days
before expiry. Nice touch; don't rely on them alone — track expiry yourself.

**Contact:** +603 4040 0091 / email via website

### Telekom Applied Business Sdn Bhd (TM Applied Business)

Also on the MCMC licensed CA list and confirmed by Rafftech's own FAQ as one
of four CAs active in this space. Public e-Invoice pricing was not visible
at the time of writing — contact them directly for a quote.

### Other licensed CAs

Check the [full MCMC licensee list](https://www.mcmc.gov.my/en/sectors/digital-signature/list-of-licensees)
for any additional authorities. Pricing varies — always get a quote before committing.

### How the four CAs compare

| Criterion | Pos Digicert | MSC Trustgate | Rafftech | TM Applied Business |
|---|---|---|---|---|
| 1-year soft cert | RM 1,620 (incl SST) | RM 1,500 (excl SST) | RM 1,500 | contact for quote |
| 2-year soft cert | Not advertised | RM 3,000 | Not advertised | — |
| 1-year roaming | RM 16,200 | RM 15,000 | RM 15,000 | contact for quote |
| TSA service | — | — | RM 10,000/yr | — |
| Renewal reminders | Manual | Manual | Automatic (2mo / 1mo / 7d) | — |
| Lead time | 3–5 working days | Not stated | 3–5 working days | — |
| Published signing guide | Rev 1.2 (July 2024) | Org Cert User Guide v1.5 | Knowledge base | — |

All four are MCMC-licensed under the Digital Signature Act 1997. Choose on
price, support responsiveness, renewal handling, and whether you need a
2-year cert or TSA.

## What must be in the certificate

For MyInvois to accept your cert, these Subject fields are **mandatory**:

| Field | Description | Example value |
|---|---|---|
| **CN** (Common Name) | Organization name | `Pos Digicert Sdn Bhd` |
| **C** (Country) | 2-letter ISO country code | `MY` |
| **E** (Email) | Organizational email | `einvoice@yourco.com` |
| **O** (Organization) | Organization name | `Pos Digicert Sdn Bhd` |
| **Organization Identifier (OI)** | **Your TIN assigned by IRBM** | `C1041895804` |
| **Serial Number** | **Your BRN linked to the TIN above** | `199801001482` |

### What can go wrong

- **OI ≠ your TIN** → `DS311` ("TIN doesn't match with the OI") on every submission
- **Serial number ≠ your BRN** → `DS310` / `DS312`
- **OI / Serial Number empty** → `DS306` / `DS307` / `DS337` / `DS338`
- **Country is not MY** → CA shouldn't issue to you at all, but if one does, expect validation failures

**Double-check the cert after you receive it.** Ask the CA to re-issue if any
of these fields is wrong — it's free to fix before you deploy; painful after.

## Certificate validity & renewal

- **Most certs issued for 1 year.** MSC Trustgate offers 2-year.
- **The General Guideline mentions 3-year validity** as a nominal standard,
  but in practice CAs sell annual certs for cost reasons.
- **You must track expiry and renew before it hits.**

### Renewal gotchas

- **Start renewal 30+ days before expiry.** The verification process takes
  3–5 working days at Pos Digicert, and you don't want to be cutting it close.
- **Test the new cert in sandbox before switching production.** The renewed
  cert will have new `X509IssuerName` / `SerialNumber` values → if you've
  hardcoded any of these (don't do that, but people do), you'll break.
- **Document signing with an expired cert will get DS304** ("Signing time is
  not within validity period") — not retryable. Your submissions pile up.

### Revocation

If your private key leaks or a suspicious signature surfaces:

1. **Revoke immediately** via the CA's revocation form (MSC Trustgate has a
   published form; Pos Digicert requires email contact).
2. **Rotate client secrets** at MyInvois Portal too — don't assume the two
   are independent.
3. **Alert IRBM via their support channels** if you suspect the key was used
   to sign fraudulent invoices. They'll want the `correlationId` of any
   suspect submissions.

## The signing pipeline (summary)

Once you have the cert, the high-level flow is:

1. **Build the UBL 2.1 document** (no signature elements yet)
2. **Apply transforms** — remove `UBLExtensions` and `cac:Signature`
3. **Canonicalize → SHA-256 → Base64** → `DocDigest`
4. **Sign the digest** with RSA-SHA256 using the cert's private key → `Sig`
5. **SHA-256 the cert's DER bytes** → `CertDigest`
6. **Populate `SignedProperties`** (CertDigest, SigningTime UTC, IssuerName, SerialNumber)
7. **SHA-256 the `SignedProperties`** (after linearization + whitespace strip) → `PropsDigest`
8. **Embed everything** into the document

Any deviation causes a `DS3xx` error. See
[02-signature-errors.md](02-signature-errors.md) for the full table.

**Reference implementations** (study these before writing your own):
- PHP: [klsheng/myinvois-php-sdk](https://github.com/klsheng/myinvois-php-sdk) — handles `.p12` and `.crt`/`.key`
- C#: [mokth/einvoice](https://github.com/mokth/einvoice) — modular XAdES implementation
- Java sample code: in PosDigicert's PDF guideline (uses Apache XML Security)

## Cert storage best practices

### For soft certificates

- **Never commit to source control.** Use secrets management (HashiCorp Vault,
  AWS Secrets Manager, Azure Key Vault, even just `.env` files outside the repo).
- **Protect with the passphrase the CA provides,** or rotate to one of your
  own. Don't leave the passphrase blank.
- **File permissions:** `chmod 400` or equivalent. Only the ERP user needs to
  read it.
- **Backup the `.p12` to a secure location** — if your server dies and you
  don't have a backup, you're waiting another 3–5 days for re-issue plus lost
  submission time.

### For roaming certificates

- **Protect the API credentials the CA issued** like you'd protect a
  production database password.
- **IP-allowlist the CA's signing endpoint** at your firewall if possible.
- **Rate-limit your own calls** to the CA's signing API — they bill by usage.
- **Log every sign request** with correlation to the invoice — for audit.

## Quick FAQ

### "Do I need a cert per customer (if I'm an intermediary)?"
**No.** One organizational cert for your company works for signing all your
clients' e-Invoices. The `onbehalfof` header and the `Login as Intermediary`
API handle the per-client identity.

### "Can my holding company's cert be used for all subsidiaries?"
**Yes, via the intermediary model.** The holding company registers as an
intermediary and signs for each subsidiary. Alternatively, each subsidiary
can buy its own cert (RM 1,500 × N is a real cost consideration).

### "Does the cert work for both v1.0 and v1.1 document versions?"
**Yes.** v1.0 submissions don't require signature validation, but the cert is
used for the optional XML signing. v1.1 requires it.

### "Can a foreign company get a digital certificate for e-Invoicing?"
**Yes** — per Rafftech's public FAQ:
> A foreign company can get a digital certificate for e-invoicing, but it
> needs to obtain a TIN (Tax Identification Number) from IRBM as it is a
> required parameter in the certificate.

Order of operations for a foreign entity:
1. Register with IRBM and obtain a TIN.
2. Register with a Malaysian-licensed CA.
3. CA issues the cert with your TIN in the OI field.
4. Normal API integration from there.

### "Can I use one digital certificate to sign invoices for multiple clients?"
**Not unless you're an intermediary.** Per Rafftech's FAQ:
> Each company needs its own digital certificate for the submission of
> e-invoices. The certificate cannot be shared among multiple clients.

The **exception is the intermediary model** — a parent / holding company
that's registered as an intermediary can submit for its subsidiaries using
one cert (because it's authenticating as itself and acting `onbehalfof`
the subsidiary's TIN).

### "What if my MCMC-licensed CA disappears?"
Unlikely in practice, but: the cert remains valid until its expiry; your
submissions keep working. For renewal, migrate to another licensed CA.

### "Can I test with a self-signed cert in sandbox?"
**No.** Sandbox uses the same chain-of-trust validation as production. You'll
get `DS329`. Some CAs offer **sandbox-only certs** at reduced price — ask.

### "How do I set `X509IssuerName` to match what MyInvois expects?"
This is the `DS326` problem. See
[02-signature-errors.md § DS326 deep-dive](02-signature-errors.md#ds326-deep-dive).

## Reference

- [IRBM e-Invoice General Guideline v4.6 — Section 2.4.1.1](https://www.hasil.gov.my/en/e-invoice/reference-for-the-implementation-of-e-invoice/guidelines/)
- [Signature page (official SDK)](https://sdk.myinvois.hasil.gov.my/signature/)
- [PosDigicert e-Invoice info](https://www.posdigicert.com.my/digital-certificate-for-einvoice)
- [MSC Trustgate e-Invoice](https://www.msctrustgate.com/e-invoice)
- [MCMC licensed CAs](https://www.mcmc.gov.my/en/sectors/digital-signature/list-of-licensees)
- [Cleartax Malaysia — Digital Certificates guide](https://www.cleartax.com/my/en/digital-certificates-malaysia)
- [QuickBooks — e-Invoicing digital certificates guide](https://quickbooks.intuit.com/my/r/invoicing/guide-to-e-invoicing-digital-certificates/)

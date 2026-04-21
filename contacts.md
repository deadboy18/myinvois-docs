# IRBM / MyInvois Official Contacts

For when you need to raise a ticket, ask a policy question, or get support.
All the official channels in one place.

## LHDN / IRBM — Official e-Invoice support

| Channel | Details |
|---|---|
| **SDK / technical feedback** | <https://feedback.myinvois.hasil.gov.my> |
| **General e-Invoice questions** (email) | `myinvois@hasil.gov.my` |
| **e-Invoice microsite** | <https://www.hasil.gov.my/en/e-invoice/> |
| **Helpdesk hotline** | **03-8682 8000** |
| **HASiL Contact Centre** | 03-8911 1000 |

### When to use which

- **Stuck submission, specific error** → email `myinvois@hasil.gov.my` with
  your `correlationId`, TIN, and a description.
- **Feature request / SDK feedback** → feedback form.
- **Policy question** (am I in scope? is X exempt?) → contact centre.
- **Genuine outage / system-wide issue** → hotline.

### Information to include when raising a ticket

Always include:

1. **TIN** (your company's, or the taxpayer you're representing if intermediary)
2. **Environment** (Production or Sandbox / Pre-production)
3. **Correlation ID** from the failed response headers — **critical**
4. **Submission UID** (if applicable)
5. **Document UUID** (if applicable)
6. **Date/time of the event** (UTC)
7. **What you expected vs. what happened**
8. **Screenshots / raw response body** (redact any other PII)

A good template:

> **Subject:** e-Invoice submission failure — Error03 / DS302 — TIN C1234567890
>
> Hi IRBM support team,
>
> We're experiencing a recurring error on e-Invoice submissions.
>
> **TIN:** C1234567890
> **Environment:** Production
> **Submission UID:** SUB_20260421_001
> **Correlation ID:** abc123-def456-ghi789
> **Timestamp (UTC):** 2026-04-21T08:30:00Z
> **Error code:** Error03 → DS302
>
> The document is NOT a duplicate on our side — we've verified there's no
> prior submission with the same (type, version, date, number, supplier TIN).
>
> Could you please investigate?
>
> Thanks,
> [Your Name]
> [Your Company]

## MyInvois Portal / MyTax Portal

| Portal | URL | Use for |
|---|---|---|
| **Production MyInvois Portal** | <https://myinvois.hasil.gov.my/> | Manual invoice issuance, viewing validated invoices, registering ERP for prod |
| **Pre-production MyInvois Portal** | <https://preprod.myinvois.hasil.gov.my/> | Sandbox equivalent |
| **MyTax Portal** | <https://mytax.hasil.gov.my/> | TIN lookup, e-Daftar registration, authentication entry point |

## Certificate Authorities (active in e-Invoice space)

Per [11-digital-certificates.md](12-cookbook/11-digital-certificates.md), these four CAs
are the ones publicly advertising e-Invoice products. The full MCMC licensee
list may include others.

### Pos Digicert Sdn Bhd

| | |
|---|---|
| Website | <https://www.posdigicert.com.my/> |
| e-Invoice page | <https://www.posdigicert.com.my/digital-certificate-for-einvoice> |
| e-Invoice team phone | **03-8800 6009 / 6011 / 6013 / 6015 / 6035** |
| General phone | +603 8800 8008 |
| Order via | Email their e-Invoice team for quote |

### MSC Trustgate.com Sdn Bhd

| | |
|---|---|
| Website | <https://www.msctrustgate.com/> |
| e-Invoice page | <https://www.msctrustgate.com/e-invoice> |
| Support email | `support@msctrustgate.com` |
| Phone | +603 8318 1800 / +603 8319 1800 |
| Online purchase | <https://www.msctrustgate.com/mytrustid/enrollment?q=lhdn> |

### Raffcomm Technologies Sdn Bhd (Rafftech)

| | |
|---|---|
| Website | <https://www.rafftech.my/> |
| e-Invoice page | <https://www.rafftech.my/einvoice> |
| Phone | +603 4040 0091 |

### Telekom Applied Business Sdn Bhd (TM Applied Business)

Listed by Rafftech as an active e-Invoice CA. Full details from the MCMC
licensee list. Contact their corporate channels for e-Invoice quote.

### MCMC — Full list of licensees

Authoritative list of all CAs licensed under the Digital Signature Act 1997:
<https://www.mcmc.gov.my/en/sectors/digital-signature/list-of-licensees>

Always verify your CA choice against this list before purchasing.

## Community resources (unofficial)

These are community-run, not official LHDN channels. Useful for real-world
implementation experience.

| Resource | URL | Notes |
|---|---|---|
| Developer Kaki (FB group) | <https://www.facebook.com/groups/developerkaki/> | Malaysian developer community; e-Invoice is a recurring topic |
| klsheng/myinvois-php-sdk | <https://github.com/klsheng/myinvois-php-sdk> | Active community SDK, issues welcome |
| mokth/einvoice (C# samples) | <https://github.com/mokth/einvoice> | Author accepts coffee donations :) |

## Tax agents / implementation partners

For businesses that want someone else to handle this entirely:

- Your existing **tax agent** probably has an e-Invoice practice now.
- **Large accounting firms** (Big 4 and mid-tier) all have LHDN e-Invoice
  implementation teams.
- **ERP vendors** (SAP, Oracle, Microsoft, SQL Account, AutoCount, ABSS, etc.)
  typically offer e-Invoice modules or partner implementations.
- **Peppol access point providers** can handle the transmission layer for
  you.

This cookbook is not an endorsement of any specific implementation partner
— the right choice depends on your ERP, volume, and budget.

## Raising a bug / feature request with this cookbook

This cookbook is a community-maintained aggregation, not an official
publication. If you spot an error or have an addition:

- **Verified from official LHDN source** → submit a PR against the
  `myinvois-docs` repo hosting this cookbook
- **Error code you hit that isn't documented** → particularly valuable;
  include the full error response (redacted), your remediation, and the
  month you encountered it
- **New community SDK discovered** → PR to `community-sdks/README.md` with
  the repo link and your assessment

## Reference

- [SDK Contacts page](https://sdk.myinvois.hasil.gov.my/contacts/)
- [IRBM e-Invoice microsite](https://www.hasil.gov.my/en/e-invoice/)
- [MCMC licensees](https://www.mcmc.gov.my/en/sectors/digital-signature/list-of-licensees)

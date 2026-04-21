# ERPGulf/myinvois

**Language:** Python (Frappe framework)  
**License:** MIT  
**Repo:** <https://github.com/ERPGulf/myinvois>  
**Compatibility:** ERPNext 15 and 16  
**Current release:** 3.0.1 (Feb 2026)

A full-featured Frappe / ERPNext app for LHDN e-Invoicing compliance.
Not a standalone SDK — this is a drop-in ERPNext module. If you're on
Frappe / ERPNext, this is the obvious choice.

## Install

Requires an existing Frappe / ERPNext deployment.

```bash
# Get the app
bench get-app https://github.com/ERPGulf/myinvois.git

# Install on your site
bench --site yoursite.com install-app myinvois_erpgulf

# Run migrations
bench --site yoursite.com migrate

# Restart
bench restart
# or
sudo service supervisor restart
```

### Verify installation

1. Log in to ERPNext.
2. Help → About.
3. The LHDN app should be listed.

## What you get

### API integration

- Automatic access token retrieval and renewal.
- Seamless integration with LHDN APIs for **submission, validation, and
  cancellation**.
- Automatic submission to LHDN.

### Supported document types

- Standard Invoices
- Credit Notes
- Debit Notes
- Refund Notes
- Self-Billed Invoices
- Self-Billed Credit Notes
- Self-Billed Debit Notes
- Self-Billed Refund Notes

### Document version support

Both v1.0 and v1.1 are supported. v1.1 is the recommended default (includes
digital signature validation per LHDN's recommendation).

### Digital signature

- Secure digital certificate management for v1.1.
- XML signing for **both** versions.

### QR codes

- Automatic QR code generation.
- QR attached to the validated invoice record.

### Invoice consolidation (a specifically nice feature)

- **Merge multiple standard Sales Invoices** into one consolidated invoice.
- Generate valid XML for the consolidated document and submit to LHDN.
- QR code auto-assigned to the consolidated invoice.
- **Link-field-based audit trail** — the consolidated invoice retains
  references to its source invoices.

Consolidation is one of the trickier workflows to build from scratch, and
having it built in is a significant time saver for retailers or any business
that does end-of-day batch submissions.

### Reporting & dashboard

- **LHDN Dashboard System** for real-time insights.
- Reports comparing ERPNext invoices against the LHDN portal's view — catches
  divergences.
- Audit-trail logging on all submissions (success + error).

## Repo structure

```
myinvois/
├── myinvois_erpgulf/       (the actual Frappe app package)
├── .github/workflows/
├── pyproject.toml
├── README.md
├── license.txt
└── version-3.0.1md         (release notes)
```

The bulk of the code is in `myinvois_erpgulf/`. Languages:

- Python: 86.3%
- JavaScript: 13.4% (client-side DocType overrides)
- Roff: 0.3% (man pages / docs)

## When to pick this

✅ **You're running ERPNext.** Nothing else matters — this is the right pick.

✅ **You need consolidation.** If your business consolidates invoices
(supermarkets, retail, any POS-heavy business), the built-in workflow saves
weeks.

✅ **You want audit-trail logging built in.** Frappe's permission model +
the app's logging gives you compliance out of the box.

## When to skip this

❌ You're **not** on ERPNext / Frappe — the app is tightly coupled to that
framework. The MyInvois integration logic is worth studying as reference, but
you can't just lift it out cleanly.

❌ You need a lightweight library — this is a full app with DocTypes, UI, and
reports.

## Strengths

- ✅ Only community option designed as a full-stack compliance app
- ✅ Handles consolidation natively
- ✅ Actively maintained (254 commits, v3.0.1 in Feb 2026)
- ✅ Comparison reports (ERPNext vs LHDN) are a real-world need that nothing else addresses
- ✅ Built-in dashboard
- ✅ MIT licensed
- ✅ Tutorial video: <https://youtu.be/ExhjZv2zHaY>

## Weaknesses

- ❌ Frappe-specific — non-portable to other stacks
- ❌ Documentation is high-level / marketing-style; deep integration details
  aren't in the README
- ❌ v3.0.1 is labeled "future release tested for v16" — check compatibility
  with your ERPNext version
- ❌ No separate unit-test suite visible in the repo
- ❌ Support is via email (`support@ERPGulf.com`) — no active Discord / forum

## Strategy notes

If you're evaluating this vs. building on top of klsheng PHP SDK:

- **Already on ERPNext:** use ERPGulf, don't reinvent.
- **Building a custom compliance portal on PHP/Laravel:** klsheng PHP SDK +
  your own workflow on top.
- **Migrating from custom-built to ERPNext:** use ERPGulf and migrate the
  workflow gradually.

## Reference

- [Repo README](https://github.com/ERPGulf/myinvois)
- [Tutorial video](https://youtu.be/ExhjZv2zHaY)
- [Release 3.0.1](https://github.com/ERPGulf/myinvois/releases/tag/3.0.1)
- Support email: `support@ERPGulf.com`

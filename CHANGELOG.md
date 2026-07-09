# Changelog

All notable changes to this documentation repository will be tracked here.
Follow [Keep a Changelog](https://keepachangelog.com/en/1.1.0/) conventions.

## [1.1.0] — 2026-07-09

**Update for IRBM Guideline revisions published 7 July 2026.**

### Sources updated

- **IRBM e-Invoice General Guideline** — v4.6 → **v4.7** (published 7 July 2026)
- **IRBM e-Invoice Specific Guideline** — v4.7 → **v4.8** (published 7 July 2026)

### Changes

- **General Guideline v4.7** (minor clarifications):
  - Abbreviation table: added FTA (Free Trade Agreement) — p.4
  - Section 2.1 (e-Invoice Overview Workflow): additional clarification text — p.17
  - Appendix 1 & 2 (required fields / annexure fields): field description amendments — pp.43–54
- **Specific Guideline v4.8** (major addition):
  - **NEW Section 17: e-Invoice Special Voluntary Disclosure Programme (SVDP)** — pp.123–126
    - Penalty-free amnesty window: 7 July 2026 to 31 December 2027
    - Covers missed, incomplete, or non-compliant e-Invoices
    - Requires special version tags: "SVDP 1.2" (without digital signature) or "SVDP 1.3" (with digital signature)
    - Consolidated e-Invoices must be submitted month-by-month, not lumped
    - Three new worked examples (Examples 23, 24, 25)
  - Example 23 (p.124): missed consolidated e-Invoices — submit per month
  - Example 24 (p.125): mixed high/low-value transactions under SVDP
  - Example 25 (pp.125–126): self-billed catch-up after interim relaxation period

### Files modified

- `CHANGELOG.md` — this entry
- `README.md` — version badges, freshness section
- `11-irbm-specific-guideline/README.md` — added Section 17 (SVDP) + ToC entry
- `12-cookbook/irbm-general-guideline/README.md` — bumped to v4.7, updated summary-of-changes table
- `12-cookbook/10-implementation-timeline.md` — updated source reference to v4.7, added SVDP cross-reference
- `12-cookbook/13-required-fields-reference.md` — updated source reference to v4.7
- `12-cookbook/14-validation-status-lifecycle.md` — updated source references to v4.7

---

## [1.0.0] — 2026-04-21

**Initial public release.**

### Sources as of this release

- **MyInvois SDK portal** — scraped April 2026 from <https://sdk.myinvois.hasil.gov.my/>
- **IRBM e-Invoice General Guideline** — v4.6 (published 7 December 2025)
- **IRBM e-Invoice Specific Guideline** — v4.7
- **Digital Signature User Guide** (PosDigicert Rev 1.2, July 2024)
- **Community SDKs** surveyed at:
  - `klsheng/myinvois-php-sdk` v1.0.14
  - `syukranDev/e-invoice-sdk-nodejs` (25 commits)
  - `ERPGulf/myinvois` v3.0.1 (Frappe v16 compatible)
  - `mokth/einvoice` (181 commits)
  - `pyhoon/lhdn-einvoice-api-client-b4x` v1.10

### Contents

- 92 markdown files
- 44 sample XML/JSON invoice payloads (normal, foreign currency, consolidated,
  multi-line, self-billed variants; v1.0 + v1.1)
- 9 code-table JSON snapshots
- 1 official PDF (Digital Signature User Guide)
- Postman collection + sandbox/production environment files
- Complete DS300–DS338 signature error table with remediation
- Production readiness checklist (130+ items)
- Operations runbooks (cert expiry, compromise, system-down, stuck
  submissions, secret rotation, post-72h adjustments)
- 853 internal links — all verified

### Known limitations (captured in the README)

- `CF###` error codes are partially enumerated — LHDN doesn't publish a
  complete list
- Industry-specific catalogue PDFs (banking, insurance, healthcare, etc.)
  are referenced but not rendered in detail
- Snapshot semantics — downstream documents (release notes, code tables)
  change independently at LHDN

---

## How to cut a new release

1. Re-scrape the SDK portal with the scripts described in [CONTRIBUTING.md](CONTRIBUTING.md).
2. Diff against this repo to see what changed.
3. Update affected pages + add a new version entry here.
4. If a major LHDN publication drops (new Guideline version, new cert rule,
   major API change), bump the minor version.
5. Tag the release in Git: `git tag -a v1.1.0 -m "General Guideline v4.7" && git push --tags`.

Versioning scheme:
- **Major** — breaking restructure of this repo's navigation
- **Minor** — significant content update (new LHDN Guideline version, new
  endpoint documented, new SDK surveyed)
- **Patch** — corrections, clarifications, new community SDK versions

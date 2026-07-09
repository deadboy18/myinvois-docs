# E-Invoice Guideline (Version 4.7)

**Inland Revenue Board of Malaysia (IRBM / LHDN)**
Issued under Section 134A of the Income Tax Act 1967
**Date of publication: 7 July 2026**

This file is a markdown rendering of the official IRBM **General Guideline**
(not to be confused with the Specific Guideline which drills into per-scenario
rules). The General Guideline covers:

- What e-Invoice is, and what it is not
- Scope: who must comply
- The rollout timeline
- Exemptions
- The MyInvois Portal and API models
- Validation rules
- Pre-submission requirements (digital certificate, preparation)
- Complete field lists (Appendix 1 & 2)

For the Specific Guideline (industry scenarios, self-billed rules, etc.) see
the `11-irbm-specific-guideline/` folder in the parent `myinvois-docs` repo.

---

## Summary of changes from v4.6 → v4.7

| Paragraph (v4.6) | Paragraph (v4.7) | Change type | Notes |
|---|---|---|---|
| Abbreviation Table | Abbreviation Table | Addition | Added FTA (Free Trade Agreement) — p.4 |
| 2.1 | 2.1 | Addition | Clarification of e-Invoice Overview Workflow — p.17 |
| Appendix 1, Appendix 2 | Appendix 1, Appendix 2 | Amendment | Field description clarifications — pp.43–54 |

### Previous changes (v4.5 → v4.6)

| Paragraph (v4.5) | Change in v4.6 | Notes |
|---|---|---|
| 1.0 | Addition | Clarification |
| 1.5 | Amendment | Clarification |
| 1.6.1(e) | Amendment | Clarification |

---

## Abbreviations

| Term | Definition |
|---|---|
| API | Application Programming Interface |
| B2B | Business-to-Business |
| B2C | Business-to-Consumer |
| B2G | Business-to-Government |
| ERP | Enterprise Resource Planning |
| FTA | Free Trade Agreement |
| IRBM | Inland Revenue Board of Malaysia |
| JSON | JavaScript Object Notation |
| MSIC | Malaysia Standard Industrial Classification |
| MSME | Micro, Small and Medium-sized Enterprises |
| SDK | Software Development Kit |
| SSM | Companies Commission of Malaysia |
| SST | Sales Tax and Service Tax |
| TIN | Tax Identification Number |
| UBL 2.1 | Universal Business Language Version 2.1 |
| XML | Extensible Markup Language |

---

## 1.0 Introduction

Malaysia's e-Invoice initiative is a phased government effort to modernize
tax administration, aligned with the Twelfth and Thirteenth Malaysia Plans.

**e-Invoice enables near real-time validation and storage of transactions**
for B2B, B2C, and B2G flows.

### 1.1 About e-Invoice

An e-Invoice is a **digital representation of a transaction** between a
supplier and a buyer. It replaces paper or electronic documents such as
invoices, credit notes, and debit notes.

**An e-Invoice IS:**
- A file in the format specified by IRBM — **XML or JSON** — that can be
  automatically processed by relevant systems.

**An e-Invoice IS NOT:**
- PDF, DOC, JPG
- A bill image attached to an email
- A scan of a paper invoice

The content is largely the same as a traditional document: supplier and buyer
details, item descriptions, quantities, prices, tax, totals. It's the
**format and the digital flow** that's new.

### 1.2 Benefits

1. **Unified invoicing process** — automation reduces manual effort and human error.
2. **Easier tax return filing** — seamless system integration for accurate reporting.
3. **For larger businesses** — streamlined operations, time/cost savings.
4. **For MSMEs** — phased, manageable transition.

### 1.3 Transaction Types

e-Invoice covers **B2B, B2C, and B2G**. B2G flows work like B2B.

**All individuals and legal entities** in Malaysia are required to comply,
including:

1. Association
2. Body of persons
3. Branch
4. Business trust
5. Co-operative societies
6. Corporations
7. Limited liability partnership
8. Partnership
9. Property trust fund
10. Property trust
11. Real estate investment trust
12. Representative office and regional office
13. Trust body
14. Unit trust

### 1.4 Scenarios and Types of e-Invoices

**Scenarios requiring an e-Invoice:**

- **Proof of Income** — issued whenever a sale or transaction is made to
  recognize income.
- **Proof of Expense** — covers purchases or spending. Includes returns,
  discounts. Can also be used to correct or subtract an income receipt.
  Includes self-billed e-Invoice for certain scenarios (e.g. foreign
  supplier transactions).

**Types of e-Invoices:**

| Type | Purpose |
|---|---|
| **Invoice** | Commercial document itemizing a transaction. Includes self-billed e-Invoice for expenses. |
| **Credit Note** | Issued to correct errors, apply discounts, account for returns. **Reduces** value. Used when no monies are returned to Buyer. |
| **Debit Note** | Adds additional charges to a previously issued e-Invoice. |
| **Refund Note** | Confirms actual refund of Buyer's payment. Used when monies are returned to Buyer. |

### 1.5 Implementation Timeline

| Phase | Targeted Taxpayers | Implementation Date |
|---|---|---|
| 1 | Annual turnover / revenue **> RM 100 million** | **1 August 2024** |
| 2 | Annual turnover / revenue **> RM 25 million and ≤ RM 100 million** | **1 January 2025** |
| 3 | Annual turnover / revenue **> RM 5 million and ≤ RM 25 million** | **1 July 2025** |
| 4 | Annual turnover / revenue **up to RM 5 million** | **1 January 2026** |

**Basis for determining annual turnover:**

1. **With audited financial statements:** from FY2022 statement of comprehensive income.
2. **Without audited financial statements:** from YA2022 tax return.
3. **If you changed accounting year-end in FY2022:** pro-rate to 12 months.

**Once determined, your deadline doesn't change** regardless of later revenue changes.

**Voluntary early adoption** is permitted for any taxpayer regardless of revenue.

**New businesses (commenced 2023–2025)** with annual turnover ≥ RM 1M:
mandatory from **1 July 2026**.

**New businesses commencing 2026+:** mandatory from 1 July 2026 or commencement date. If first-year revenue < RM 1M, deadline is 1 January of the second year following the year you crossed RM 1M.

### 1.6 Exemptions

#### 1.6.1 Exempted persons (entities / individuals)

(a) Foreign diplomatic office
(b) Individual not conducting business
(c) Statutory body, statutory authority, local authority — exempt only for:
    - Collection of payment, fee, charge, statutory levy, summon, compound
      and penalty under any written law
    - Goods sold / services performed **before 1 July 2025**
(d) International organization — exempt for goods sold / services performed
    **before 1 July 2025** (see [full list](../10-implementation-timeline.md#list-of-exempted-international-organisations))
(e) Taxpayers with annual turnover / revenue **< RM 1,000,000**

#### 1.6.2 Consequences

These exempted persons are not required to issue e-Invoice (including self-billed).
For tax purposes, their existing receipts serve as proof of expense.

#### 1.6.3 Suppliers to these persons

**Suppliers to exempt parties still have to issue e-Invoice.** For diplomatic
offices, suppliers may replace Buyer's details per Table 3.5 of the Specific Guideline.

#### 1.6.4 Exemption scope

Exemption only covers the exempted **person**. Any entity (company, LLP, etc.)
owned by them must still implement e-Invoice.

#### 1.6.5 Voluntary implementation

Exempt persons may voluntarily implement e-Invoice.

#### 1.6.7 Exempt income/expense types

e-Invoice (including self-billed) NOT required for:

(a) Employment income
(b) Pension
(c) Alimony
(d) Distribution of dividend (specific circumstances — Section 11 of Specific Guideline)
(e) Zakat
(f) Securities / derivatives traded on a stock exchange (Malaysia or elsewhere)
(g) Disposal of unlisted company shares — EXCEPT where disposer is a company, LLP, trust body, or co-operative society
(h) Donations / contributions received (per FAQ Part A, Question 1 — Donations)

#### 1.6.8 Review cycle

Exemptions are reviewed and updated from time to time.

---

## 2.0 Getting Ready for e-Invoice

### 2.1 Overview Workflow

**Typical flow:**

1. Supplier makes a sale → creates e-Invoice → submits via Portal or API
2. IRBM validates in near real-time
3. IRBM notifies Supplier + Buyer
4. Supplier shares validated e-Invoice / visual representation with Buyer
5. Rejection or cancellation (stipulated 72-hour window)
6. IRBM stores validated e-Invoices
7. Reporting & dashboards available to both parties

### 2.2 e-Invoice Model — Two transmission mechanisms

| # | Mechanism | Key Features | Best For |
|---|---|---|---|
| 1 | **MyInvois Portal** (Section 2.3) | Individual form OR batch spreadsheet upload. Accessible to all taxpayers. | Businesses without API integration |
| 2 | **API** (Section 2.4) | High-volume transmission. Direct ERP integration, Peppol providers, or non-Peppol tech providers | Large taxpayers, high volumes |

---

### 2.3 e-Invoice Model via MyInvois Portal

Access via [MyTax Portal](https://mytax.hasil.gov.my).

#### 2.3.1 Pre-Submission

**Retrieve / verify TIN:**
1. Check via MyTax Portal, OR
2. Register via e-Daftar:
   - Log in to MyTax Portal
   - Choose e-Daftar
   - Fill required fields (type, email, phone)
   - Click Search to register and obtain TIN

#### 2.3.2 Step 1 — Creation and Submission

Two options:
1. **Individual Creation** — fill form with required fields.
2. **Batch Upload** — upload pre-defined Excel spreadsheet.

See [Appendix 1](#appendix-1--list-of-required-fields-for-e-invoice) for full field list.

#### 2.3.3 Step 2 — Validation

Validation happens in near real-time. On success, Supplier receives:
- Validated e-Invoice
- Visual representation (PDF)
- IRBM Unique Identifier Number
- Date and time of validation
- Validation link

On failure, an error message is displayed. Supplier must correct and resubmit.

#### 2.3.4 Step 3 — Notification

Both Supplier and Buyer are notified via portal and email. Notifications include:
- Invoice clearance
- Buyer rejection requests

#### 2.3.5 Step 4 — Sharing

Supplier obligated to share validated e-Invoice with Buyer. Visual representation
includes a QR code that validates existence + status via MyInvois Portal.

**Concession:** Supplier may share either the validated e-Invoice OR visual
representation (until further notice).

#### 2.3.6 Steps 5, 6, 7 — Rejection or Cancellation

**Buyer rejection request:**
- Must be within 72 hours of validation
- Must specify reason (erroneous information — SST #, BRN, etc.)
- Notification sent to Supplier on request
- If Supplier agrees: Supplier cancels within 72 hours
- If Supplier doesn't agree / doesn't cancel: no cancellation after 72hr;
  adjustments require new credit/debit/refund note

**Supplier cancellation:**
- Within 72 hours of validation
- Must include justification
- Notification sent to Buyer

**Past 72 hours:** no cancellation possible. Adjustments via new
credit/debit/refund note.

#### 2.3.7 Step 8 — Storing

All validated e-Invoices stored in IRBM's database. **Taxpayers must still
retain their own records** (Income Tax Act 1967 requirement).

#### 2.3.8 Step 9 — Reporting & Dashboard

MyInvois Portal provides retrieval in formats:
1. XML / JSON (individual or package)
2. Metadata
3. Grid
4. PDF

---

### 2.4 e-Invoice Model via API

Methods:
1. Direct ERP integration with MyInvois
2. Through Peppol service providers
3. Through non-Peppol technology providers

API integration & endpoints details: [SDK](https://sdk.myinvois.hasil.gov.my/).

**Supported formats:**
- **XML** — strict syntax, widely used, reliable automated processing
- **JSON** — lightweight, derived from JavaScript, language-independent

Both adhere to **UBL 2.1** data structure.

**55 data fields** grouped into 8 categories:
1. Address
2. Business Details
3. Contact Number
4. Invoice Details
5. Parties
6. Party Details
7. Payment Info
8. Products / Services

See [Appendix 1](#appendix-1--list-of-required-fields-for-e-invoice) for full
list.

#### 2.4.1 Pre-Submission

**2.4.1.1 Digital Certificate**

A digital certificate (`.cer` or `.pfx`) verifies the issuer's identity. The
digital signature's hashed value is part of the API submission request body.

See [11-digital-certificates.md](../11-digital-certificates.md) for full
procurement and usage guidance.

**2.4.1.2 e-Invoice Preparation**

Configure your system (or engage a technology provider) to generate e-Invoices
in XML or JSON matching the defined structure.

#### 2.4.2 Step 1 — Submission

Supplier (or tech provider) creates e-Invoice per UBL 2.1 in XML/JSON,
submits via API.

#### 2.4.3 Step 2 — Validation

On success, API response returns:
- IRBM Unique Identifier Number
- Date and time of validation
- Information to form validation link (see SDK Get Recent / Get Document / Get Document Details)

On failure, API error response. Notification sent to Supplier and Buyer on success.

#### 2.4.4 Step 3 — Sharing

Same as Portal model. Supplier ensures QR code is embedded if sharing visual representation.

#### 2.4.5 Steps 4, 5, 6 — Rejection and Cancellation

Same 72-hour rules as Portal model, with API operations:

**Buyer rejection via API:**
- Request body must contain: unique identifier of e-Invoice + rejection reason
- Notification sent to Supplier
- If Supplier agrees: Supplier cancels via API within 72 hours

**Supplier cancellation via API:**
- Request body must contain: unique identifier of e-Invoice
- Notification sent to Buyer
- May need to issue new e-Invoice if applicable

#### 2.4.6 Step 7 — Storing

Same as Portal model.

#### 2.4.7 Step 8 — Reporting

API integration provides:
1. XML / JSON (individual or package)
2. Metadata

---

### 2.5 Validation

#### 2.5.1 What's validated

Series of validations for data field requirements, formats, standards.

#### 2.5.2 e-Invoice statuses

| Status | Meaning |
|---|---|
| **Submitted** | Transmitted to MyInvois, passed immediate validations (structure + core fields), background validations pending |
| **Valid** | All validations passed |
| **Invalid** | One or more validations failed |
| **Cancelled** | Cancelled by Supplier within 72 hours; no longer valid |

See [14-validation-status-lifecycle.md](../14-validation-status-lifecycle.md)
for detailed state machine.

#### 2.5.3 Validators (Table 2.2)

| # | Validator | Type | What it does |
|---|---|---|---|
| 1 | Structure | Immediate | Checks XML/JSON structure matches required UBL 2.1 for the doc type + version |
| 2 | Core Fields | Immediate | Checks mandatory data fields are present |
| 3 | Signature | Background | Validates digital signature |
| 4 | Taxpayer | Background | Validates referenced taxpayers are valid at issuance date |
| 5 | Referenced Documents | Background | Validates credit/debit/refund notes reference valid originals |
| 6 | Code | Immediate + Background | Validates currency, tax types, etc. |
| 7 | Duplicate Document | Background | Flags potential duplicates |

See [Document Validation Rules (SDK)](https://sdk.myinvois.hasil.gov.my/document-validation-rules/).

#### 2.5.4 System Downtime Concession

Key operational clause:

> In cases where the MyInvois System is down due to maintenance or technical
> issues, and taxpayers are able to demonstrate their evidence of their
> e-Invoice compliance efforts, the Director General of Inland Revenue will
> evaluate this on a case-to-case basis. If the justifications are valid,
> the Director General will not take action against the taxpayers for their
> inability to comply with the e-Invoice transmission and validation
> requirements during such period.

**Keep submission logs + evidence** — you may need them if MyInvois has
extended downtime.

### 2.6 Sharing with Royal Malaysian Customs Department (RMCD)

- e-Invoice requirements consider Income Tax Act 1967, Labuan Business
  Activity Tax Act 1990, Petroleum (Income Tax) Act 1967, Sales Tax Act 2018,
  Service Tax Act 2018.
- **Pursuant to Section 138(4)(aa) of the Income Tax Act 1967, e-Invoice
  information submitted will be shared with RMCD.**
- Taxpayers may adopt any visual representation format, but should include
  particulars required by applicable laws (e.g. Service Tax Regulations 2018).

---

## 3.0 Data Security and Privacy Monitoring by IRBM

MyInvois System designed with security monitoring tools. Key activities:

1. **Assess data protection needs** — identify data types, legal obligations,
   policies.
2. **Implement controls** — encryption, authentication, access control,
   backup, firewall, antivirus, logging.
3. **Monitor and audit** — performance benchmarking, reporting, investigating
   incidents.
4. **Continuous improvement** — review and update based on findings.

---

## 4.0 Assessing Readiness

Key steps:

1. **Allocate & equip personnel** with capabilities to adopt and oversee
   implementation.
2. **Determine data sources, IT capabilities, processes** for compliance.
3. **Review current processes** for issuing invoices, debit notes, credit
   notes, refund notes.

---

## Appendix 1 — List of Required Fields for e-Invoice

See [13-required-fields-reference.md](../13-required-fields-reference.md) for
the complete list with explanations and validation rules.

**55 fields across 8 categories:**

| Category | Field range |
|---|---|
| Parties | #1–2 |
| Supplier's Details | #3–9 |
| Buyer's Details | #10–13 |
| Address | #14–15 |
| Contact Number | #16–17 |
| Invoice Details | #18–27 |
| Products / Services | #28–48 |
| Payment Info | #49–55 |

---

## Appendix 2 — Annexure Fields

**Mandatory Annexure fields** (applicable to import/export of goods):
- Reference Number of Customs Form No.1, 9, etc.

**Optional Annexure fields** (applicable to ship-to-different-recipient):
- Shipping Recipient's Name / Address / TIN / Registration or ID or Passport Number

**Optional Annexure fields** (applicable to import/export of goods):
- Incoterms
- Product Tariff Code (HS code)
- FTA Information (export only, if applicable)
- Authorisation Number for Certified Exporter (e.g. ATIGA)
- Reference Number of Customs Form No.2
- Country of Origin
- Details of other charges

**Note:** Annexure field requirements may be updated from time to time.

---

## Appendix 3 — List of Exempted International Organisations

See [10-implementation-timeline.md § List of exempted international organisations](../10-implementation-timeline.md#list-of-exempted-international-organisations)
for the full 41-entry list.

---

## Glossary

| Term | Definition |
|---|---|
| **API** | Collection of rules/protocols facilitating communication between apps |
| **B2B / B2C / B2G** | Business-to-Business / Business-to-Consumer / Business-to-Government |
| **Credit Note** | Issued to reduce original e-Invoice value (returns, discounts). Must reference the associated invoice(s). |
| **Debit Note** | Adds to original amount (additional services, shipping, etc.). Must reference associated invoice(s). |
| **Refund Note** | Acknowledges actual return of monies to Buyer. |
| **Invoice** | Standard commercial document recording a transaction. |
| **ERP** | Enterprise Resource Planning software |
| **MSME** | Micro, Small and Medium-sized Enterprises |
| **MyInvois Portal** | IRBM's web app for e-Invoice actions |
| **MyInvois System** | IRBM's e-invoicing system |
| **QR code** | Encoded data for locator/reference + identifier |
| **SGML (ISO 8879)** | Standard Generalized Markup Language |
| **SST** | Sales Tax (Sales Tax Act 2018) or Service Tax (Service Tax Act 2018) |
| **UBL 2.1** | Universal Business Language Version 2.1 — standard XML business documents |
| **Validation** | MyInvois evaluating/verifying submitted e-Invoice |
| **XML** | Extensible Markup Language |

---

## Reference

**Official source:**
- [IRBM e-Invoice guidelines page](https://www.hasil.gov.my/en/e-invoice/reference-for-the-implementation-of-e-invoice/guidelines/)
- [IRBM e-Invoice implementation page](https://www.hasil.gov.my/en/e-invoice/implementation-of-e-invoicing-in-malaysia/)
- [MyInvois SDK portal](https://sdk.myinvois.hasil.gov.my/)

**This General Guideline supplements:**
- The **Specific Guideline** (industry scenarios, self-billed details, etc.)
  — see `11-irbm-specific-guideline/` in parent `myinvois-docs` repo
- The **SDK documentation** — technical integration details
- **FAQ pages** at <https://sdk.myinvois.hasil.gov.my/faq/>

# Real-World e-Invoice Lessons (Hotel PMS Implementation)

Field notes from a 16-month production deployment at a Malaysian hotel using the MyInvois API. These are errors, fixes, and patterns discovered through trial and error that aren't covered in the official SDK documentation.

---

## 1. General Public TIN requires Classification Code 004

**Error:** Submission rejected when using General Public TIN `EI00000000010` with classification code `022`.

**Root cause:** Only classification codes `004` (consolidated) and `022` (certain scenarios) work with general TINs. For General Public TIN with `NA` as the ID value, classification code **must** be `004`.

**Fix:** Set `ItemClassificationCode` to `004` whenever using any of the three default TIN numbers:
- `EI00000000010` (General Public)
- `EI00000000020` (Foreign Buyers)
- `EI00000000040` (Government)

**Reference error:**
> "ERR205 where general TIN provided, NA is not allowed"

This error occurs when General TIN and ID Type "NA" are used for **individual** e-Invoice submissions. This combination is only permitted for **consolidated** e-Invoice submissions with classification code `004`.

---

## 2. CF349: Contact Number Required for Buyer

**Error code:** `CF349`
**Message:** `Contact number is required - BUYER`
**Path:** `Invoice.AccountingCustomerParty.Party.Contact.Telephone`

**When it happens:** Government agencies (Jabatan, Kementerian) or companies where no phone number was entered in the client profile.

**Fix:** Always populate the buyer phone number. For government entities, look up their publicly listed number. The field cannot be empty or null.

**Affected clients in practice:** JABATAN PERDANA MENTERI, JABATAN KERJA RAYA, KEMENTERIAN PENDIDIKAN, MALAYSIAN INVESTMENT DEVELOPMENT AUTHORITY, etc.

---

## 3. Government TIN (EI00000000040) Usage

Government agencies use a separate General TIN: `EI00000000040`

**How to identify government clients in your system:**
- Name starts with "JABATAN" (Department)
- Name starts with "KEMENTERIAN" (Ministry)
- Name contains "POLIS DIRAJA" (Royal Police)
- Other statutory bodies

**Important:** Not all organizations with official-sounding names are government. Example: "MALAYSIAN ASSOCIATION OF HOTELS (MAH)" is a private association, not government. Verify before assigning the government TIN.

---

## 4. F-Type TIN Requires BRN

**Error:** 400 Bad Request with null payload when submitting with an F-prefix TIN (e.g., `F26412625040` for foundations/yayasan).

**Root cause:** Non-public/non-government TINs require a valid BRN in the `PartyIdentification` field. The JSON payload becomes malformed when BRN is missing because the system expects an additional identifier.

**Fix:** Always pair non-general TINs with a valid BRN. Use the Search TIN API to find the correct BRN if the client hasn't provided one.

---

## 5. Empty String Validation (August 2026 Silent Change)

**Date discovered:** 24 August 2026
**Last working:** 13 August 2026

LHDN silently tightened server-side validation. Previously accepted empty strings in optional date/time fields now cause rejection.

**Affected fields:**
- `InvoicePeriod.StartDate` - needs `YYYY-MM-DD`, was accepting `""`
- `InvoicePeriod.EndDate` - needs `YYYY-MM-DD`, was accepting `""`
- `PrepaidPayment.PaidDate` - needs `YYYY-MM-DD`, was accepting `""`
- `PrepaidPayment.PaidTime` - needs `HH:MM:SSZ`, was accepting `""`

**Fix:** When these fields have no data, **omit the entire block** from the JSON. Both `InvoicePeriod` and `PrepaidPayment` are optional (cardinality `[0..1]`). Do not include them with empty values.

**Error message:**
```
ArrayItemNotValid: #/Invoice[0].InvoicePeriod[0]
  DateExpected: #/Invoice[0].InvoicePeriod[0].StartDate[0]._
```

---

## 6. "NA" Discontinued for BRN (August 2026)

**Effective:** 1 August 2026

LHDN now cross-validates TIN with BRN. The string `"NA"` is no longer accepted in the BRN/NRIC field except for specific General TIN scenarios.

**Rules:**
- General Public TIN (`EI00000000010`) + Classification `004`: `"NA"` is still allowed
- Any real TIN (C-prefix, F-prefix, etc.): Must provide actual BRN or NRIC
- Government TIN (`EI00000000040`) + Classification `004`: `"NA"` is still allowed
- For TIN provided without NRIC: use `000000000000` instead of `"NA"`

---

## 7. UBLExtensions and Signature Rejection (v1.1)

**Error:**
```
NoAdditionalPropertiesAllowed: #/UBLExtensions
NoAdditionalPropertiesAllowed: #/Signature
```

**Root cause:** Including `UBLExtensions` and `Signature` blocks in the JSON payload when not using a valid purchased digital certificate.

**Fix:** Remove both `UBLExtensions` and `Signature` root-level blocks entirely from the JSON. These are only valid when using a properly purchased digital certificate (soft cert RM1,500/year or roaming cert RM15,000/year). Self-signed certificates do not work.

**Version note:** You can still submit using document type version `1.0` (without signature) indefinitely. Version `1.1` (with signature) requires a valid certificate from an MCMC-licensed CA.

---

## 8. Duplicate Invoice Numbers Accepted

LHDN does **not** validate duplicate invoice numbers. You can submit the same invoice number multiple times and all will be marked as Valid.

**Impact:** Your system must implement its own duplicate detection. Without it, re-submissions (due to network issues, retries, or user error) will create duplicate tax records.

**Recommended approach:** Check submission status before re-submitting. Track `submissionUid` and `uuid` for each invoice.

---

## 9. API Key Expiry Error

**Error response:**
```json
{"code":"Invalid","message":"Unable to obtain valid MyInvois token","target":"GET TOKEN"}
```

**Root cause:** Client Secret has expired. This is not documented in the SDK error reference.

**Fix:** Generate new Client Secret from the MyInvois portal. Production keys can be set for up to 3 years.

---

## 10. Credit Note Workflow (After 72 Hours)

When an invoice needs correction after the 72-hour cancellation window:

1. Create a Credit Note (document type `12`) referencing the original invoice UUID
2. Submit the Credit Note to LHDN (it must include the original invoice reference)
3. Create a new corrected invoice with the right details
4. Submit the new invoice

**Important:** The Credit Note line items must match the original invoice exactly so the tax ledger balances to zero.

**Where to submit from:** If the regular tax submission page doesn't show credit memos, use the Income Audit > Invoice tab which has a separate "Send to Tax Interface" button.

---

## 11. Rate Limit Increases (Unofficial, May 2025)

Production-only changes (not reflected in SDK documentation):
- Login API: 12 RPM increased to 400 RPM
- Submit Documents API: 100 RPM increased to 400 RPM
- Get Submission API: 300 RPM increased to 1,200 RPM

**Source:** Insider information, not officially announced. Sandbox limits remain unchanged.

**HTTP 429 handling:** Even with increased limits, implement exponential backoff. Validation times have increased to 10-30 minutes during peak periods.

---

## 12. Batch Submission Practical Limits

While the API doesn't specify a batch size limit, real-world testing shows:

- **Under 200 invoices:** Reliable, completes without issues
- **200-500 invoices:** Generally works, occasional timeout on verification URL retrieval
- **500-1000 invoices:** Frequent "connection lost" errors, but submissions often still process in the background
- **1000+ invoices:** High failure rate with HTTP 504 Gateway Timeout from middleware

**Recommended approach:** Submit in batches of 200 or fewer. After submission, retrieve verification URLs separately if they timeout during the initial request.

---

## 13. QR Code Validation URL Construction

The validation URL (for QR codes on receipts) follows this format:

```
{base_url}/{uuid}/share/{longId}
```

**Production:** `https://myinvois.hasil.gov.my/{uuid}/share/{longId}`
**Sandbox:** `https://preprod.myinvois.hasil.gov.my/{uuid}/share/{longId}`

Both `uuid` and `longId` come from the Get Submission or Get Document Details API response.

**Timing note:** The `longId` may not be available immediately after submission. Implement a polling mechanism with 15-second intervals to retrieve it.

---

## 14. OTA Bookings and e-Invoice Responsibility

For hotel bookings through OTAs (Agoda, Booking.com, etc.) under the agency model:

**Transaction 1 (Hotel to Guest):** The hotel is the supplier. Issue e-Invoice for the full room price. If the guest requests an individual e-Invoice, use their details. Otherwise, include in the monthly consolidated e-Invoice.

**Transaction 2 (Agoda/OTA Commission):** This is a separate transaction. The hotel issues a **self-billed e-Invoice** for the commission paid to the OTA. Use foreign TIN `EI00000000020` if the OTA doesn't have a Malaysian TIN.

**Not confirmed officially.** Consult your tax advisor.

---

## 15. Digital Certificate Options

| Type | Price/year | Location | Use case |
|------|-----------|----------|----------|
| Soft Certificate | RM 1,620 (incl. 8% SST) | Installed on your server (.p12 file) | Single taxpayer, in-house system |
| Roaming Certificate | RM 16,200 (incl. 8% SST) | Stored at CA's HSM, accessed via API | Intermediaries serving multiple clients |

**Vendors:** PosDigicert, MSC Trustgate, Rafftech, TM Applied (all MCMC-licensed)

**Important:** Self-signed certificates do NOT work for v1.1 submissions. Someone has tested this and confirmed it fails.

---

## 16. LHDN Portal Print Limitation

Documents submitted via API **cannot** be printed from the MyInvois portal. The print/download PDF function only works for documents created directly through the portal UI.

For API-submitted documents, you must generate your own PDF using the data from the Get Document Details API response.

---

## 17. Buyer Name for General Public TIN

When using General Public TIN `EI00000000010`, the buyer's `RegistrationName` should be set to `"General Public"`, not the guest's actual name or the business name.

---

## 18. LHDN Data Breach (July 2026)

In July 2026, approximately 10 million taxpayer records were reportedly compromised through an enumeration attack on an API endpoint that lacked rate limiting. Exposed data allegedly included NRIC numbers, TIN numbers, addresses, bank accounts, and employer data.

**Implication for developers:** Do not assume that a TIN or NRIC is a secret value. Implement your own access controls on stored taxpayer data.

---

## 19. Sandbox vs Production Differences

| Aspect | Sandbox | Production |
|--------|---------|------------|
| Rate limits | Stricter (3-second intervals) | More generous (400 RPM for submission) |
| Validation speed | Faster (seconds) | Slower (10-30 minutes during peak) |
| TIN validation | Relaxed | Strict (TIN+BRN cross-validation since Aug 2026) |
| Document persistence | May be periodically cleared | Permanent |
| Date export | Limited to 10 days | Limited to 10 days |

---

## 20. Common Error Quick Reference

| Error | Meaning | Fix |
|-------|---------|-----|
| CF349 | Buyer phone number missing | Add phone number to client profile |
| CF358 | Buyer TIN invalid | Verify TIN format, add trailing zero if needed |
| ERR205 | General TIN with NA not allowed | Use classification code 004 for consolidated |
| ERR406 | Buyer TIN not found in LHDN records | Use Search TIN API to find correct TIN |
| DS300-DS338 | Signature errors | See signature errors cookbook |
| HTTP 429 | Rate limit exceeded | Implement backoff, wait retry-after seconds |
| HTTP 504 | Gateway timeout | Reduce batch size, retry |
| "Unable to obtain valid MyInvois token" | API key expired | Generate new Client Secret |
| DateExpected / TimeExpected | Empty date/time fields | Remove optional blocks entirely from JSON |
| NoAdditionalPropertiesAllowed | UBLExtensions in JSON | Remove UBLExtensions and Signature blocks |

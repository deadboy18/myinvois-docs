<!-- source: LHDNM e-Invoice General FAQs (Updated 5 May 2026) -->

# Implementation of e-Invoice in Malaysia — Frequently Asked Questions (FAQs)

**Updated on 5 May 2026**

---

## Table of Contents

- [Part 1: General](#part-1-general)
  - [(i) Introduction to e-Invoice](#i-introduction-to-e-invoice)
  - [(ii) Applicability of e-Invoice Implementation in Malaysia](#ii-applicability-of-e-invoice-implementation-in-malaysia)
  - [(iii) Available Support and Assistance](#iii-available-support-and-assistance)
- [Part 2: Scope and Process](#part-2-scope-and-process)
  - [(i) General](#i-general-1)
  - [(ii) Details of Supplier and Buyer](#ii-details-of-supplier-and-buyer)
  - [(iii) Issuance of e-Invoices](#iii-issuance-of-e-invoices)
  - [(iv) Validation of e-Invoices](#iv-validation-of-e-invoices)
  - [(v) Sharing of e-Invoices](#v-sharing-of-e-invoices)
  - [(vi) Adjustments to e-Invoices](#vi-adjustments-to-e-invoices)
  - [(vii) Transitional period](#vii-transitional-period)
  - [(viii) e-Invoice treatment for certain transactions](#viii-e-invoice-treatment-for-certain-transactions)
  - [(ix) e-Invoice treatment for vouchers, gift cards and loyalty points](#ix-e-invoice-treatment-for-vouchers-gift-cards-and-loyalty-points)
  - [(x) Import / Export of goods or services](#x-import--export-of-goods-or-services)
- [Part 3: e-Invoice for MSME](#part-3-e-invoice-for-msme)
- [Part 4: e-Invoice Treatment During Interim Relaxation Period](#part-4-e-invoice-treatment-during-interim-relaxation-period)
- [Part 5: Systems, Data Security and Privacy](#part-5-systems-data-security-and-privacy)
  - [(i) Issuance of e-Invoice to MyInvois System via API and/or MyInvois Portal](#i-issuance-of-e-invoice-to-myinvois-system-via-api-andor-myinvois-portal)
  - [(ii) Data security and data privacy](#ii-data-security-and-data-privacy)

---

## Part 1: General

### (i) Introduction to e-Invoice

#### 1. What is an e-Invoice?

An e-Invoice is a digital representation of a transaction between a supplier and a buyer, formatted in a structured, machine-readable manner. It is a file created in the format specified by the IRBM (i.e., in XML or JSON file format) and not in the form of PDF, JPG and etc.

#### 2. What is the e-Invoice file format acceptable by IRBM for validation purposes?

The e-Invoice must be generated in the form of XML or JSON file format, in accordance with the requirements outlined by the IRBM. Refer to e-Invoice Software Development Kit (SDK) Microsite via the following link — <https://sdk.myinvois.hasil.gov.my> for the sample of XML / JSON.

#### 3. What are the transmission mechanisms available to transmit e-Invoices?

IRBM has provided two (2) e-Invoice transmission mechanisms:

1. Through the MyInvois Portal provided by IRBM; and
2. Application Programming Interface (API).

Taxpayers may use either or both transmission mechanisms to transmit e-Invoices, as long as there is no duplication of e-Invoices.

> **Note:** The e-Invoice model in Malaysia adopts the Continuous Transaction Control (CTC) Model, which enables a high level of control through the validation of e-Invoices received by IRBM.

#### 4. Are taxpayers allowed to continue claiming for tax deduction / personal tax relief without an e-Invoice?

Yes, taxpayers can continue to claim tax deductions or personal tax relief using existing documentation until such time the legislation has been amended.

---

### (ii) Applicability of e-Invoice Implementation in Malaysia

#### 5. Is e-Invoice applicable to transactions in Malaysia only?

No, the issuance of e-Invoice is not limited to only transactions within Malaysia. It is also applicable for cross-border transactions.

#### 6. Are all businesses required to issue e-Invoice?

Yes, all taxpayers undertaking commercial activities in Malaysia are required to issue e-Invoice, in accordance with the phased mandatory implementation timeline. Refer to Section 1.5 of the e-Invoice Guideline for further details.

#### 7. Are all industries included in the e-Invoice implementation? Are there any industries exempted?

Currently, there are no industries that are exempted from the e-Invoice implementation.

Note that certain persons and types of income and expense are exempted from e-Invoice implementation. Refer to Section 1.6 of the e-Invoice Guideline for further details.

#### 8. Is a Special Purpose Vehicle (SPV) under Section 60I of the Income Tax Act 1967 required to implement e-Invoice?

Yes, an SPV under Section 60I is required to implement e-Invoice, and is also required to obtain its own tax identification number (TIN) to meet its e-Invoice obligations.

#### 9. What are the thresholds for e-Invoice implementation to be applicable to taxpayers?

All taxpayers are required to implement e-Invoice. e-Invoice will be implemented in phases according to annual turnover or revenue thresholds as stated in the statement of comprehensive income in the Financial Year 2022 Audited Financial Statements.

Refer to Section 1.5 of the e-Invoice Guideline for further details.

#### 10. Has there been any change to the mandatory implementation date for taxpayers with an annual turnover or revenue exceeding RM100 million due to the release of Income Tax (Issuance of Electronic Invoice) Rules 2024 [P.U. (A) 265] on 30 September 2024? If so, what is the revised mandatory implementation date for these taxpayers?

The Income Tax (Issuance of Electronic Invoice) Rules 2024 [P.U. (A) 265] has been gazetted effective from 1 October 2024. The rules are issued in accordance with the power conferred to Minister to prescribe a person to issue an e-Invoice and the particulars to be included in the e-Invoice.

The prescribed mandatory implementation date of 1 October 2024 for taxpayers with an annual turnover or revenue of more than RM100 million specifically relates to the period when enforcement actions for non-compliance may be initiated by the IRBM against such taxpayers. In other words, any penalty for non-compliance, should they arise, will only be imposed starting from 1 October 2024.

The mandatory implementation date as outlined in Section 1.5 of the e-Invoice Guideline remains unchanged.

| Targeted Taxpayers | Implementation Date |
|---|---|
| Taxpayers with an annual turnover or revenue of more than RM100 million | 1 August 2024 |
| Taxpayers with an annual turnover or revenue of more than RM25 million and up to RM100 million | 1 January 2025 |
| Taxpayers with an annual turnover or revenue of more than RM5 million and up to RM25 million | 1 July 2025 |
| Taxpayers with an annual turnover or revenue of up to RM5 million | 1 January 2026 |

#### 11. If my business is in operation as at year of assessment (YA) 2022 and my annual revenue did not reach RM1 million in YA2022, how should I determine my e-Invoice implementation date?

**(a) Taxpayers that are able to meet the e-Invoice exemption criteria** (please refer Part 3: e-Invoice for MSME of this FAQ for more details on the criteria)

- The taxpayer's e-Invoice implementation date will fall on 1 July 2026 (concessionary e-Invoice implementation date) as the taxpayer's YA2022 annual turnover or revenue did not exceed RM1 million. However, since the taxpayer fulfilled the criteria to be exempted, the taxpayer is exempt from implementation of e-Invoice (including the self-billed e-Invoice requirements).
- If the taxpayer's annual turnover or revenue subsequently reached / exceeded RM1 million in YA2023, YA2024 or YA2025, the taxpayer is required to implement e-Invoice starting from 1 July 2026.
- If the taxpayer's annual turnover or revenue reaches / exceeds RM1 million in YA2026 or thereafter, the taxpayer is required to implement starting from 1 January in the second year following the YA in which the total annual turnover or revenue reaches / exceeds RM1 million.

**(b) Taxpayers that are not able to meet the e-Invoice exemption criteria** (please refer Part 3: e-Invoice for MSME of this FAQ for more details on the criteria)

- The taxpayer's e-Invoice implementation date will fall on 1 July 2026 (concessionary e-Invoice implementation date) as the taxpayer's YA2022 annual turnover or revenue did not exceed RM1 million. Since the exemption criteria were not met, the taxpayer is required to implement e-Invoice starting from 1 July 2026 (concessionary e-Invoice implementation date).

> **Example:** Mountain Jump Sdn Bhd closes its financial year on 31 December every year. Mountain Jump Sdn Bhd is an independent company with no corporate shareholders. In YA2022, Mountain Jump Sdn Bhd recorded an annual revenue of RM400,000. As Mountain Jump Sdn Bhd's annual revenue is below the RM1 million threshold and it fulfilled the criteria to be exempted, Mountain Jump Sdn Bhd is exempt from implementing e-Invoice.
>
> However, in YA2024, Mountain Jump Sdn Bhd has recorded an annual revenue of RM1.1 million. As Mountain Jump Sdn Bhd's annual revenue has exceeded the exemption threshold, Mountain Jump Sdn Bhd is required to implement e-Invoice starting from 1 July 2026 (concessionary e-Invoice implementation date).

#### 12. If my business commenced operation from YA2023 to YA2025, how should I determine my e-Invoice implementation date?

**(a) Taxpayers with annual turnover or revenue of at least RM1 million in YA2023, YA2024 or YA2025**

- As the taxpayer's annual turnover or revenue reached / exceeded RM1 million in YA2023, YA2024 or YA2025, the taxpayer is required to implement e-Invoice starting from 1 July 2026.

> **Example:** Layang Warna Sdn Bhd was incorporated on 1 October 2023 and commenced operations on the same date, closing its accounts on 30 September each year. It is an independent company with no corporate shareholders. In YA2024 (i.e., 1 October 2023 to 30 September 2024), Layang Warna Sdn Bhd recorded an annual revenue of RM1.12 million. As Layang Warna Sdn Bhd's annual revenue has exceeded the exemption threshold, Layang Warna Sdn Bhd is required to implement e-Invoice starting from 1 July 2026.

**(b) Taxpayers with annual turnover or revenue of less than RM1 million in YA2023, YA2024 and YA2025 who are **able** to meet the e-Invoice exemption criteria** (please refer Part 3: e-Invoice for MSME of this FAQ for more details on the criteria)

- The taxpayer's e-Invoice implementation date will fall on 1 July 2026 (concessionary e-Invoice implementation date) as the taxpayer's YA2023, YA2024 and YA2025 annual turnover or revenue did not exceed RM1 million. However, since the taxpayer fulfilled the criteria to be exempted, the taxpayer is exempt from implementation of e-Invoice (including self-billed e-Invoice requirements).
- If the taxpayer's annual turnover or revenue subsequently reached / exceeded RM1 million in YA2026 onwards, the taxpayer is required to implement starting from 1 January in the second year following the YA in which the total annual turnover or revenue reaches RM1 million.

> **Example:** Pak Salleh started its sole proprietorship business, Kedai Runcit Salleh, on 1 January 2025. In YA2025, Pak Salleh records an annual revenue of RM185,000. As Pak Salleh's annual revenue is recorded below the RM1 million threshold and the criteria to be exempted have been fulfilled, Pak Salleh is exempted from implementing e-Invoice.
>
> However, in YA2027, Pak Salleh records an annual revenue of RM1.17 million. As Pak Salleh's annual revenue has exceeded the exemption threshold, Pak Salleh is required to implement e-Invoice starting from 1 January 2029.

**(c) Taxpayers with annual turnover or revenue of less than RM1 million in YA2023, YA2024 and YA2025 who are **not able** to meet the e-Invoice exemption criteria** (please refer Part 3: e-Invoice for MSME of this FAQ for more details on the criteria)

- The taxpayer's e-Invoice implementation date will fall on 1 July 2026 (concessionary e-Invoice implementation date) as the taxpayer's YA2023, YA2024 and YA2025 annual turnover or revenue did not exceed RM1 million. Since the exemption criteria were not met, the taxpayer is required to implement e-Invoice starting from 1 July 2026.

> **Example:** Paper Bottle Sdn Bhd, a wholly-owned subsidiary of Paper All Berhad that has implemented e-Invoice, was incorporated on 1 April 2023 and commenced operations on the same date, closing its accounts on 31 March each year. In YA2024 and YA2025, Paper Bottle Sdn Bhd recorded annual revenue of RM230,000 and RM365,000 respectively. Even though Paper Bottle Sdn Bhd's annual revenue did not exceed the exemption threshold, the exemption criteria were not met. In this case, Paper Bottle Sdn Bhd is required to implement e-Invoice starting from 1 July 2026.

#### 13. If my business commences operation from YA2026 onwards, how should I determine my e-Invoice implementation date?

**(a) Taxpayers that are not able to meet the e-Invoice implementation exemption criteria** (please refer Part 3: e-Invoice for MSME of this FAQ for more details on the criteria)

- The taxpayer is required to implement e-Invoice starting from 1 July 2026 or upon the operation commencement date, whichever later, as the exemption criteria are not met.

> **Example:** Coco Boss Sdn Bhd is a company principally engaged in the business of cocoa plantation and it has implemented e-Invoice since 1 August 2024. Coco Boss Sdn Bhd has established a new subsidiary, Coco Lock Sdn Bhd, on 1 September 2026. Coco Lock Sdn Bhd commenced operations immediately, on the same day of incorporation. As the exemption criteria were not met, Coco Lock Sdn Bhd is required to implement e-Invoice starting from its operation commencement date (i.e., 1 September 2026).

**(b) Taxpayers that are able to meet the e-Invoice implementation exemption criteria** (please refer Part 3: e-Invoice for MSME of this FAQ for more details on the criteria)

- As the exemption criteria are being met, the taxpayer is required to implement e-Invoice starting from 1 January in the second year following the YA in which the total annual turnover or revenue reaches RM1 million.
- If the taxpayer's annual turnover or revenue for the first YA (i.e., YA2026) has reached / exceeded RM1 million, the taxpayer is required to implement starting from 1 January in the second year following the YA in which the total annual turnover or revenue reaches RM1 million.
- However, if the taxpayer's annual turnover or revenue for the first YA (i.e., YA2026) did not exceed RM1 million, the taxpayer is exempt from implementation of e-Invoice. Subsequently, if the taxpayer's annual turnover or revenue reached / exceeded RM1 million in YA2027 or thereafter, the taxpayer is required to implement starting from 1 January in the second year following the YA in which the total annual turnover or revenue reaches RM1 million.

> **Example:** The operations of Warung Salima, owned and run by Puan Salima, commenced on 1 January 2026. In YA2026, Puan Salima records an annual revenue of RM1.12 million. As Puan Salima's annual revenue has exceeded the RM1 million threshold, Puan Salima is required to implement e-Invoice starting from 1 January 2028.

> **Example:** Adjacent to Warung Salima, Mak Cik Samsiah started her sole proprietorship business, Pinggan Mangkuk Enterprise, on 1 January 2026. For YA2026, Mak Cik Samsiah records an annual revenue of RM308,000. As Mak Cik Samsiah's annual revenue is recorded below the RM1 million threshold and the criteria to be exempted have been fulfilled, Mak Cik Samsiah is exempted from implementing e-Invoice.
>
> In YA2027, Mak Cik Samsiah records an annual revenue of RM1.14 million. As Mak Cik Samsiah's annual revenue has exceeded the exemption threshold, Mak Cik Samsiah is required to implement e-Invoice starting from 1 January 2029.

---

### (iii) Available Support and Assistance

#### 14. How can taxpayers start preparing for the e-Invoice implementation?

When implementing e-Invoice, the three (3) primary factors to consider are:

- **Human resources:** Businesses are advised to assemble a dedicated team equipped with the necessary expertise and capabilities to prepare for the implementation.
- **Business processes:** Conduct a comprehensive review of the current process and procedures in issuing invoice transaction documents.
- **Technology and systems:** Confirm the availability of the necessary resources, data structures and existing IT capabilities to ensure the systems in place comply with the e-Invoice requirements.

#### 15. Will there be any engagement sessions with IRBM?

IRBM is actively conducting a series of engagement sessions with industry players, tax practitioners, professional bodies and identified stakeholders to provide comprehensive information regarding the implementation of e-Invoice in Malaysia including:

a) Sharing the planning of action plans, strategies, and status developments regarding the implementation of e-Invoice; and

b) Obtaining feedback and views through two-way communication between IRBM and taxpayers on the implementation of e-Invoice.

Taxpayers may also contact their respective IRBM state offices to request for engagement sessions.

#### 16. Who should we reach out to if there are any queries or concerns?

You may contact us through the following channels for any queries or concerns regarding:

a) The e-Invoice implementation in Malaysia: myinvois@hasil.gov.my

b) Any enquiries (e.g.: Software Development Kit (SDK)): MyInvois Customer Feedback Form at <https://feedback.myinvois.hasil.gov.my>

Alternatively, taxpayers may reach out to the e-Invois HASiL Help Desk Line at **03-8682 8000**, available 24 hours a day, from Monday to Sunday OR chat with us via the MyInvois Live Chat at <https://www.hasil.gov.my/en/e-invoice/contact-us>.

#### 17. Are there any incentives provided to taxpayers for the implementation of e-Invoice?

The Malaysian government has announced the following tax incentives or grants in relation to implementation of e-Invoice during Budget 2024.

Tax deduction of up to RM50,000 for each year of assessment given on environmental, social and governance related expenditure, including consultation fee for the implementation of e-Invoice incurred by Micro, Small, and Medium Enterprises (MSMEs), effective from year of assessment 2024 to year of assessment 2027.

---

## Part 2: Scope and Process

### (i) General

#### 18. For transactions conducted in foreign currencies, are we required to convert it into Ringgit Malaysia (RM) equivalent (RM-equivalent) when issuing an e-Invoice / self-billed e-Invoice?

e-Invoice can be displayed in any currency, including RM. Unless there are legal or tax requirements to include the RM-equivalent, taxpayers can issue the e-Invoice in foreign currency. Refer to Section 13 of the e-Invoice Specific Guideline for further details.

#### 19. Do buyers need to issue self-billed e-Invoices to record expenses when dealing with suppliers located in rural areas with poor internet connectivity?

Kindly note that self-billed e-Invoice is only allowed for certain transactions as stated under Section 8 of the e-Invoice Specific Guideline.

Businesses who are facing poor internet connectivity and lack infrastructure to issue e-Invoice are advised to approach the nearest IRBM state office for further discussions.

#### 20. Can I issue self-billed e-Invoice if my supplier did not issue e-Invoice to me?

Issuance of self-billed e-Invoice is only permitted for circumstances that are provided under Section 8.3 of the e-Invoice Specific Guideline.

Where the transaction does not fall within Section 8.3 of the e-Invoice Specific Guideline, taxpayers are not allowed to issue self-billed e-Invoice.

#### 21. Can I issue self-billed e-Invoice for any circumstance that is not listed under Section 8.3 of the e-Invoice Specific Guideline?

No, please note that issuance of self-billed e-Invoice is only permitted for circumstances that are provided under Section 8.3 of the e-Invoice Specific Guideline.

#### 22. Do taxpayers need to include the details of sales tax exemption under Schedule 3 of the Sales Tax (Person Exempted from Payment of Tax) Order 2018 (i.e., C3 exemption approval number) in the 'Details of Tax Exemption' data field when issuing e-Invoice / self-billed e-Invoice?

Further to harmonisation with Royal Malaysian Customs Department (RMCD), taxpayers are required to input the C3 details when issuing e-Invoice or self-billed e-Invoice, as the case may be.

---

### (ii) Details of Supplier and Buyer

#### 23. Are the buyers required to provide their relevant details to the supplier for the issuance of e-Invoices?

Yes, where the buyers request for an e-Invoice to be issued, the buyers are required to share their details to the supplier for the purposes of issuing e-Invoice.

#### 24. Which TIN should individual taxpayers use for the purposes of e-Invoice?

For the purposes of e-Invoice, individual taxpayers should provide TIN with prefix of "IG". Please refer to Section 2.3.1.1 of the e-Invoice Guideline for more details on how to retrieve and verify TIN.

#### 25. How do taxpayers issue an e-Invoice to a non-TIN holder such as foreign (non-Malaysian) buyers?

Suppliers are required to obtain buyer's details from the foreign buyers for the purposes of e-Invoice issuance.

In relation to TIN to be input in the e-Invoice, supplier may use "EI00000000020" for foreign buyers without TIN. Refer to Section 10.5 of the e-Invoice Specific Guideline for further details.

#### 26. What is the TIN and BRN should be used for the issuance of e-Invoice / self-billed e-Invoice to government, state government and state authority, government entity, local authority, statutory authority and statutory body?

The issuer can input general TIN "EI00000000040" and BRN "NA" for the issuance of e-Invoice / self-billed e-Invoice to the abovementioned persons.

#### 27. For the purposes of issuing e-Invoice / self-billed e-Invoice, can the Malaysian taxpayers default the TIN of the foreign supplier / foreign buyer to be general TIN, regardless of whether the foreign supplier / foreign buyer has a specific TIN assigned by the IRBM?

It is the taxpayers' responsibility to verify with their foreign supplier / foreign buyer on the availability of TIN. The general TIN provided is to ease taxpayers in issuing e-Invoice / self-billed e-Invoice in the case where the said foreign supplier / foreign buyer does not have a TIN assigned by the IRBM.

#### 28. Should taxpayers register with Companies Commission of Malaysia (SSM) input both the old and new business registration number when issuing an e-Invoice?

Taxpayers registered with SSM are required to input the new business registration number (12-digit characters) for the purposes of e-Invoice issuance.

However, IRBM understand that there have been some difficulties for taxpayers to provide the new business registration number from SSM, resulting in validation failures. In response, IRBM has taken a step to temporarily disable the business registration number validation requirement from the e-Invoice submission process, effective from 19 July 2024.

#### 29. If taxpayers are not register with SSM, what are the information that the taxpayer should input for business registration number when issuing an e-Invoice?

For taxpayers that are registered with other authority / body, the taxpayers are required to input the relevant registration number.

---

### (iii) Issuance of e-Invoices

#### 30. Do businesses need to submit an e-Invoice within the same day the transaction is being made?

There is no specific requirement on the timing of e-Invoice issuance, except in specific cases such as consolidated e-Invoice, self-billed e-Invoice for importation of goods / services and e-Invoice for foreign income.

- For **consolidated e-Invoice**, suppliers are required to issue the consolidated e-Invoice within seven (7) calendar days after the month end.
- For **self-billed e-Invoice for importation of goods**, the Malaysian purchasers are required to issue self-billed e-Invoice latest by the end of the second month following the month of customs clearance is obtained.
- For **self-billed e-Invoice for importation of services**, the Malaysian purchasers are required to issue self-billed e-Invoice latest by the end of the month following the month upon (1) payment made by the Malaysian purchaser; or (2) receipt of invoice from foreign supplier, whichever earlier. The determination of the aforementioned (1) and (2) is in accordance with the prevailing rules applicable for imported taxable service.
- For **e-Invoice for foreign income**, the suppliers (i.e., income recipients) are required to issue the e-Invoice latest by the end of the month following the month of receipt of the said foreign income.

Where any specific legislation is applicable, you may proceed to follow as per the said legislation.

#### 31. Are businesses allowed to issue consolidated e-Invoice for all types of transactions where the Buyers do not require an e-Invoice?

Yes, the supplier is allowed to issue consolidated e-Invoice for transactions where no request for e-Invoice has been made by the buyer, regardless of business-to-business (B2B), business-to-consumer (B2C) or business-to-government (B2G) transactions, except for transactions / activities listed under Section 3.7 of the e-Invoice Specific Guideline.

#### 32. Does the MyInvois Portal allow for e-Invoice to be created and stored in draft form prior to finalisation and issuance?

Yes, the supplier will be able to create e-Invoice in draft or proforma. e-Invoice will only be accepted once the validation is successful.

#### 33. What are the consequences for failure to issue e-Invoice?

Failure to issue e-Invoice is an offence under Section 120(1)(d) of the Income Tax Act 1967 and will result in a fine of not less than RM200 and not more than RM20,000 or imprisonment not exceeding 6 months or both, for each non-compliance.

---

### (iv) Validation of e-Invoices

#### 34. How long does it take for an e-Invoice to be validated by IRBM?

The e-Invoice validation by IRBM will be done in near real-time, generally in less than two (2) seconds. Refer to Section 3.8 of the e-Invoice Specific Guideline for further details.

#### 35. What does IRBM validate in the e-Invoice?

IRBM validation includes a series of checks to ensure the e-Invoice submitted to IRBM conforms to the e-Invoice format and data structure as specified by IRBM. Refer to the e-Invoice SDK Microsite for further details.

#### 36. Will the supplier's invoice number remain in the invoice document or will a different invoice number be assigned by IRBM upon the return of the validated e-Invoice to the supplier?

The supplier's invoice number will remain as a separate field in the same invoice document for the purpose of the supplier's internal reference and tracking.

Upon validation, the IRBM will assign a Unique Identifier Number to each e-Invoice.

---

### (v) Sharing of e-Invoices

#### 37. Does IRBM provide a standard visual representation template for suppliers to share the e-Invoices to buyers upon IRBM's validation?

Taxpayers are allowed to adopt any format for the visual representation of the e-Invoice as per current practice, provided the QR code is embedded accordingly.

#### 38. Is the issuer required to share the validated e-Invoice or visual representation of the e-Invoice?

IRBM acknowledges that there may be practical challenges in sharing the validated e-Invoice (in the form of XML / JSON file). Therefore, until further notice, the IRBM allows taxpayers to share either the validated e-Invoice or a visual representation of the validated e-Invoice, or both.

---

### (vi) Adjustments to e-Invoices

#### 39. Does the MyInvois Portal allow for editing of information after the e-Invoice has been validated by IRBM?

No, the supplier would need to cancel the e-Invoice within 72 hours from time of validation and reissue a new e-Invoice.

Any changes after 72 hours from time of validation would require the supplier to issue a new e-Invoice (i.e., debit note, credit note, refund note e-Invoice) to adjust the original e-Invoice issued. Thereafter, a new e-Invoice would be required to be issued accordingly.

#### 40. Is there any adjustment window allowed to the supplier to cancel an invoice submitted to IRBM?

Yes, there is a 72-hour timeframe for the e-Invoice to be cancelled by the supplier. Refer to sections 2.3.6 or 2.4.5 of the e-Invoice Guideline for further details.

#### 41. If the issuer accidentally made a mistake or an information was provided inaccurately during the issuance of e-Invoice / self-billed e-Invoice, how should the issuer rectify this mistake if the issuer notices the mistake within 72-hour from time of validation of the said e-Invoice / self-billed e-Invoice?

The issuer is allowed to cancel the e-Invoice issued and validated by IRBM within 72 hours from the time of validation, and subsequently reissue a new e-Invoice.

#### 42. Following the above scenario, if the issuer does not want to cancel the e-Invoice but decides to issue a credit note / debit note / refund note e-Invoice to make adjustment, will this be allowed?

Yes, the issuer is allowed to issue a credit note / debit note / refund note e-Invoice to reflect the necessary adjustment to the original e-Invoice, even if the 72-hour timeframe has not elapsed.

Kindly note to include the Unique Identifier Number of the affected original e-Invoice under the "Original e-Invoice Reference Number" field in the issuance of credit note / debit note / refund note e-Invoice.

#### 43. Following the above scenario, if the issuer notices the mistake only in the following week, how should the issuer rectify the mistake in this case?

In event where the mistake was noticed and/or any amendment is required on the e-Invoice issued after the 72-hour timeframe, the issuer is required to reflect the amendments / adjustment by way of issuing a credit note / debit note / refund note e-Invoice.

Kindly note to include the Unique Identifier Number of the affected original e-Invoice under the "Original e-Invoice Reference Number" field in the issuance of credit note / debit note / refund note e-Invoice.

#### 44. What is the timeframe to issue e-Invoice adjustments (i.e., debit note, credit note and refund note) after the 72-hours validation period?

There is no timeframe restriction for suppliers to issue e-Invoice adjustments. Taxpayers may follow their respective company policy for any e-Invoice adjustments.

As foreign suppliers and/or buyers generally do not use MyInvois System, any e-Invoice adjustments would be done through issuance of debit note, credit note, and refund note e-Invoice.

#### 45. Can taxpayers issue one (1) credit note e-Invoice to adjust multiple original e-Invoices?

Yes, taxpayers can make adjustments to multiple original e-Invoices by issuing one (1) single credit note / debit note / refund note e-Invoice, as the case may be. Note that adjustments can also be made to consolidated e-Invoices.

Where multiple original e-Invoices are being adjusted in the same credit note / debit note / refund note e-Invoice, the IRBM Unique Identifier Number of each original e-Invoice must be indicated in the 'Original e-Invoice Reference Number' data field. Taxpayers who are transmitting via API are reminded to refer to the SDK for the appropriate code to be input.

If adjustments are to be made to original invoices that were issued prior to the implementation of e-Invoice (i.e., no IRBM Unique Identifier Number has been assigned), taxpayers are then allowed to input "NA" in the 'Original e-Invoice Reference Number' data field.

#### 46. Is an e-Invoice required for return of monies to buyers?

Yes, a refund note e-Invoice is required for return of monies to buyers, with the following exception:

1. Payment made wrongly by buyers
2. Overpayment by buyers
3. Return of security deposits

---

### (vii) Transitional period

#### 47. Prior to full implementation, there may be suppliers who do not issue e-Invoice as they have not reached the mandatory implementation phase / date. In this situation, will IRBM accept normal invoices issued by these suppliers?

The compliance obligation of issuing e-Invoice lies with the Supplier (or the Buyer in the case of self-billed e-Invoice). As such, during the transitional period, taxpayers will be allowed to provide either normal bill / receipt / invoice (if the Supplier has yet to implement e-Invoice in accordance with the implementation timeline) or validated e-Invoice to substantiate a transaction for tax purposes until full implementation of e-Invoice.

---

### (viii) e-Invoice treatment for certain transactions

#### 48. Which types of income fall into the scope for e-Invoice, considering taxpayers may have a range of income sources of contributing to their annual turnover (e.g., interest income, investment income, gains from investment disposal)?

e-Invoice is required for all types of income and expenses, excluding certain types as outlined in the e-Invoice General Guideline and e-Invoice Specific Guideline.

Kindly note that the abovementioned exemptions are subject to periodic review and updates.

#### 49. What is the e-Invoice treatment for deposits?

In cases where the deposit is refundable, the issuance of e-Invoice is not required. However, if the deposit is non-refundable, issuance of e-Invoice is required.

#### 50. What is the e-Invoice treatment for director fees?

Where the Director has entered into contract for service, the Director is required to issue e-Invoice to the company for any income received.

However, where the Director has entered into contract of service, the fees are considered as part of employment income. Currently, this is being exempted from issuance of e-Invoice as per the e-Invoice Guideline. Kindly note that the exemptions are subject to periodic review and updates.

#### 51. Is an e-Invoice required for disbursements and reimbursements?

An e-Invoice may be required for disbursements and reimbursements, subject to the situation. Refer to Section 5 of the e-Invoice Specific Guideline for further details.

#### 52. Do we need to issue e-Invoice for intercompany charges?

Yes, an e-Invoice is required to be issued for intercompany charges.

#### 53. Is e-Invoice required for inter-department / inter-division transactions within the same company?

No e-Invoice is required for inter-department / inter-division transactions with the same company. However, businesses may issue e-Invoice between inter-department / inter-division, depending on the businesses' practices.

#### 54. Is an individual landlord required to issue e-Invoice to the tenant for rental of property?

Where the individual landlord is conducting a business, an e-Invoice is required to be issued to the tenant.

Where the individual landlord is not conducting a business, the tenant (if they are a business) would be required to issue a self-billed e-Invoice for the rental of property.

#### 55. How should the tenant substantiate its payment of utility expenses if the utility bills are issued in the landlord's name?

Where the tenant is unable to request for the utility bills to be issued in his name, such utilities paid by the tenant should be included in:

- the e-Invoice issued by the landlord (if the landlord is conducting a business); or
- the self-billed e-Invoice issued by the tenant (if the landlord is not conducting a business),

as the case may be.

#### 56. In a consignment arrangement where the consignee pays the consignor for the goods consigned at their premises only when the goods are sold / consumed. In this case, when should an e-Invoice be issued?

The consignor (supplier in this case) should issue the e-Invoices to the consignee (buyer in this case) upon sale / consumption of goods by the consignee, in accordance with the current consignor's practice in issuing invoices.

#### 57. Should self-billed e-Invoice on payment to agents, dealers and distributors be done on an accrual or paid basis?

The issuance of self-billed e-Invoices on payments to agents, dealers, and distributors can be done on either an accrual basis or paid basis, as long as this is consistently applied and in line with the documentation provided to the agents (e.g., statements).

#### 58. For certain corporate agents where the existing process includes the issuance of invoice to the company, can the current process of the corporate agent issuing the e-Invoice be maintained instead of being under self-billed e-invoice?

Buyer (i.e., taxpayer that makes payment to agents, dealers and distributors) is required to issue self-billed e-Invoice for payments or any other incentives (whether in monetary form or otherwise made) to the agent, dealers and distributors as outlined under Section 9.4 of the e-Invoice Specific Guideline.

This requirement applies regardless of whether the agents, dealers and distributors are individual or corporate (i.e., supplier in this case). The buyer is obliged to share validated self-billed e-Invoice with the supplier upon validation.

In this regard, the agents, dealers and distributors are not required to issue an e-Invoice to the taxpayers who make payment.

#### 59. For monetary incentive payments, can self-billed e-Invoice be done based on the periodic statements provided to agents?

Yes, self-billed e-Invoices can be issued based on the periodic statements provided to agents following current practice / frequency (e.g., bi-weekly, monthly etc.). The appropriate classification code should be input in the self-billed e-Invoice.

#### 60. As the invoices / statements currently do not contain non-monetary incentive payments (e.g., incentive trips, gifts, etc.) and this information is typically only available during the preparation of the annual CP58, can the data for non-monetary incentives be transmitted upon the availability of such information (i.e., annually)?

Yes, it is acceptable to transmit the information for non-monetary incentive payments annually if such information is only available during the preparation of the annual CP58.

However, if such information is available on accrual or payment basis, or upon issuance of periodic invoices / statements provided to agents, dealers and distributors, it should be transmitted to IRBM in the self-billed e-Invoice with the same frequency.

The appropriate classification code should be input in the self-billed e-Invoice.

#### 61. Can the self-billed e-Invoice be issued on a net basis (gross amount minus reversals), which matches the total amount paid as presented in the statements to agents?

Yes, a self-billing arrangement can be done on a net commission basis (gross amount minus reversals), provided the line details of the commission reversals are being included in the self-billed e-Invoice.

**Scenario 1: Net commission paid to agent**

| Month | Details | Amount (RM) |
|---|---|---|
| Jan 2025 | Commission payable for Jan 2025 | 3,000 |
| Jan 2025 | Actual payment to agent in Jan 2025 | 3,000 |
| Feb 2025 | Commission payable for Feb 2025 | 2,000 |
| Feb 2025 | Less: Clawback / reversal of commission for Jan 2025 | (1,000) |
| Feb 2025 | Actual payment to agent in Feb 2025 | 1,000 |

The line details of the commission reversal in Feb 2025 needs to be included in the self-billed e-Invoice. Self-billed e-Invoice will be issued on monthly basis (in accordance with current issuance frequency): RM3,000 for Jan 2025 and RM1,000 for Feb 2025.

**Scenario 2: Clawback is more than the commission payable**

Self-billed credit note is required to be issued for any monetary and non-monetary incentives payable to agents, irrespective of whether the actual payment is nil. Self-billed e-Invoice will be issued on monthly basis: RM3,000 for Jan 2025, (RM1,000) for Feb 2025, RM2,000 for Mar 2025.

**Scenario 3: No payment to individual agents due to set-off of expenses**

The self-billed e-Invoice should include only monetary and non-monetary incentives payable to agents. In cases where rental is charged to an agent, a separate e-Invoice is required to be issued by the company, as this does not constitute a part of the incentive payments.

#### 62. Should self-billed e-Invoice on interest be done on an accrual or paid basis?

The issuance of self-billed e-Invoices on interest can be done on either an accrual basis or paid basis.

---

### (ix) e-Invoice treatment for vouchers, gift cards and loyalty points

> Note that this applies to both monetary vouchers (e.g., gift card, token, stamp (other than postage stamp), credit reload etc.) and non-monetary vouchers (i.e., voucher / coupon which displays the specific goods or services that can be redeemed).

#### 63. Do we need to issue e-Invoice upon sale / giveaway of vouchers?

In cases where the voucher is given for free or is refundable in nature, no e-Invoice is required to be issued. However, if the voucher sold is non-refundable, issuance of e-Invoice is required.

#### 64. Is e-Invoice required to be issued for expired vouchers?

Where the free vouchers or non-refundable vouchers sold (where an e-Invoice has been issued) have expired, no e-Invoice is required to be issued upon expiry. However, where the refundable vouchers have expired, issuance of e-Invoice is required upon expiry.

#### 65. Do we need to issue e-Invoice upon redemption of refundable vouchers?

Yes, taxpayers are required to issue e-Invoice upon utilisation of the refundable vouchers. However, as the vouchers are refundable in nature (where no e-Invoice has been issued), taxpayers are required to ensure that the "Total Excluding Tax" at invoice level matches the total sales for the transaction. Taxpayers may include the refundable vouchers in the "prepayment" field, as this is an optional field.

#### 66. Do we need to issue e-Invoice upon redemption of non-refundable vouchers?

Yes, taxpayers are required to issue e-Invoice upon utilisation of the non-refundable vouchers. However, as the vouchers are non-refundable in nature (where e-Invoice has been issued), taxpayers are required to ensure that the "Total Excluding Tax" at invoice level matches the amount of sales not settled by the non-refundable vouchers. Taxpayers should include the non-refundable vouchers in a separate invoice line.

#### 67. Do we need to issue e-Invoice for the purchase-with-purchase vouchers given to buyers upon certain spending threshold has been met or fulfilling certain conditions? Do we also need to issue e-Invoice for goods sold / services rendered that are settled by the purchase-with-purchase vouchers?

No e-Invoice is required to be issued for the purchase-with-purchase voucher, if the said voucher is provided for free.

For the e-Invoice treatment for expired vouchers and vouchers redemption, please refer to the earlier questions under (ix) Vouchers, gift cards and loyalty points in Part 2: Scope and Process e-Invoice.

---

### (x) Import / Export of goods or services

#### 68. For the purposes of issuing self-billed e-Invoices for purchases from foreign supplier, can the Malaysian buyer consolidate all invoices received from the same foreign supplier?

No. At this juncture, a separate self-billed e-Invoice must be issued for each individual transactions, save for exceptions listed under section 3.6.5 of the e-Invoice Specific Guideline.

#### 69. Are Malaysian buyers required to include the import duty and/or sales tax levied by the RMCD upon customs clearance on imported goods when issuing self-billed e-Invoice?

No, Malaysian buyers are not required to include the duties and/or taxes levied by the RMCD in the self-billed e-Invoice.

#### 70. Does e-Invoice comply with RMCD documents for RMCD clearance? Can the visual representation of e-Invoice be used for RMCD clearance and/or other RMCD purposes?

e-Invoice requirements as outlined in Appendix 1 of the e-Invoice Guideline have taken into consideration the information required by both IRBM and RMCD. Both IRBM and RMCD may use the e-Invoice for tax purposes.

However, note that existing documents required by RMCD for RMCD clearance would still apply. Where the visual representation of e-Invoice contains all the information required by Sales Tax or Service Tax (SST) acts / regulations / guidelines, the taxpayers should be allowed to use the same visual representation for SST purposes.

#### 71. For the purposes of self-billed e-Invoice on importation of goods, which goods value should the taxpayers adopt?

Taxpayer is required to input the actual transaction value incurred as stated in the foreign supplier's invoice for e-Invoice purposes.

Where applicable, taxpayer may include the extra charges applicable as per the agreed incoterms under "Details of other charges" field.

#### 72. In relation to acquisition of goods from foreign suppliers, when will the Malaysian buyers be required to issue a self-billed e-Invoice?

The Malaysian buyers are required to issue a self-billed e-Invoice latest by the end of the second month following the month of RMCD's clearance is obtained on the imported goods. The buyers should include the appropriate details as listed in the self-billed e-Invoice annexure on importation of goods.

#### 73. Where the imported goods are purchased under one foreign supplier's invoice but imported into Malaysia via multiple shipments (with different RMCD's clearance dates), when will the Malaysian buyer be required to issue self-billed e-Invoice?

Where there are multiple import clearances under the same invoice from the foreign supplier, the timing for the issuance of the self-billed e-Invoice depends on when the foreign supplier's invoice was issued relative to the shipments:

**(a) If the foreign supplier's invoice is issued by the time the first import shipment clearance is obtained:**

- The Malaysian buyer is required to issue self-billed e-Invoice based on the foreign supplier's invoice as per the timing of issuance outlined under Section 10.4.8 of the e-Invoice Specific Guideline (i.e., latest by the end of the second month following the month of RMCD's clearance is obtained). The Malaysian buyer is required to input the customs form reference number in the self-billed e-Invoice.
- No self-billed e-Invoice is required to be issued for the subsequent shipments for goods that are included within the same foreign supplier's invoice.

**(b) If the foreign supplier's invoice is issued after the first RMCD's clearance:**

- The Malaysian buyer is required to issue self-billed e-Invoice for each import shipment, based on the amount declared to RMCD, and a final self-billed e-Invoice upon issuance of the foreign supplier's invoice.
- No self-billed e-Invoice is required to be issued for the subsequent shipments of goods that are included within the same foreign supplier's invoice.

**(c) If the foreign supplier's invoice is issued after the final import shipment:**

- The Malaysian buyer is required to issue self-billed e-Invoice for each import shipment, based on the amount declared to RMCD, and a final self-billed e-Invoice upon the issuance of foreign supplier's invoice, if the total amount in the foreign supplier's invoice is different from the total self-billed e-Invoices.

#### 74. If I purchase goods from foreign supplier but the goods are delivered from a bonded warehouse / Free Zone, do I need to include the customs form reference number in the self-billed e-Invoice issued?

Where the goods are delivered by the Foreign Supplier from the bonded warehouse / Free Zone to the Malaysian buyer located in a PCA, the Malaysian buyer is required to input the customs form reference number (e.g., Customs Form No. 1 or No. 9) in the self-billed e-Invoice.

#### 75. If I am located in a bonded warehouse and purchase goods from a foreign supplier, and the said goods are subsequently sold to a Malaysian buyer, what are the e-Invoice treatments?

a) Malaysian Buyer #1 to issue a self-billed e-Invoice for the goods purchased from the Foreign Supplier. The "Reference Number of Customs Form No. 1, 9, etc." field is optional.

b) Malaysian Buyer #1 to issue an e-Invoice for the goods sold to Malaysian Buyer #2. The "Reference Number of Customs Form No. 1, 9, etc." field is optional.

#### 76. If I purchase goods from a foreign supplier (where the goods are stored in bonded warehouse / Free Zone) and I subsequently sell the goods to another Malaysian buyer (i.e., MY Buyer #2), how should the e-Invoice and self-billed e-Invoice be issued?

a) Malaysian Buyer #1 to issue a self-billed e-Invoice for the goods purchased from the Foreign Supplier. The "Reference Number of Customs Form No. 1, 9, etc." field is optional.

b) Malaysian Buyer #1 to issue an e-Invoice for the goods sold to Malaysian Buyer #2. The "Reference Number of Customs Form No. 1, 9, etc." field is optional.

#### 77. I purchase goods from a foreign supplier and subsequently sell the goods to other Malaysian buyers. However, I instruct the foreign supplier to directly deliver the goods to the other Malaysian buyers. How should the e-Invoice and self-billed e-Invoice be issued?

a) Malaysian Buyer #1 to issue a self-billed e-Invoice for the goods purchased from Foreign Supplier. The "Reference Number of Customs Form No. 1, 9, etc." field is optional.

b) Malaysian Buyer #1 to issue e-Invoice to the other Malaysian Buyers #2, #3 and #4 for the goods sold respectively. The "Reference Number of Customs Form No. 1, 9, etc." field is optional.

#### 78. Where the goods are purchased from a Malaysian supplier, whom sources the goods from a foreign supplier and requests for the goods to be directly delivered directly to its customers, how should the e-Invoice and self-billed e-Invoice be issued?

a) Malaysian Buyer #1 to issue a self-billed e-Invoice for the goods purchased from the Foreign Supplier. The "Reference Number of Customs Form No. 1, 9, etc." field is optional.

b) Malaysian Buyer #1 to issue an e-Invoice for the goods sold to Malaysian Buyer #2. The "Reference Number of Customs Form No. 1, 9, etc." field is optional.

#### 79. If I purchase goods from a foreign supplier but the foreign supplier requested for the goods to be directly delivered from their Malaysian supplier (located in a bonded warehouse / Free Zone) to me, am I required to input the customs form reference number?

a) Malaysian Buyer to issue a self-billed e-Invoice for the goods purchased from the Foreign Supplier. The Malaysian Buyer is required to input the customs form reference number (e.g., Customs Form No. 9) under the "Reference Number of Customs Form No. 1, 9, etc." field when issuing self-billed e-Invoice.

b) Malaysian Supplier to issue an e-Invoice for the goods sold to the Foreign Supplier. The "Reference Number of Customs Form No. 2" is an optional field.

#### 80. In relation to acquisition of services from foreign suppliers, when will the Malaysian buyers be required to issue a self-billed e-Invoice?

The Malaysian buyers are required to issue a self-billed e-Invoice latest by the end of the month following the month upon:

1. payment made by the Malaysian buyer; or
2. receipt of invoice from foreign supplier,

whichever is earlier.

The determination of (1) and (2) above is in accordance with the prevailing rules applicable for imported taxable service administered by RMCD.

#### 81. In relation to exportation of goods to foreign buyers, when will the Malaysian supplier be required to issue an e-Invoice?

The Malaysian suppliers are required to issue an e-Invoice as per their current invoicing arrangement. If the customs form (e.g., Customs Form No. 2) is available, the Malaysian suppliers may include the customs form reference number in the e-Invoice. Kindly note that this field is currently an optional field.

#### 82. In relation to drop shipment transactions where the goods are not physically imported into Malaysia, when will the Malaysian taxpayers be required to issue self-billed e-Invoice?

As there is no importation of goods into Malaysia, the timing of issuance of self-billed e-Invoice will be upon the foreign invoice being recorded in the Malaysian taxpayer's accounting system.

#### 83. For low value goods that are declared under the electronic Pre-Alert Manifest (e-PAM) system, what should be input under "Reference Number of Customs Form No. 1, 9, etc." field?

Further to harmonisation with RMCD, IRBM provides a concession allowing taxpayers to not include the e-PAM reference number under the "Reference Number of Customs Form No. 1, 9, etc." field and exclude such field when issuing self-billed e-Invoice.

#### 84. For goods imported via loose container load (LCL) shipment, what should be input under "Reference Number of Customs Form No. 1, 9, etc." field?

Further to harmonisation with RMCD, IRBM provides a concession allowing taxpayers who import goods via LCL shipment to exclude the "Reference Number of Customs Form No. 1, 9, etc." when issuing self-billed e-Invoice.

#### 85. In drop shipment transactions where the goods are not imported into Malaysia, what should be input under "Reference Number of Customs Form No. 1, 9, etc." field?

As the goods are not imported into Malaysia, the "Reference Number of Customs Form No. 1, 9, etc." will not be applicable and taxpayers may exclude such field when issuing e-Invoice / self-billed e-Invoice for IRBM's validation.

#### 86. For goods purchased under Delivered Duty Paid (DDP) incoterm, do I need to input customs form reference number in the "Reference Number of Customs Form No. 1, 9, etc." field?

Further to harmonisation with RMCD, the current legislation requires the Malaysian Buyers to retain the relevant import documentation, even if the goods are purchased under DDP incoterm. As such, the Malaysian Buyer is required to obtain the relevant customs form reference number from the respective foreign supplier or third-party (e.g., agent, freight forwarder, etc.) for self-billed e-Invoice purposes.

#### 87. If there are more than one (1) customs form reference number for the imported goods, how should taxpayers fill up the field of "Reference Number of Customs Form No. 1, 9, etc."?

The format allows the taxpayers to include multiple customs form reference number, where applicable, under the "Reference Number of Customs Form No. 1, 9, etc." field. Refer to e-Invoice SDK Microsite for more details.

#### 88. Will the customs form reference number for transportation of goods between Peninsular Malaysia, Wilayah Persekutuan Labuan, Sabah and Sarawak be included in the e-Invoice?

No, taxpayers are not required to include the customs form reference number in the "Reference Number of Customs Form No. 1, 9, etc." for the purposes of e-Invoice.

---

## Part 3: e-Invoice for MSME

#### 89. Are MSMEs in Malaysia required to issue e-Invoice?

All persons conducting a business are required to implement e-Invoice in accordance with their respective implementation timeline as outlined under section 1.5 of the e-Invoice Guideline.

However, the Government of Malaysia has exempted taxpayers with annual turnover or revenue below RM1 million from the issuance of e-Invoice.

> **Example:** Amzah operates a sole proprietorship, AMZ Enterprise. As at 31 December 2024, his business recorded total annual turnover or revenue of below RM1 million. Since his business is still below the threshold, Amzah is exempted from issuing e-Invoice (including issuance of self-billed e-Invoice).

#### 90. Does the exemption apply to all MSMEs?

The exemption applies to all categories of taxpayers (e.g., individuals, partnerships, companies, cooperatives, etc.) with an annual turnover or revenue below RM1 million.

However, this exemption does not apply to the following taxpayers:

a) taxpayer with non-individual shareholder(s) (or equivalent) with annual turnover or revenue of at least RM1 million; or

b) taxpayer is a subsidiary of a holding company with annual turnover or revenue of at least RM1 million; or

c) taxpayer has related company\* / joint venture with annual turnover or revenue of at least RM1 million.

\* A "related company" has the meaning assigned to it in section 2 of the Promotion of Investments Act 1986.

> **Example:** RC Cycling Sdn. Bhd. closes its financial year on 31 December every year. RC Cycling Sdn. Bhd. is a subsidiary of JT Motors Sdn. Bhd., which has implemented e-Invoice. The annual revenue recorded for RC Cycling Sdn. Bhd. as at 31 December 2022 is RM400,000. Even though the annual turnover or revenue is recorded below the RM1 million threshold, RC Cycling Sdn. Bhd. does not qualify for the exemption because it is a subsidiary of another company (i.e., JT Motors Sdn. Bhd.) that is required to implement e-Invoice. As such, RC Cycling Sdn. Bhd. is required to implement e-Invoice starting from 1 July 2026.

#### 91. When will MSMEs be required to implement e-Invoice if their annual turnover or revenue have reached or exceeded the threshold of RM1 million?

MSME is required to implement e-Invoice starting from 1 January in the second year following the YA in which the total annual turnover or revenue reaches RM1 million.

> **Example:** Johan operates a business specialising in the sale of fertilisers, Winner Fertilizer Sdn. Bhd. and its accounting period ends every 31 December. As at 31 August 2026, his company already recorded an annual turnover or revenue exceeding RM1 million. Since the company has exceeded the exemption threshold, Winner Fertilizer Sdn. Bhd. is required to implement e-Invoice starting from 1 January 2028.

#### 92. What are the determination for the implementation of e-Invoice for MSMEs?

The implementation of e-Invoices is applicable to taxpayers with an annual turnover or revenue of at least RM1 million, determined as follows:

a) Taxpayers with audited financial statements: Based on annual turnover or revenue stated in the statement of comprehensive income in the audited financial statements for relevant year; or

b) Taxpayers without audited financial statements: Based on annual revenue reported in the tax return for year of assessment relevant year; or

c) Taxpayers with annual turnover or revenue of at least RM1 million for the relevant year.

> **Example:** Kedai Berkat Gemilang recorded product sales income of RM1.2 million as of 31 December 2026. In view that the total income earned has reached RM1 million, Kedai Berkat Gemilang is required to implement e-Invoice starting from 1 January 2028.

#### 93. For sole proprietorship, how will the mandatory implementation be determined if a sole proprietor has more than one business?

For sole proprietorship, the determination of annual turnover or revenue below RM1 million threshold includes all sole proprietorship businesses owned or registered under the name of the respective sole proprietor.

> **Example:** Awiyah owns several sole proprietorship businesses, namely RAM Cosmetic Enterprise, Princess Tailor and Fifty Cafe. The total annual turnover or revenue as of 31 December 2026 are as follows:
>
> | Business | Annual turnover or revenue (RM) |
> |---|---|
> | RAM Cosmetic Enterprise | 200,000 |
> | Princess Tailor | 550,000 |
> | Fifty Cafe | 280,000 |
> | **Total annual revenue or turnover** | **1,030,000** |
>
> In view that the total annual turnover or revenue exceeds RM1 million, Awiyah is required to implement e-Invoice starting from 1 January 2028.

#### 94. Does a taxpayer have a related company under the following scenario?

**(a) Taxpayers with a common corporate shareholder**

- Where the operations of the companies can be controlled, either directly or indirectly, by another company, the companies will be considered as related companies for e-Invoice purposes. A company holding at least 20% of the issued share capital in another company is treated as related company for e-Invoice purposes.
- However, even if the shareholding of the corporate shareholder in a company is below 20%, the companies will still be considered as related companies for e-Invoice purposes if the corporate shareholder has control over the operations of the companies.

**(b) Taxpayers with a common individual shareholder**

- Where the operations of two taxpayers (2) are controlled by the same individual shareholder, the taxpayers will not be considered as related companies for e-Invoice purposes.

**(c) Taxpayers with a common director who does not have any shareholding in the taxpayers**

- The mere presence of a common director does not result in two (2) companies being considered as related companies for e-Invoice purposes. For e-Invoice purposes, the assessment of whether companies are related is based on shareholding ownership and control at the shareholder level.

**(d) Taxpayers with a common individual shareholder who is also a common director of the taxpayers**

- Where two (2) companies are owned by the same individual shareholder, both companies will not be considered as related companies for e-Invoice purposes, even if the individual also serves as a director in both companies.

#### 95. Will MSMEs that have been mandated to implement e-Invoice be eligible for the exemption again if their annual turnover or revenue falls below the RM1 million threshold in the subsequent years?

No exemption will be granted after the mandatory implementation year has been determined, and taxpayers are required to continue issuing e-Invoices even if their total annual turnover or revenue do not exceed RM1 million in the subsequent years.

#### 96. For eligible taxpayers that are exempted from issuance of e-Invoice, are they required to issue consolidated e-Invoice and self-billed e-Invoice?

Eligible taxpayers who are exempted from the issuance of e-Invoice in accordance with section 1.6.1 of the e-Invoice Guideline are not required to issue consolidated e-Invoice and self-billed e-Invoice.

However, IRBM would encourage the exempted taxpayers to adopt e-Invoice on a voluntary basis.

#### 97. If MSME has an annual turnover or revenue below RM1 million and sells goods on a local e-commerce platform, is the MSME obligated to provide details to the e-commerce platform for issuance of e-Invoice?

Yes. This is because the obligation to issue e-Invoice / self-billed e-Invoice rests with the e-commerce platform provider, for all transactions conducted on the e-commerce platform.

#### 98. Can large enterprises compel MSMEs to issue e-Invoice prior to the MSMEs' mandatory implementation timeline (i.e., 1 July 2026)?

For clarity, the compliance obligation is from the issuance of e-Invoice perspective. Taxpayers who are within the annual turnover or revenue threshold specified in section 1.5 of the e-Invoice Guideline are required to issue and submit e-Invoice for IRBM's validation according to the stipulated implementation timeline.

Taxpayers who have yet to reach the mandatory implementation timeline are allowed to continue issuing existing documentation to record transactions. Hence, taxpayers who have already implemented e-Invoice cannot compel other taxpayers who have not reached their respective mandatory implementation timeline to issue an e-Invoice.

#### 99. Are MSMEs allowed to issue consolidated e-Invoice on a monthly basis to record all transactions within the month?

Yes, MSMEs are allowed to issue consolidated e-Invoice to record all transactions conducted in the previous month, except for activities / transactions stipulated under section 3.7 of the e-Invoice Specific Guideline.

#### 100. How can MSMEs issue consolidated e-Invoice if they currently do not issue receipts for their transactions?

MSMEs with annual gross takings exceeding RM150,000 for goods sold or for services performed are required to issue serially numbered receipts for all transactions as per section 82(1)(b) of the Income Tax Act 1967. These MSMEs are then required to include the receipt reference number in the consolidated e-Invoice issued for IRBM's validation.

MSMEs below the abovementioned threshold are not required to issue receipts and are not required to issue consolidated e-Invoice to IRBM.

#### 101. If MSME conduct sales through physical store as well as e-commerce platform, must MSME issue e-Invoices for transactions from both sources?

MSME is required to issue e-Invoices for sales conducted through physical store.

As for the sales conducted through e-commerce platform, MSME is not required to issue any e-Invoice as the obligation to issue e-Invoice / self-billed e-Invoice rests with the e-commerce platform provider.

#### 102. What support is available for MSMEs which do not have budget for new digital system to implement e-Invoice?

IRBM will provide support to MSMEs in complying with the e-Invoicing requirements via the MyInvois Portal and MyInvois Mobile App which will be made available to the public for free.

#### 103. Can MSMEs issue e-Invoice before their respective implementation timeline?

If MSMEs are ready to implement e-Invoice in advance of their implementation timeline, they are welcome to implement earlier than the set timeline.

---

## Part 4: e-Invoice Treatment During Interim Relaxation Period

#### 104. Is the e-Invoice treatment during interim relaxation period applicable to taxpayers with mandatory implementation dates on 1 January 2025, 1 July 2025, 1 January 2026 and 1 July 2026?

Yes, the 6-month interim relaxation period is applicable for all phases of e-Invoice implementation, as follows:

| Targeted Taxpayers | Interim Relaxation Period |
|---|---|
| Taxpayers with an annual turnover or revenue of more than RM100 million | 1 August 2024 to 31 January 2025 |
| Taxpayers with an annual turnover or revenue of more than RM25 million and up to RM100 million | 1 January 2025 to 30 June 2025 |
| Taxpayers with an annual turnover or revenue of more than RM5 million and up to RM25 million | 1 July 2025 to 31 December 2025 |
| Taxpayers with an annual turnover or revenue of up to RM5 million (i) 1 January 2026 implementation date (ii) 1 July 2026 implementation date | Until 31 December 2027 |

#### 105. Following the release of Income Tax (Issuance of Electronic Invoice) Rules 2024 [P.U. (A) 265], is the 6-month interim relaxation period for taxpayers with annual turnover or revenue of more than RM100 million now starting on 1 October 2024, instead of the previously stated 1 August 2024?

No, the commencement of the 6-month interim relaxation period for taxpayers with an annual turnover or revenue of more than RM100 million remains unchanged, i.e., from 1 August 2024. This interim relaxation period applies to all phases of e-Invoice implementation as stated in Section 16 of the e-Invoice Specific Guideline.

#### 106. Are all taxpayers required to adopt the e-Invoice treatment during the interim relaxation period, or do they have the option to issue individual e-Invoices and individual self-billed e-Invoices in accordance with the current requirements outlined in the e-Invoice Guideline and Specific Guideline if their system is ready?

The e-Invoice treatment during interim relaxation period is provided for taxpayers who have yet to be ready to issue individual e-Invoices and individual self-billed e-Invoices.

If a taxpayer's system is ready to issue individual e-Invoices / individual self-billed e-Invoices for each transaction, the taxpayer may opt to not adopt the e-Invoice treatment during interim relaxation period and issue individual e-Invoices and individual self-billed e-Invoices as per the requirements outlined in the e-Invoice Guideline and Specific Guideline.

#### 107. During the interim relaxation period, are taxpayers obligated to issue and submit consolidated e-Invoices / consolidated self-billed e-Invoices for IRBM's validation on a monthly basis or only once at the end of the 6-month period?

Taxpayers are still required to issue the consolidated e-Invoices / consolidated self-billed e-Invoices on a monthly basis, in accordance with the timing of issuance of consolidated e-Invoice and consolidated self-billed e-Invoice as stipulated under sections 3.6.2 and 3.6.6 of the e-Invoice Specific Guideline respectively.

#### 108. Are taxpayers required to issue individual self-billed e-Invoice / consolidated self-billed e-Invoice for importation of goods if the customs clearance is obtained prior to mandatory implementation date (e.g., prior to 1 August 2024 for phase 1 taxpayers)?

Where the customs clearance of the imported goods is obtained before the mandatory implementation date, there would not be any requirement to issue self-billed e-Invoice for the said imported goods.

#### 109. During the interim relaxation period, when should the phase 1 taxpayers issue individual self-billed e-Invoice / consolidated self-billed e-Invoice for importation of goods, if the customs clearance is obtained in the month of August 2024?

Where the customs clearance of the imported goods is obtained in the month of August 2024, the timing of issuance of self-billed e-Invoice are as follows:

(i) Individual self-billed e-Invoice: Latest by 31 October 2024, in accordance with section 10.4.8 of the e-Invoice Specific Guideline

(ii) Consolidated self-billed e-Invoice: Latest by 7 November 2024, in accordance with section 3.6.6 of the e-Invoice Specific Guideline

#### 110. Are taxpayers required to issue individual self-billed e-Invoice / consolidated self-billed e-Invoice for importation of services if the taxpayers have made payment for the imported services or if the foreign supplier's invoice is received prior to mandatory implementation date (e.g., prior to 1 August 2024 for phase 1 taxpayers)?

Where the taxpayer has made payment for the imported services or if the foreign supplier's invoice is received before the mandatory implementation date, there would not be any requirement to issue self-billed e-Invoice for the said imported services.

#### 111. During the interim relaxation period, when should the phase 1 taxpayers issue individual self-billed e-Invoice / consolidated self-billed e-Invoice for importation of services, upon (1) payment made by the taxpayer, or (2) receipt of invoice from foreign supplier, whichever earlier, in the month of August 2024?

If the earlier of the aforementioned events occurred in August 2024, the timing of issuance of self-billed e-Invoice are as follows:

(i) Individual self-billed e-Invoice: Latest by 30 September 2024, in accordance with section 10.4.9 of the e-Invoice Specific Guideline

(ii) Consolidated self-billed e-Invoice: Latest by 7 October 2024, in accordance with section 3.6.6 of the e-Invoice Specific Guideline

#### 112. Are taxpayers required to share the consolidated e-Invoice / consolidated self-billed e-Invoice with the buyers / suppliers?

The taxpayer is not required to share validated consolidated e-Invoice with the buyers / validated consolidated self-billed e-Invoice with the suppliers.

#### 113. Are taxpayers required to input the receipt numbers under the "Description of Product / Service" field for the issuance of consolidated e-Invoice / consolidated self-billed e-Invoice during the interim period?

No, taxpayers are allowed to input any description as appropriate when issuing consolidated e-Invoice / consolidated self-billed e-Invoice during the interim relaxation period.

---

## Part 5: Systems, Data Security and Privacy

### (i) Issuance of e-Invoice to MyInvois System via API and/or MyInvois Portal

#### 114. Can a company use a combination of transmission mechanisms (API and MyInvois Portal)?

Yes. Taxpayers are recommended to perform reconciliation to ensure no duplication of e-Invoice submitted to IRBM.

#### 115. Is there a file size limit for submission of e-Invoices to MyInvois System?

In order to improve the performance of the MyInvois System, the following limitations have been put in place:

a) maximum size of 5MB per submission;

b) maximum of 100 e-Invoices per submission; and

c) maximum size of 300KB per e-Invoice.

No restrictions on the number of lines in an e-Invoice will be imposed, as long as the requirements above are met.

For more information, please visit the e-Invoice SDK Microsite via the following link: <https://sdk.myinvois.hasil.gov.my/einvoicingapi/02-submit-documents/#additional-considerations>.

#### 116. Will IRBM provide any technical guide on system integration to the MyInvois System?

Yes, an e-Invoice SDK had been published on 9 February 2024.

The e-Invoice SDK is a collection of resources related to a set of technical functionalities, APIs, and development guidelines. The SDK aims to assist businesses in integrating their existing system to the MyInvois System via API.

Visit the e-Invoice SDK Microsite via the following link: <https://sdk.myinvois.hasil.gov.my>. For any e-Invoice SDK-related queries, submit to us via MyInvois Customer Feedback Form at <https://feedback.myinvois.hasil.gov.my>.

#### 117. Can taxpayers adopt Peppol as the transmission mechanism for e-Invoice?

Taxpayers are allowed to select any transmission method including using Peppol Service Providers, that is most suited to their business nature and preference. Any service providers in the market that can comply to IRBM's API requirements are welcomed.

Refer to Section 2.2 of the e-Invoice Guideline or MDEC's official portal at <https://mdec.my/> for further details.

For any queries regarding e-Invoicing using Peppol, kindly e-mail to clic@mdec.com.my.

#### 118. Is there a registration requirement for technology providers to participate in the implementation?

There is no registration requirement at this juncture. However, technology providers are responsible to ensure the functionality and reliability of their API integration with IRBM.

#### 119. Do technology providers need to apply for a certification in Malaysia in order to provide the CTC e-Invoice solution?

Not at this juncture. However, this may change in the future. Kindly refer to IRBM's Official Portal for any latest updates on the latest position.

#### 120. For taxpayers who are currently not using any system for invoicing purposes, how should the taxpayers adopt and implement e-Invoicing?

Taxpayers are encouraged to utilise the MyInvois Portal hosted by IRBM (which is provided free of charge to all taxpayers).

Alternatively, taxpayers may consider engaging third-party service providers to understand on their service offerings in facilitating taxpayers' adoption of e-Invoice.

#### 121. Is MyInvois Portal able to accept large volume of e-Invoices from taxpayers?

MyInvois Portal supports both individual and batch e-Invoice generation through spreadsheet upload for processing multiple transactions.

MyInvois System will undergo testing with actual estimated volume of e-Invoices before its go-live. Additionally, it is designed to scale up additional computing resources as and when required.

#### 122. Is there any specific application required to scan the QR code embedded in the visual representation of e-Invoice generated by MyInvois portal?

Any device (e.g., mobile phone camera, QR code scanner application) capable of scanning a QR code can be used to scan the QR code embedded in the visual representation of e-Invoice generated by MyInvois portal.

#### 123. What is the workaround in the event that the MyInvois System is down?

System will be available 99.97% of the time. However, in the event that IRBM's system is down, suppliers are given 72 hours to issue an e-Invoice once system is available. A retry mechanism shall be implemented in supplier's system to submit the e-Invoices once the system is available.

As for MyInvois Portal, supplier should periodically check the portal to determine if it is back online.

---

### (ii) Data security and data privacy

#### 124. Does Personal Data Protection Act 2010 govern the processing of e-Invoice?

Section 82C(11) of the Income Tax Act 1967 has outlined that the provisions of the Personal Data and Protection Act 2010 shall not apply to any personal data processed for e-Invoice issued or transmitted to the Director General.

#### 125. Does the API solution offer security and encryption services?

Yes, through the necessary Network & Security monitoring tools to ensure data security and privacy. Additionally, the API solution will utilise industry standard encryption protocol to ensure information transmitted remains confidential and secure.

#### 126. How would IRBM monitor and audit the e-Invoice data security and privacy?

IRBM adopts a high standard of data security in managing data of taxpayers. These are the steps that will be taken in monitoring and auditing the e-Invoice data security and privacy:

1. **Assess the data protection needs** — Before IRBM starts monitoring and auditing the e-Invoice data security and privacy, IRBM will identify what kind of data that IRBM collects, processes, stores, and shares through the e-Invoice system.

2. **Implementation of data protection controls** — In order to protect the e-Invoice data from unauthorised access, modification, loss, or disclosure, IRBM will implement appropriate technical and organisational controls. These may include encryption, authentication, access control, backup, firewall, antivirus, and logging.

3. **Monitoring and auditing data protection performance and incidents** — This can be done by benchmarking the performance against the objectives and industry best practices and reporting, investigating, resolving, and learning from any data breaches, errors, complaints, or violations that may affect the e-Invoice data.

4. **Review and improve the data protection practices** — IRBM will use the results of the monitoring and auditing activities to identify any gaps, weaknesses, or opportunities for improvement in the data protection policies, controls, performance, or incidents.

#### 127. What are the measures taken by IRBM in protecting the confidentiality of e-Invoice submitted to IRBM?

IRBM will ensure that the data are always secured by complying with Dasar Keselamatan ICT IRBM and Surat Pekeliling Am: Garis Panduan Pengurusan Keselamatan Maklumat Melalui Perkomputeran Awan (Cloud Computing) Dalam Perkhidmatan Awam.

To protect e-Invoice from unauthorised access, modification, loss, or disclosure, IRBM will comply to industry standards and implement appropriate cybersecurity measures including encryption, authentication, access controls, firewalls, and more.

Additionally, IRBM will ensure that the system is in compliance and certified with ISO/IEC 27001 Information Security Management System (ISMS) and ISO 22301 Business Continuity Management System (BCMS).

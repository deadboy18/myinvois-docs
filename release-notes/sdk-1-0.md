<!-- source: https://sdk.myinvois.hasil.gov.my/sdk-1-0-release/ -->

# SDK 1.0 Release

10 Dec 2025 - Lembaga Hasil Dalam Negeri Malaysia

Lembaga Hasil Dalam Negeri Malaysia has provisioned SDK 1.0 Release to help taxpayers in preparing for the upcoming MyInvois System. SDK Updates section below highlights key updates as part of this release.

# Base URLs

In order for taxpayers to access the URLs for the various environments, please ensure that root certificate is trusted by the machine used to avoid any security-related issues accessing these URLs. See more in [FAQ](../08-faq/README.md#how-to-set-up-environment-to-access-test-apis-and-test-portals).

# SDK Updates

**08 July 2026**

**Introduction to New Document Versions for e-Invoice Special Voluntary Disclosure Programme (SVDP)**

• e-Invoice Special Voluntary Disclosure Programme (SVDP) has been introduced as an ongoing effort to support taxpayers in regularising their e-Invoice compliance, effective period until 31 December 2027.

• To support the implementation of the programme, HASiL has introduced new document versions in the MyInvois Software Development Kit (SDK).

  - SVDP 1.2 – without digital signature
  - SVDP 1.3 – with digital signature

Taxpayers and service providers are advised to update their systems in accordance with the latest SDK specifications and refer to the e-Invoice Guideline and Specific Guidelines for the applicable requirements and procedures under e-Invoice SVDP.

• Taxpayers and service providers are advised to ensure the following:

  a. Update your integration to support the new document versions (SVDP 1.2 and SVDP 1.3)

  b. The document version SVDP 1.2 and SVDP 1.3 can only be used during the effective period of the e-Invoice SVDP.

  c. For Batch Upload users, please download and use the latest version of the Batch Upload template that supports the document version SVDP 1.2 and SVDP 1.3 before submitting e-Invoices.

  d. All existing document validation rules remain applicable unless otherwise specified in the SDK.

Please refer to the sample below for reference:

- Invoice SVDP 1.2 (without digital signature) Sample XML
- Invoice SVDP 1.2 (without digital signature) Sample JSON
- Invoice SVDP 1.3 (with digital signature) Sample XML
- Invoice SVDP 1.3 (with digital signature) Sample JSON

Please ensure your system is updated to support SVDP 1.2 or SVDP 1.3 to facilitate participation in the e-Invoice SVDP, enabling taxpayers to rectify inaccuracies and regularise compliance with HASiL's e-Invoice requirements.

**12 June 2026**

**Latest Update on Validate Taxpayer's TIN API**

• To enhance data accuracy and quality, HASiL will introduce Taxpayer Identification Number (TIN) and Business Registration Number (BRN) validation starting **1 August 2026**.

This validation will be implemented for the Validate Taxpayer's TIN API, and taxpayers are advised to follow the requirements outlined in the Software Development Kit (SDK) in preparation for this update.

Taxpayers are advised to ensure the following:

  a) Obtain updated and accurate BRN information from Buyers

  b) Remind buyers to update their latest BRN information with HASiL to ensure consistency between the BRN provided to your company and HASiL records

**30 April 2026**

**Updates to Fields Validation**

• Effectively immediately, State Code 17 can only be used in the following cases:

  a) Consolidated e-Invoices issued in Malaysia, or

  b) e-Invoices for transactions outside Malaysia.

• Scientific notation is not supported in any amount fields. All amount values must be provided in standard numeric format using plain numbers with optional decimal points.

**10 April 2026**

**Other Updates**

â¢ Added âHectareâ to the list of Unit of Measurements in Codes page. Kindly refer to
https://sdk.myinvois.hasil.gov.my/codes/unit-types/

**6 February 2026**

**Other Updates**

â¢ The SSL certificate for [myinvois.hasil.gov.my](https://myinvois.hasil.gov.my/) will be renewed on 8 February 2026.

**10 December 2025**

**Updates to Fields Validation**

â¢ To enhance data integrity, taxpayers will be required to comply with the validation rules for the following fields:

Â Â a) **Date fields** will only accept valid date formats (YYYY-MM-DD). Entries such as âN/Aâ will no longer be permitted.

Â Â b) **Supplierâs Bank Account number** must not exceed 150 characters.

Â Â c) **e-Invoice Code/Number** must not exceed 50 characters.

Â Â d) **Authorisation Number for Certified Exporter** must not exceed 300 characters.

Â Â e) **Incoterms** must not exceed 3 characters.

Â Â f) **Frequency of Billing** must not exceed 50 characters.

Â Â g) **Unit of Measurement** must follow unit code stated **[here](../05-code-tables/unit-types.md)**.

Â Â h) **Supplierâs Business Activity Description** must not exceed 300 characters.

Â Â i) **Payment Terms** must not exceed 300 characters.

Â Â j) **PrePayment Reference Number** must not exceed 150 characters.

Please ensure your systems are updated to align with these rules ahead of the upcoming changes. Submissions that do not meet the validation requirements may not be processed successfully.

These changes will take effect in the **Sandbox environment on 15 December 2025**. Deployment to the **Production environment has been postponed until further notice.**

**9 August 2025**

**Other Updates**

â¢ With reference to the release note issued on 1 August 2025, which specified the requirement to include the **[Currency Exchange Rate](../04-document-types/invoice-v1-0.md#core)** element when the Invoice Currency Code is not equal to MYR, please be advised of the following timeline:

The validation rule will be applied in the  **Sandbox environment effective 9 August 2025**, and in the  **Production environment effective 1 September 2025**, as previously communicated.

To avoid submission errors, ensure that all e-Invoice payloads involving foreign currency include the **[Currency Exchange Rate](../04-document-types/invoice-v1-0.md#core)** element in compliance with the specified validation rule.

**1 August 2025**

**Updates to API Documentation**

â¢ Added new optional input parameter for Search Taxpayerâs TIN API to enable filtering of
results by entity type (individual or non-individual).

**Other Updates**

â¢ To ensure successful submission, please note that if the **Invoice Currency Code is not equal to MYR**, the payload must include the **[Currency Exchange Rate](../04-document-types/invoice-v1-0.md#core)** element. Submissions will be rejected if **Currency Exchange Rate is missing under these conditions**. Please verify your payload before submission to avoid errors as this validation rule will be **effective from 1 September 2025**.

**4 July 2025**

**Other Updates**

â¢ Added Crimea and Scotland to the list of countries under âCountry Codesâ  
â¢ Updated Cape Verde to Cabo Verde  
â¢ Removed â(Dutch Part)â from Sint Maarten (Dutch Part)

Changes will be effective from 8 July 2025.

**13 June 2025**

**Updates to API Documentation**

â¢ Updated the maximum number of documents that can be returned by Search Document API to 10,000.

**Other Updates**

â¢ The use of state code â00â is no longer applicable for e-invoice issuance via API / Batch Upload. Please refer to the SDK for the appropriate state codes.

**16 May 2025**

**Other Updates**

â¢ Added new currency code CNH for Yuan Renminbi international trade. The description for code CNY has been amended to differentiate usage for domestic trade.

**28 April 2025**

**Other Updates**

â¢ Kindly take note that data in the sandbox environment will be retained for a maximum of 3 months, after which it will be permanently deleted. Please ensure to back up any important data before the retention period expires.

**25 April 2025**

**Other Updates**

â¢ Please note that the sandbox environment is intended for functional testing and has a lower API rate limit compared to the production environment, effective 28 April 2025.

**11 April 2025**

**Updates to API Documentation**

â¢ Added new API definition for Taxpayerâs QR Code in e-Invoice API section.

**7 February 2025**

**Updates to the XML and JSON samples**

â¢ Added General TIN âEI00000000010â to Consolidated Sample XML and Consolidated Sample JSON.  
â¢ Updated Multi Line Item Sample XML and Multi Line Item Sample JSON with different Tax Type, Tax Rate and Tax Exemption.

**16 January 2025**

**Other Updates**

â¢ The SSL certificate for [myinvois.hasil.gov.my](https://myinvois.hasil.gov.my/) was renewed on 16 January 2025.

**14 January 2025**

**Updates to the API Documentation**

â¢ A rate limit has been added to the [Integration Practices FAQ](../01-getting-started/integration-practices.md#rate-limit) section.

**Other Updates**

â¢ Rate limits and important notes have been introduced for the following e-invoice APIs:

* Log in as Taxpayer System
* Log in as Intermediary System
* Submit Documents
* Cancel Document
* Reject Document
* Get Recent Documents
* Get Submission
* Get Document
* Get Document Details
* Search Documents
* Search Taxpayerâs TIN

â¢ We have updated the best practices for optimising API rate limits to ensure optimal performance. The new rate limit will be enforced on May 30, 2025.

**28 December 2024**

**Updates to API Documentation**

â¢ Added new API definition for Search Taxpayersâ TIN in e-Invoice API section.

**Other Updates**

â¢ Added unit code for gross ton (GT) to the system.  
â¢ Updated the sample JSON and XML files for invoice versions 1.0, 1.1 and Signature creation json.

**10 October 2024**

**Updated Validations & Documentation**

â¢ Updated support of Multiline Invoice sample in XML/JSON and the inclusion of MSIC subcategories.

**Updates to API Documentation**

â¢ Removed the Channel parameter in Get Notifications API.  
â¢ Updated Notification Status and Notification Types for Get Notifications.  
â¢ Changed parameter âdirectionâ to âInvoiceDirectionâ.

**Other Updates**

â¢ Added Netherlands Antilles And Kosovo to list of countries.

**10 August 2024**

**Updates to Signature**

â¢ Included Powershell Script under Signature Creation section for additional guidance on signing JSON file for Version 1.1 document types.

**28 June 2024**

**Updated Validations & Documentation**

â¢ Updated to align API Response and Standard Error Response.  
â¢ Updated the Document Type examples to specify âv1.1â instead of âv1.0â.  
â¢ Removed âContinuation Tokenâ from Search Documents API.

**Updates to Signature**

â¢ Updated detailed step on encoding the hashed property tag using HEX-to-Base64 Encoder in Document Signature Creation.  
â¢ Updated the steps in Signing a JSON Document.

**New Additions**

â¢ Added Signature Page and Relevant Samples for v1.1.  
â¢ Added Type Page and Relevant Samples for v1.1.  
â¢ Added new page (Sample) containing sample payload for each document type.  
â¢ Updated response to the Get All Document Types, Reject Document and Get Recent Documents APIs.

**Other Updates**

â¢ Updated Contacts page.

**21 JUN 2024**

**Updated Validations & Documentation**

â¢ Updated Actual API deployed to Sandbox environment.  
â¢ Updated the UBL Sample XML and JSON for Foreign Currency and Multi Line Item.  
â¢ Added a pagination URL parameter to the Get Submission.  
â¢ Included clarification on Tax Rate.  
â¢ Updated Shipping Recipient fields from âMandatory where Applicableâ to âOptionalâ.  
â¢ Updated SST Registration Number to a maximum character count of 35 and provided support for the special character â semicolon (;).

**New Additions**

â¢ Implement Minification of XML / JSON documents within the Submit Document API.

**Other Updates**

â¢ Updated the FAQ:

* Added guidance on QR code generation.
* Updated the title from Error 404 to Error 400 and replaced BadArgument with BadRequest.
* Updated TIN parameters.

**24 May 2024**

**Updated Validations & Documentation**

â¢ Amended Number of Characters of Supplier & Buyer Contact Number.  
â¢ Refined note on Tax Rate.  
â¢ Updated Percent node on Tax Rate.

**Updates to Signature**

â¢ Updated sample and explanation of actual Digital Certificate JSON.

**New Additions**

â¢ Added New Sample XML for Foreign Currency with Tax Exchange Rate and Multi Line Item.  
â¢ Added X-Rate Limit Description in FAQ.  
â¢ Added new page for Signing Document JSON in Document Signature Creation page.  
â¢ Added new code âEâ under Tax Types.

**Other Updates**

â¢ Included General, Signature and Postman API Guidance subpages under FAQ along with Integration Practices.

**10 May 2024**

**Updated Validations & Documentation**

â¢ Amended descriptions for several fields including Fee / Charge Rate, Fee / Charge Amount, Total Discount Value, and Invoice Additional Fee Amount.  
â¢ Updated available APIs in the sandbox environment under FAQs.  
â¢ Updated cardinality of fields including Quantity, Measurement, Reference Number of Customs Form No.1, 9, etc., SST Registration Number and Tourism Tax Registration Number.  
â¢ Updated number of characters for Measurement field to 3 characters.  
â¢ Refined descriptions for fields such as Tax Type, City Name, Payment Mode, and Prepayment Time.

**Updates to Signature**

â¢ Removal of Timestamp Authority (TSA) requirement of XAdES for digital signature.  
â¢ Provided clearer guidance on hashing with SHA256 followed by HEX-to-Base64 encoding.  
â¢ Updated sample in SDK with RSA-SHA256.

**New Additions**

â¢ Added Measurement Code Table under Codes.

**Other Updates**

â¢ Improved visual for API input parameters with new column on optional / mandatory

**19 April 2024**

**Document Types:**

â¢ Updates to cardinality and number of characters for all document types.  
â¢ Guidance on Taxpayersâ details for MyPR and MyKAS to adhere to NRIC scheme.  
â¢ Updated guidance on address section table.

**e-Invoice API:**

â¢ Updated IdType input parameter to list all possible values.  
â¢ Added new âMandatory/Optionalâ column for each API input parameter.

**6 April 2024**

**Updated Validations & Documentation:**

â¢ Updated Types > Invoice v1.0 > All data fields, and Types > Invoice v1.0 > Field descriptions.  
â¢ Updated applicable fields mandatory in Types > Invoice v1.0 > Data Structure.  
â¢ Updated descriptions in Types > Invoice v1.0 > Data Structure.  
â¢ Provided guidance for non-registrants in Types > Invoice v1.0 > Data Structure.  
â¢ Implemented UBL2.1 Sequencing to improve validation and readability.  
â¢ Updated Description field for Fee / Charge Rate.

**New Additions:**

â¢ Added new code â044 - Vouchers, gift cards, loyalty points, etcâ in âCodesâ.  
â¢ Added Tax Exemption Field in Invoice and InvoiceLine.  
â¢ Updated guidance with Digital Signature Creation and Validation, along with sample.

**Updates to API Documentation:**

â¢ Updated API documentation for e-Invoice functionality.  
â¢ Clarified error responses and inputs for various API endpoints.  
â¢ Updated list of Sandbox APIs and Sandbox Identity Service URL on the FAQ page.  
â¢ Updates to writeup of HTTP 400 Error Responses for multiple APIs.

**Other Updates:**

â¢ Improved documentation on document types, hashing, and getting started.  
â¢ Updates to currency exchange rate and file size limitations.  
â¢ Updates to mathematical mapping.

# Document Version Updates

Addition of Version 1.1 for all Document Types for Signature Validation.


---

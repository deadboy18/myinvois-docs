<!-- source: https://sdk.myinvois.hasil.gov.my/signature-creation/ -->

# Document Signature Creation

Page describes the the algorithm of how signatures for documents submitted should be created.

# High-Level Process

The overall steps to take when preparing a single document for submission are:

* **Step 1:** Create document XML or JSON (no signature elements yet) for a single document
* **Step 2:** Apply transformations to the document
* **Step 3:** Canonicalize the document and generate the document hash (digest) (property reference **DocDigest**)
* **Step 4:** Sign the document digest (property reference **Sig**)
* **Step 5:** Generate the certificate hash (Digest) (property reference **CertDigest**)
* **Step 6:** Populate the signed properties section
* **Step 7:** Generate Signed Properties Hash (Digest) (property reference **PropsDigest**)
* **Step 8:** Populate the information in the document to create the signed document

# Detailed Steps

## Step 2: Apply transformations to the document

1. Make sure the source document is in the UTF-8 format.
2. Remove the XML version.
3. Remove the not required elements if any exists (UBLExtension (XPath: *[local-name()=×³Invoice×³]//*[local-name()=×³UBLExtensions×³]), and Signature (XPath: *[local-name()=×³Invoice×³]//*[local-name()=×³Signature×³]))

## Step 3: Canonicalize the document and generate the document hash (digest)

1. Prepare document canonical version, to be used for signing:
   * XML format - apply [xml-c14n11](https://www.w3.org/TR/xml-c14n11/) canonicalization to the document
   * JSON format - minify the document by removing following elements from it:
     + Whitespaces
     + Line breaks
     + Comments
2. Hash the canonicalized document invoice body using SHA-256.
   * Use a HEX-to-Base64 encoder to encode the hashed value and convert it to base 64.
3. The output will be set as the value of the property **DocDigest**

## Step 4: Sign the document digest

1. Sign the generated invoice hash with RSA-SHA256 using the signing certificate private key
2. The output will be set as the value of the property **Sig**
3. Encode the hashed property tag using Base64 Encoder

## Step 5: Generate the certificate hash

1. Hash the signing certificate using SHA-256
2. Encode the certificate hash using Base64 encoding

## Step 6: Populate the signed properties section

Update the document, being signed, by filling-in the following properties as per the below guidance:

| Field | Value | UBL Mapping Path |
| --- | --- | --- |
| DigestValue | Encoded certificate hashed property reference **CertDigest** | /Invoice /ext:UBLExtensions /ext:UBLExtension /ext:ExtensionContent /sig:UBLDocumentSignatures /sac:SignatureInformation /ds:Signature /ds:Object /xades:QualifyingProperties / xades:SignedProperties/xades:SignedSignatureProperties / xades:SigningCertificate /xades:Cert /xades:CertDigest /ds:DigestValue |
| SigningTime | Sign timestamp as current datetime in UTC | /Invoice /ext:UBLExtensions /ext:UBLExtension /ext:ExtensionContent /sig:UBLDocumentSignatures /sac:SignatureInformation /ds:Signature /ds:Object /xades:QualifyingProperties /xades:SignedProperties /xades:SignedSignatureProperties /xades:SigningTime |
| X509IssuerName | Signing certificate issuer name | /Invoice /ext:UBLExtensions /ext:UBLExtension /ext:ExtensionContent /sig:UBLDocumentSignatures /sac:SignatureInformation /ds:Signature /ds:Object /xades:QualifyingProperties / xades:SignedProperties /xades:SignedSignatureProperties / xades:SigningCertificate /xades:Cert /xades:IssuerSerial /ds:X509IssuerName |
| X509SerialNumber | Signing certificate serial number | /Invoice /ext:UBLExtensions /ext:UBLExtension /ext:ExtensionContent /sig:UBLDocumentSignatures /sac:SignatureInformation /ds:Signature /ds:Object /xades:QualifyingProperties / xades:SignedProperties /xades:SignedSignatureProperties / xades:SigningCertificate /xades:Cert /xades:IssuerSerial /ds:X509SerialNumber |

> Now the signing properties section should be ready to be hashed.

## Step 7: Generate Signed Properties Hash

1. Get the properties tag only using the XPath (/Invoice/ext:UBLExtensions/ext:UBLExtension/ext:ExtensionContent/sig:UBLDocumentSignatures/sac:SignatureInformation/ds:Signature/ds:Object/xades:QualifyingProperties/xades:SignedProperties)
2. Linearize the XML block (properties tag) and remove the spaces
3. Hash the property tag using SHA-256.
4. Encode the hashed property tag using Base64 Encoder
5. The value generated would be the property named **PropsDigest**

## Step 8: Populate the information in the document to create the signed document

Populate the properties gathered into the document as per the below table to generate the signed document.

| Field | Value | UBL Mapping Path |
| --- | --- | --- |
| SignatureValue | Digital Signature property ref **Sig** | /Invoice /ext:UBLExtensions /ext:UBLExtension /ext:ExtensionContent /sig:UBLDocumentSignatures /sac:SignatureInformation /ds:Signature /ds:SignatureValue |
| X509Certificate | Certificate | /Invoice /ext:UBLExtensions /ext:UBLExtension /ext:ExtensionContent /sig:UBLDocumentSignatures /sac:SignatureInformation /ds:Signature /ds:KeyInfo /ds:X509Data /ds:X509Certificate |
| DigestValue | Encoded signed Properties hash property reference **PropsDigest** | /Invoice /ext:UBLExtensions /ext:UBLExtension /ext:ExtensionContent /sig:UBLDocumentSignatures /sac:SignatureInformation /ds:Signature /ds:SignedInfo /ds:Reference[@URI='#id-xades-signed-props'] /ds:DigestValue |
| DigestValue | Encoded invoice hash property reference **DocDigest** | /Invoice /ext:UBLExtensions /ext:UBLExtension /ext:ExtensionContent /sig:UBLDocumentSignatures /sac:SignatureInformation /ds:Signature /ds:SignedInfo /ds:Reference[@Id='id-doc-signed-data'] /ds:DigestValue |
| Signature | An electronic signature to authenticate the e-Invoice | / ubl:Invoice / cac:Signature |

> **Note**: To ensure data transfer over network, potential newline symbols or spaces added removed between XML and JSON elements are not changing the signature value, the solution leverages specialized data document canonicalization approach to ensure only significant data (names and values of fields) is used as part of the signature.

Once multiple documents are prepared as per description above they all need to be added into `documents` array when JSON is used and into `submission/documents` element when XML is used. After that the submission is ready to be sent to the `MyInvois` System by calling [Submit Document API](../03-einvoice-api/02-submit-documents.md).

## Additional Consideration

The JSON document digital signature creation has been also clarified in this page : [Signature Creation JSON](signature-creation-json.md)

A sample PowerShell script for document digital signature creation can be found in this page : [Powershell Script For Signing JSON File](../10-sample-documents/samples/Digital_Signature_User_Guide.pdf).


---

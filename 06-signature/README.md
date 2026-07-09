<!-- source: https://sdk.myinvois.hasil.gov.my/signature/ -->

# Signature

Page describes the signature elements, creation and validation.

[FAQ](../08-faq/README.md)
[Postman](../09-postman/README.md)

## Introduction

This page provides guidance on the methods and approach that can be used to create digital signature for a document. This follows UBL 2.1 published standards as document here ([Universal Business Language Version 2.1](https://docs.oasis-open.org/ubl/UBL-2.1.html)) for XML and here ([UBL 2.1 JSON Alternative Representation Version 2.0](https://docs.oasis-open.org/ubl/UBL-2.1-JSON/v2.0/cnd01/UBL-2.1-JSON-v2.0-cnd01.html)) for JSON.  
The methods described in this page apply only to the documents that are submitted using the APIs described in this SDK as per the description here [Submit Documents](../03-einvoice-api/02-submit-documents.md)

> The submitter of the documents will have to use a valid digital certificate that is issued by a certificate authority (CA) in Malaysia as documented here [List of Certification Authorities and Recognition](https://www.mcmc.gov.my/en/sectors/digital-signature/list-of-licensees).

### Digital Signing Certificate Profile

The digital certificate used to digitally sign the submitted documents should be a valid X.509 certificate that should follow the below certificate profile.

#### Certificate Distinguished Name

Certificate distinguished name must have elements listed in the table below:

| Display Name | OID | Expected Content | Value Example |
| --- | --- | --- | --- |
| Common name (CN) | 2.5.4.3 | The company or organization name. | Company Name Sdn Bhd |
| Country (C) | 2.5.4.6 | The country of the organization - 2-letter ISO code. | MY |
| Email (E) | 1.2.840.113549.1.9.1 | (Optional) an email for the organization. | noemail@org.com.my |
| Organization (O) | 2.5.4.10 | The company or organization name. | Company Name Sdn Bhd |
| Organization identifier | 2.5.4.97 | The Tax Identification Number of the organization (TIN) | C20830570210 |
| Organization Unit (OU) | 2.5.4.11 | (Optional) A value representing an organization unit in the organization. |  |
| Serial number (serialNumber) | 2.5.4.5 | The business registration number (BRN) of the organization that is linked to the TIN provided above. | 202005123456 |

#### Key Usage and Enhanced Key Usage (aka Extended Key Usage)

To ensure digital signature level, following certificate extensions must be set:

* Key Usage - "Non-Repudiation (40)". Note that additional key usages can also be specified, but "Non-Repudiation (40)" must be present.
* Enhanced Key Usage (aka Extended Key Usage) - "Document Signing (1.3.6.1.4.1.311.10.3.12)". Note that additional enhanced key usages (e.g., "Secure Email (1.3.6.1.5.5.7.3.4)") can also be specified, but "Document Signing (1.3.6.1.4.1.311.10.3.12)" must be present.

## Digital Signature Creation Requirements and Considerations

The list below provides the requirements for the digital signature creation along with additional considerations.

* The digital signature algorithm to be used must be **XAdES** (XML Advanced Electronic Signature). (Digital signature timestamp will still be required to be added as part of the signed properties section as listed below). This envelope allows ascertaining the validity of a signature in case the signer certificate is later revoked.
* The hashing algorithm must be SHA 256.
* The signature algorithm must be RSA.
* UBL 2.1 XML standard includes by definition the required digital signature standard that would be used as described here [UBL Digital Signature Profiles 1.0](https://docs.oasis-open.org/ubl/prd2-UBL-2.1/doc/dsig/cd11-UBL-DSig-1.0.html)
* The **Enveloped** digital signature profile, where the signature applies to the XML content that contains `<ds:Signature>` as an element. Implementations of enveloped signature must make sure not to include `<ds:Signature>` element and any of it's sub-elements in the calculation of the digest and signature values as per the algorithm that will be described later.
* UBL 2.1 JSON Alternative Representation Version 2.0 doesn't define a specification for the use of digital signatures in JSON documents for UBL, this is supported using a foreign extension which is a transliteration of the UBL extension for digital signatures.
* Although UBL 2.1 supports multiple signatures to be included in the document, the requirement is to include only a single signature element which will be described later.

## Digital Signature Structure

| ELEMENT | Field TYPE | DESCRIPTION | VALUE EXAMPLE | UBL Schema Mapping | Mandatory | Cardinality |
| --- | --- | --- | --- | --- | --- | --- |
| [Signature Element](#signature-element) | Structure | This is the root signature element that represents the digital signature information. |  | /ubl:Invoice /ext:UBLExtensions /ext:UBLExtension /ext:ExtensionContent /sig:UBLDocumentSignatures /sac:SignatureInformation /ds:Signature[@Id = 'DocSig'] | Mandatory | [1..1] |

> Please note that the `ExtensionURI` should be set as per the value below:
>
> ```
> <ext:ExtensionURI>urn:oasis:names:specification:ubl:dsig:enveloped:xades</ext:ExtensionURI>
> ```

## Signature Element

| ELEMENT | Field TYPE | DESCRIPTION | VALUE EXAMPLE | UBL Schema Mapping | Mandatory | Cardinality |
| --- | --- | --- | --- | --- | --- | --- |
| [Signed information](#signed-information) | Structure | This contains information on how to transform the document to retrieve the information that are covered by the digital signature. This includes any transformations performed. The signed information would cover all the document content except specific sections as will be presented later in the signature creation section. |  | /ubl:Invoice /ext:UBLExtensions /ext:UBLExtension /ext:ExtensionContent /sig:UBLDocumentSignatures /sac:SignatureInformation /ds:Signature /ds:SignedInfo | Mandatory | [1..1] |
| Signature value | xsd:base64Binary | This contains the signature value that was generated by signing the document digest within the signed information section above. [Reference field: **Sig**] | `MEQCIGvLa1f3uMCe0AidKUWJ5ghMiDMRcC0qO78 ntcTKVOYgAiAKBkX+uuFhbIcye3JznNa45qH1twlLFu/ qPzEQ9HMNLw==` | /ubl:Invoice /ext:UBLExtensions /ext:UBLExtension /ext:ExtensionContent /sig:UBLDocumentSignatures /sac:SignatureInformation /ds:Signature /ds:SignatureValue [@ID = 'DocSigValue'] | Mandatory | [1..1] |
| Key information | Structure | This structure contains the X509 compliant certificate public information that was used to sign the document. | <ds:KeyInfo>  <ds:X509Data>  <ds:X509Certificate>MIID3jCCA4SgAwIBAgITEQAAOAPF90A.../rXA==</ds:X509Certificate>  </ds:X509Data> </ds:KeyInfo> | /ubl:Invoice /ext:UBLExtensions /ext:UBLExtension /ext:ExtensionContent /sig:UBLDocumentSignatures /sac:SignatureInformation /ds:Signature /ds:KeyInfo | Mandatory | [1..1] |
| [Object element](#signed-properties) | Structure | This would contain the signed properties object that would be hashed and included in the signed information object. The object element that would be required is `xades:QualifyingProperties` |  | /ubl:Invoice /ext:UBLExtensions /ext:UBLExtension /ext:ExtensionContent /sig:UBLDocumentSignatures /sac:SignatureInformation /ds:Signature /ds:Object | Mandatory | [1..1] |

## Signed Information

| ELEMENT | Field TYPE | DESCRIPTION | VALUE EXAMPLE | UBL Schema Mapping | Mandatory | Cardinality |
| --- | --- | --- | --- | --- | --- | --- |
| Canonicalization Method | Empty | Name of the XML canonicalization algorithm - `xml-c14n11`. | <ds:CanonicalizationMethod Algorithm="https://www.w3.org/TR/xml-c14n11/#"/> | /ubl:Invoice /ext:UBLExtensions /ext:UBLExtension /ext:ExtensionContent /sig:UBLDocumentSignatures /sac:SignatureInformation /ds:Signature /ds:SignedInfo /ds:CanonicalizationMethod [@Algorithm = 'https://www.w3.org/TR/xml-c14n11/#'] | Mandatory | [1..1] |
| Signature Method | Empty | This is the signature creation method which should be set to rsa-sha256. | <ds:SignatureMethod Algorithm="http://www.w3.org/2001/04/xmldsig-more#rsa-sha256"/> | /ubl:Invoice /ext:UBLExtensions /ext:UBLExtension /ext:ExtensionContent /sig:UBLDocumentSignatures /sac:SignatureInformation /ds:Signature /ds:SignedInfo /ds:SignatureMethod [@Algorithm = 'http://www.w3.org/2001/04/xmldsig-more#rsa-sha256'] | Mandatory | [1..1] |
| [Document Signed Data](#document-signed-data) | Structure | This would contain the information that would be signed, which would be the document digest and related details on how that digest was computed. |  | /ubl:Invoice /ext:UBLExtensions /ext:UBLExtension /ext:ExtensionContent /sig:UBLDocumentSignatures /sac:SignatureInformation /ds:Signature /ds:SignedInfo /ds:Reference[@Id = 'id-doc-signed-data' AND @URI = ''] (The URI has to be given as empty to refer to the parent enveloping document) | Mandatory | [1..1] |
| [XAdES Signed Properties](#xades-signed-properties) | Structure | This contains the signed properties representing the hash of the information of the certificate that was used to sign the document. |  | /ubl:Invoice /ext:UBLExtensions /ext:UBLExtension /ext:ExtensionContent /sig:UBLDocumentSignatures /sac:SignatureInformation /ds:Signature /ds:SignedInfo /ds:Reference[@URI = '#id-xades-signed-props'] | Mandatory | [1..1] |

## Document Signed Data

| ELEMENT | Field TYPE | DESCRIPTION | VALUE EXAMPLE | UBL Schema Mapping | Mandatory | Cardinality |
| --- | --- | --- | --- | --- | --- | --- |
| Transforms | Structure | The value of this field should be given exactly as per the sample value provided. | <ds:Transform Algorithm="http://www.w3.org/TR/1999/REC-xpath-19991116">        <ds:XPath>not(//ancestor-or-self::ext:UBLExtensions)</ds:XPath>   </ds:Transform>   <ds:Transform Algorithm="http://www.w3.org/TR/1999/REC-xpath-19991116">        <ds:XPath>not(//ancestor-or-self::cac:Signature)</ds:XPath>   </ds:Transform>   <ds:Transform Algorithm="http://www.w3.org/2006/12/xml-c14n11"/> | /ubl:Invoice /ext:UBLExtensions /ext:UBLExtension /ext:ExtensionContent /sig:UBLDocumentSignatures /sac:SignatureInformation /ds:Signature /ds:SignedInfo /ds:Reference[@Id = 'id-doc-signed-data' AND @URI = ''] / ds:Transforms | Mandatory |  |
| Digest Method | Empty | The value of this field should be given exactly as per the sample value provided. | <ds:DigestMethod Algorithm="http://www.w3.org/2001/04/xmlenc#sha256"/> | /ubl:Invoice /ext:UBLExtensions /ext:UBLExtension /ext:ExtensionContent /sig:UBLDocumentSignatures /sac:SignatureInformation /ds:Signature /ds:SignedInfo /ds:Reference[@Id = 'id-doc-signed-data' AND @URI = ''] / ds:DigestMethod | Mandatory | [1..1] |
| Digest | xsd:base64Binary | This is the document digest value that would be created using HEX-to Base64 Encoder applied to the entire transformed document content. This is the value that would be signed using the private key. [Reference field: **DocDigest**] | `oRtv5YelD32v/jp/tC3MzfG0PimzfZ8lLQQkPlTBGj8=` | /ubl:Invoice /ext:UBLExtensions /ext:UBLExtension /ext:ExtensionContent /sig:UBLDocumentSignatures /sac:SignatureInformation /ds:Signature /ds:SignedInfo /ds:Reference[@Id = 'id-doc-signed-data' AND @URI = ''] / ds:DigestValue | Mandatory | [1..1] |

## XAdES Signed Properties

| ELEMENT | Field TYPE | DESCRIPTION | VALUE EXAMPLE | UBL Schema Mapping | Mandatory | Cardinality |
| --- | --- | --- | --- | --- | --- | --- |
| Digest Method | Empty | The value of this field should be given exactly as per the sample value provided. | <ds:DigestMethod Algorithm="http://www.w3.org/2001/04/xmlenc#sha256"/> | /ubl:Invoice /ext:UBLExtensions /ext:UBLExtension /ext:ExtensionContent /sig:UBLDocumentSignatures /sac:SignatureInformation /ds:Signature /ds:SignedInfo /ds:Reference[@URI = '#id-xades-signed-props'] / ds:DigestMethod | Mandatory | [1..1] |
| Digest | xsd:base64Binary | This is the certificate digest value that would be created using HEX-to Base64 Encoder applied to the XAdES object that is described later containing the certificate and signing properties. [Reference field: **PropsDigest**] | `oRtv5YelD32v/jp/tC3MzfG0PimzfZ8lLQQkPlTBGj8=` | /ubl:Invoice /ext:UBLExtensions /ext:UBLExtension /ext:ExtensionContent /sig:UBLDocumentSignatures /sac:SignatureInformation /ds:Signature /ds:SignedInfo /ds:Reference[@URI = '#id-xades-signed-props'] / ds:DigestValue | Mandatory | [1..1] |

## Signed Properties

| ELEMENT | Field TYPE | DESCRIPTION | VALUE EXAMPLE | UBL Schema Mapping | Mandatory | Cardinality |
| --- | --- | --- | --- | --- | --- | --- |
| SigningTime | Date and time | The property specifies the UTC time at which the signer (purportedly) performed the signing process. Time | 2024-11-26T18:00:00Z | /ubl:Invoice /ext:UBLExtensions /ext:UBLExtension /ext:ExtensionContent /sig:UBLDocumentSignatures /sac:SignatureInformation /ds:Signature /ds:Object /xades:QualifyingProperties [@Target = 'signature'] /xades:SignedProperties [@Id = 'id-xades-signed-props'] /xades:SignedSignatureProperties /xades:SigningTime | Mandatory | [1..1] |
| SigningCertificate.Cert.CertDigest.DigestMethod | Empty | This is the method used to calculate the digest and it is set to SHA256. The value should be set as per the sample value provided here. | `<ds:DigestMethod Algorithm="http://www.w3.org/2001/04/xmlenc#sha256"/>` | /ubl:Invoice /ext:UBLExtensions /ext:UBLExtension /ext:ExtensionContent /sig:UBLDocumentSignatures /sac:SignatureInformation /ds:Signature /ds:Object /xades:QualifyingProperties [@Target = 'signature'] /xades:SignedProperties [@Id = 'id-xades-signed-props'] /xades:SignedSignatureProperties /xades:SigningCertificate /xades:Cert /xades:CertDigest /ds:DigestMethod | Mandatory | [1..1] |
| SigningCertificate.Cert.CertDigest.DigestValue | xsd:base64binary | This is the HEX-SHA256 encoded certificate information. [Reference field: **CertDigest**] | `ZDMwMmI0MTE1NzVjOTU2NTk4YzVlOD...k1YTA2OWQ0NjY2MjQ4NQ==` | /ubl:Invoice /ext:UBLExtensions /ext:UBLExtension /ext:ExtensionContent /sig:UBLDocumentSignatures /sac:SignatureInformation /ds:Signature /ds:Object /xades:QualifyingProperties [@Target = 'signature'] /xades:SignedProperties [@Id = 'id-xades-signed-props'] /xades:SignedSignatureProperties /xades:SigningCertificate /xades:Cert /xades:CertDigest /ds:DigestValue | Mandatory | [1..1] |
| SigningCertificate.Cert.IssuerSerial.X509IssuerName | String | This is the CN of the certificate used to generate the digital signature. This should be equal to the organization name. | Company Name | /ubl:Invoice /ext:UBLExtensions /ext:UBLExtension /ext:ExtensionContent /sig:UBLDocumentSignatures /sac:SignatureInformation /ds:Signature /ds:Object /xades:QualifyingProperties [@Target = 'signature'] /xades:SignedProperties [@Id = 'id-xades-signed-props'] /xades:SignedSignatureProperties /xades:SigningCertificate /xades:Cert /xades:IssuerSerial /ds:X509IssuerName | Mandatory | [1..1] |
| SigningCertificate.Cert.IssuerSerial.X509SerialNumber | xsd:normalizedString | This is the digital certificate serial number. This is the out of the standard certificate serial number. | 379112742831380471835263969587287663520637587 | /ubl:Invoice /ext:UBLExtensions /ext:UBLExtension /ext:ExtensionContent /sig:UBLDocumentSignatures /sac:SignatureInformation /ds:Signature /ds:Object /xades:QualifyingProperties [@Target = 'signature'] /xades:SignedProperties [@Id = 'id-xades-signed-props'] /xades:SignedSignatureProperties /xades:SigningCertificate /xades:Cert /xades:IssuerSerial /ds:X509SerialNumber | Mandatory | [1..1] |

## Signature Creation

Signed content is entire Document structure except for the following sections:

* UBLExtension (XPath: *[local-name()=×³Invoice×³]//*[local-name()=×³UBLExtensions×³]).
* Signature (XPath: *[local-name()=×³Invoice×³]//*[local-name()=×³Signature×³]). SHA256 hashing used for creating hash of the elements to sign.

The signature creation steps would mainly target to construct the enveloped XAdES signature structure, by computing the required values for the properties referenced above.

| PROPERTY | DESCRIPTION | Reference |
| --- | --- | --- |
| Signature value | This contains the signature value that was generated by signing the document digest within the signed information section | **Sig** |
| Document digest | This is the document digest value that would be created using HEX-to-Base64 Encoder applied to the entire transformed document content. This is the value that would be signed using the private key. | **DocDigest** |
| Signed properties digest | This is the certificate digest value that would be created using HEX-to-Base64 Encoder applied to the XAdES object that is described later containing the signing certificate information and signing properties. | **PropsDigest** |
| Certificate information digest | This is the HEX-SHA256 encoded certificate information that would be part of the signing properties. | **CertDigest** |

More details on how to define structure of the document to hash, create canonical version, apply hashing, sign the hash and then embed the signature are provided in section [Signature Creation](signature-creation-xml.md).

## Signature Validation

The validation rules for signatures are:

* Base64 encoded value that is a valid XAdES structure.
* Signing certificate is valid at the date of submission of document for validation.
* Signing certificate is issued to:
  + issuer of the document, i.e., taxpayer tax identification number and business registration number matches in certificate and document,
  + service provider (intermediary organization) who is active representative of taxpayer at the document issuance date and time and have the required permissions assigned to them to submit documents on behalf of the taxpayer. The intermediary tax identification number and business registration number matches in certificate **but** they will not match the document details.
* XAdES signature is valid RSA signature created using one of the approved certificate authorities in Malaysia (as described before).

## Signature Sample

[UBL 2.1 Invoice Sample XML with Signature](../10-sample-documents/samples/one-doc-signed.xml)

[UBL 2.1 Invoice Sample JSON with Signature](../10-sample-documents/samples/sample-ul-invoice-2.1-signed.min.json)

Disclaimer: The sample JSON file is for digital signature illustration purposes only. Taxpayers are advised to consult local laws and regulations for guidance on digital signature implementation.

---

[FAQ](../08-faq/README.md)
[Postman](../09-postman/README.md)


---

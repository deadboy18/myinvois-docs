<!-- source: https://sdk.myinvois.hasil.gov.my/signature-creation-json/ -->

# Signing a JSON document

## Introduction

This page provides sample content on how to generate a signed version of a JSON document. This is assuming that the certificate has already been generated and is available as per the guidance provided here [Digital Signing Certificate Profile](README.md#digital-signing-certificate-profile)

**Note: the code shown on this page is not production ready and hence not optimized for performance and is also missing errors and exceptions handling, please make sure you handle these points in your production ready code.**

The following represents the steps required to be performed to generate the final signed JSON document including the sample of the document. The steps assume that the input document is as per provided below.

```
{
  "_D": "urn:oasis:names:specification:ubl:schema:xsd:Invoice-2",
  "_A": "urn:oasis:names:specification:ubl:schema:xsd:CommonAggregateComponents-2",
  "_B": "urn:oasis:names:specification:ubl:schema:xsd:CommonBasicComponents-2",
  "Invoice": [
    {
      "ID": [
        {
          "_": "JSON-INV12345"
        }
      ],
      "IssueDate": [
        {
          "_": "2024-07-23"
        }
      ],
      "IssueTime": [
        {
          "_": "00:30:00Z"
        }
      ],
      "InvoiceTypeCode": [
        {
          "_": "01",
          "listVersionID": "1.0"
        }
      ],
      "DocumentCurrencyCode": [
        {
          "_": "MYR"
        }
      ],
      "TaxCurrencyCode": [
        {
          "_": "MYR"
        }
      ],
      "InvoicePeriod": [
        {
          "StartDate": [
            {
              "_": "2024-01-01"
            }
          ],
          "EndDate": [
            {
              "_": "2024-07-31"
            }
          ],
          "Description": [
            {
              "_": "Monthly"
            }
          ]
        }
      ],
      "BillingReference": [
        {
          "AdditionalDocumentReference": [
            {
              "ID": [
                {
                  "_": "E12345678912"
                }
              ]
            }
          ]
        }
      ],
      "AdditionalDocumentReference": [
        {
          "ID": [
            {
              "_": "E12345678912"
            }
          ],
          "DocumentType": [
            {
              "_": "CustomsImportForm"
            }
          ]
        },
        {
          "ID": [
            {
              "_": "sa313321312"
            }
          ],
          "DocumentType": [
            {
              "_": "213312dddddd"
            }
          ],
          "DocumentDescription": [
            {
              "_": "asddasdwqfd ddq"
            }
          ]
        },
        {
          "ID": [
            {
              "_": "E12345678912"
            }
          ],
          "DocumentType": [
            {
              "_": "K2"
            }
          ]
        },
        {
          "ID": [
            {
              "_": "CIF"
            }
          ]
        }
      ],
      "AccountingSupplierParty": [
        {
          "AdditionalAccountID": [
            {
              "_": "CPT-CCN-W-211111-KL-000002",
              "schemeAgencyName": "CertEX"
            }
          ],
          "Party": [
            {
              "IndustryClassificationCode": [
                {
                  "_": "46510",
                  "name": "Wholesale of computer hardware, software and peripherals"
                }
              ],
              "PartyIdentification": [
                {
                  "ID": [
                    {
                      "_": "Supplier's TIN",
                      "schemeID": "TIN"
                    }
                  ]
                },
                {
                  "ID": [
                    {
                      "_": "Supplier's BRN",
                      "schemeID": "BRN"
                    }
                  ]
                },
                {
                  "ID": [
                    {
                      "_": "NA",
                      "schemeID": "SST"
                    }
                  ]
                },
                {
                  "ID": [
                    {
                      "_": "NA",
                      "schemeID": "TTX"
                    }
                  ]
                }
              ],
              "PostalAddress": [
                {
                  "CityName": [
                    {
                      "_": "Kuala Lumpur"
                    }
                  ],
                  "PostalZone": [
                    {
                      "_": "50480"
                    }
                  ],
                  "CountrySubentityCode": [
                    {
                      "_": "10"
                    }
                  ],
                  "AddressLine": [
                    {
                      "Line": [
                        {
                          "_": "Lot 66"
                        }
                      ]
                    },
                    {
                      "Line": [
                        {
                          "_": "Bangunan Merdeka"
                        }
                      ]
                    },
                    {
                      "Line": [
                        {
                          "_": "Persiaran Jaya"
                        }
                      ]
                    }
                  ],
                  "Country": [
                    {
                      "IdentificationCode": [
                        {
                          "_": "MYS",
                          "listID": "ISO3166-1",
                          "listAgencyID": "6"
                        }
                      ]
                    }
                  ]
                }
              ],
              "PartyLegalEntity": [
                {
                  "RegistrationName": [
                    {
                      "_": "Supplier's Name"
                    }
                  ]
                }
              ],
              "Contact": [
                {
                  "Telephone": [
                    {
                      "_": "+60-123456789"
                    }
                  ],
                  "ElectronicMail": [
                    {
                      "_": "supplier@email.com"
                    }
                  ]
                }
              ]
            }
          ]
        }
      ],
      "AccountingCustomerParty": [
        {
          "Party": [
            {
              "PostalAddress": [
                {
                  "CityName": [
                    {
                      "_": "Kuala Lumpur"
                    }
                  ],
                  "PostalZone": [
                    {
                      "_": "50480"
                    }
                  ],
                  "CountrySubentityCode": [
                    {
                      "_": "10"
                    }
                  ],
                  "AddressLine": [
                    {
                      "Line": [
                        {
                          "_": "Lot 66"
                        }
                      ]
                    },
                    {
                      "Line": [
                        {
                          "_": "Bangunan Merdeka"
                        }
                      ]
                    },
                    {
                      "Line": [
                        {
                          "_": "Persiaran Jaya"
                        }
                      ]
                    }
                  ],
                  "Country": [
                    {
                      "IdentificationCode": [
                        {
                          "_": "MYS",
                          "listID": "ISO3166-1",
                          "listAgencyID": "6"
                        }
                      ]
                    }
                  ]
                }
              ],
              "PartyLegalEntity": [
                {
                  "RegistrationName": [
                    {
                      "_": "Buyer's Name"
                    }
                  ]
                }
              ],
              "PartyIdentification": [
                {
                  "ID": [
                    {
                      "_": "Buyer's TIN",
                      "schemeID": "TIN"
                    }
                  ]
                },
                {
                  "ID": [
                    {
                      "_": "Buyer's BRN",
                      "schemeID": "BRN"
                    }
                  ]
                },
                {
                  "ID": [
                    {
                      "_": "NA",
                      "schemeID": "SST"
                    }
                  ]
                },
                {
                  "ID": [
                    {
                      "_": "NA",
                      "schemeID": "TTX"
                    }
                  ]
                }
              ],
              "Contact": [
                {
                  "Telephone": [
                    {
                      "_": "+60-123456789"
                    }
                  ],
                  "ElectronicMail": [
                    {
                      "_": "buyer@email.com"
                    }
                  ]
                }
              ]
            }
          ]
        }
      ],
      "Delivery": [
        {
          "DeliveryParty": [
            {
              "PartyLegalEntity": [
                {
                  "RegistrationName": [
                    {
                      "_": "Recipient's Name"
                    }
                  ]
                }
              ],
              "PostalAddress": [
                {
                  "CityName": [
                    {
                      "_": "Kuala Lumpur"
                    }
                  ],
                  "PostalZone": [
                    {
                      "_": "50480"
                    }
                  ],
                  "CountrySubentityCode": [
                    {
                      "_": "10"
                    }
                  ],
                  "AddressLine": [
                    {
                      "Line": [
                        {
                          "_": "Lot 66"
                        }
                      ]
                    },
                    {
                      "Line": [
                        {
                          "_": "Bangunan Merdeka"
                        }
                      ]
                    },
                    {
                      "Line": [
                        {
                          "_": "Persiaran Jaya"
                        }
                      ]
                    }
                  ],
                  "Country": [
                    {
                      "IdentificationCode": [
                        {
                          "_": "MYS",
                          "listID": "ISO3166-1",
                          "listAgencyID": "6"
                        }
                      ]
                    }
                  ]
                }
              ],
              "PartyIdentification": [
                {
                  "ID": [
                    {
                      "_": "Recipient's TIN",
                      "schemeID": "TIN"
                    }
                  ]
                },
                {
                  "ID": [
                    {
                      "_": "Recipient's BRN",
                      "schemeID": "BRN"
                    }
                  ]
                }
              ]
            }
          ],
          "Shipment": [
            {
              "ID": [
                {
                  "_": "1234"
                }
              ],
              "FreightAllowanceCharge": [
                {
                  "ChargeIndicator": [
                    {
                      "_": true
                    }
                  ],
                  "AllowanceChargeReason": [
                    {
                      "_": "Service charge"
                    }
                  ],
                  "Amount": [
                    {
                      "_": 100,
                      "currencyID": "MYR"
                    }
                  ]
                }
              ]
            }
          ]
        }
      ],
      "PaymentMeans": [
        {
          "PaymentMeansCode": [
            {
              "_": "03"
            }
          ],
          "PayeeFinancialAccount": [
            {
              "ID": [
                {
                  "_": "1234567890123"
                }
              ]
            }
          ]
        }
      ],
      "PaymentTerms": [
        {
          "Note": [
            {
              "_": "Payment method is cash"
            }
          ]
        }
      ],
      "PrepaidPayment": [
        {
          "ID": [
            {
              "_": "E12345678912"
            }
          ],
          "PaidAmount": [
            {
              "_": 1,
              "currencyID": "MYR"
            }
          ],
          "PaidDate": [
            {
              "_": "2024-07-23"
            }
          ],
          "PaidTime": [
            {
              "_": "00:30:00Z"
            }
          ]
        }
      ],
      "AllowanceCharge": [
        {
          "ChargeIndicator": [
            {
              "_": false
            }
          ],
          "AllowanceChargeReason": [
            {
              "_": "Sample Description"
            }
          ],
          "Amount": [
            {
              "_": 100,
              "currencyID": "MYR"
            }
          ]
        },
        {
          "ChargeIndicator": [
            {
              "_": true
            }
          ],
          "AllowanceChargeReason": [
            {
              "_": "Service charge"
            }
          ],
          "Amount": [
            {
              "_": 100,
              "currencyID": "MYR"
            }
          ]
        }
      ],
      "TaxTotal": [
        {
          "TaxAmount": [
            {
              "_": 87.63,
              "currencyID": "MYR"
            }
          ],
          "TaxSubtotal": [
            {
              "TaxableAmount": [
                {
                  "_": 87.63,
                  "currencyID": "MYR"
                }
              ],
              "TaxAmount": [
                {
                  "_": 87.63,
                  "currencyID": "MYR"
                }
              ],
              "TaxCategory": [
                {
                  "ID": [
                    {
                      "_": "01"
                    }
                  ],
                  "TaxScheme": [
                    {
                      "ID": [
                        {
                          "_": "OTH",
                          "schemeID": "UN/ECE 5153",
                          "schemeAgencyID": "6"
                        }
                      ]
                    }
                  ]
                }
              ]
            }
          ]
        }
      ],
      "LegalMonetaryTotal": [
        {
          "LineExtensionAmount": [
            {
              "_": 1436.5,
              "currencyID": "MYR"
            }
          ],
          "TaxExclusiveAmount": [
            {
              "_": 1436.5,
              "currencyID": "MYR"
            }
          ],
          "TaxInclusiveAmount": [
            {
              "_": 1436.5,
              "currencyID": "MYR"
            }
          ],
          "AllowanceTotalAmount": [
            {
              "_": 1436.5,
              "currencyID": "MYR"
            }
          ],
          "ChargeTotalAmount": [
            {
              "_": 1436.5,
              "currencyID": "MYR"
            }
          ],
          "PayableRoundingAmount": [
            {
              "_": 0.3,
              "currencyID": "MYR"
            }
          ],
          "PayableAmount": [
            {
              "_": 1436.5,
              "currencyID": "MYR"
            }
          ]
        }
      ],
      "InvoiceLine": [
        {
          "ID": [
            {
              "_": "1234"
            }
          ],
          "InvoicedQuantity": [
            {
              "_": 1,
              "unitCode": "C62"
            }
          ],
          "LineExtensionAmount": [
            {
              "_": 1436.5,
              "currencyID": "MYR"
            }
          ],
          "AllowanceCharge": [
            {
              "ChargeIndicator": [
                {
                  "_": false
                }
              ],
              "AllowanceChargeReason": [
                {
                  "_": "Sample Description"
                }
              ],
              "MultiplierFactorNumeric": [
                {
                  "_": 0.15
                }
              ],
              "Amount": [
                {
                  "_": 100,
                  "currencyID": "MYR"
                }
              ]
            },
            {
              "ChargeIndicator": [
                {
                  "_": true
                }
              ],
              "AllowanceChargeReason": [
                {
                  "_": "Sample Description"
                }
              ],
              "MultiplierFactorNumeric": [
                {
                  "_": 0.1
                }
              ],
              "Amount": [
                {
                  "_": 100,
                  "currencyID": "MYR"
                }
              ]
            }
          ],
          "TaxTotal": [
            {
              "TaxAmount": [
                {
                  "_": 1460.5,
                  "currencyID": "MYR"
                }
              ],
              "TaxSubtotal": [
                {
                  "TaxableAmount": [
                    {
                      "_": 1460.5,
                      "currencyID": "MYR"
                    }
                  ],
                  "TaxAmount": [
                    {
                      "_": 1460.5,
                      "currencyID": "MYR"
                    }
                  ],
                  "Percent": [
                    {
                      "_": 6
                    }
                  ],
                  "TaxCategory": [
                    {
                      "ID": [
                        {
                          "_": "E"
                        }
                      ],
                      "TaxExemptionReason": [
                        {
                          "_": "Exempt New Means of Transport"
                        }
                      ],
                      "TaxScheme": [
                        {
                          "ID": [
                            {
                              "_": "OTH",
                              "schemeID": "UN/ECE 5153",
                              "schemeAgencyID": "6"
                            }
                          ]
                        }
                      ]
                    }
                  ]
                }
              ]
            }
          ],
          "Item": [
            {
              "CommodityClassification": [
                {
                  "ItemClassificationCode": [
                    {
                      "_": "9800.00.0010",
                      "listID": "PTC"
                    }
                  ]
                },
                {
                  "ItemClassificationCode": [
                    {
                      "_": "003",
                      "listID": "CLASS"
                    }
                  ]
                }
              ],
              "Description": [
                {
                  "_": "Laptop Peripherals"
                }
              ],
              "OriginCountry": [
                {
                  "IdentificationCode": [
                    {
                      "_": "MYS"
                    }
                  ]
                }
              ]
            }
          ],
          "Price": [
            {
              "PriceAmount": [
                {
                  "_": 17,
                  "currencyID": "MYR"
                }
              ]
            }
          ],
          "ItemPriceExtension": [
            {
              "Amount": [
                {
                  "_": 100,
                  "currencyID": "MYR"
                }
              ]
            }
          ]
        }
      ]
    }
  ]
}
```

## Step 1: Transform the document

The document will be required to be transformed by performing the following:  
â¢ Removing the sections âUBLExtensionsâ, and âSignatureâ if they do exist.  
â¢ Minify the file by removing new lines and not needed spaces.  
The output of this step would be a transformed document. The input document shown above once transformed it should be as per the below JSON file.

```
{"_D":"urn:oasis:names:specification:ubl:schema:xsd:Invoice-2","_A":"urn:oasis:names:specification:ubl:schema:xsd:CommonAggregateComponents-2","_B":"urn:oasis:names:specification:ubl:schema:xsd:CommonBasicComponents-2","Invoice":[{"ID":[{"_":"JSON-INV12345"}],"IssueDate":[{"_":"2024-07-23"}],"IssueTime":[{"_":"00:30:00Z"}],"InvoiceTypeCode":[{"_":"01","listVersionID":"1.0"}],"DocumentCurrencyCode":[{"_":"MYR"}],"TaxCurrencyCode":[{"_":"MYR"}],"InvoicePeriod":[{"StartDate":[{"_":"2024-01-01"}],"EndDate":[{"_":"2024-07-31"}],"Description":[{"_":"Monthly"}]}],"BillingReference":[{"AdditionalDocumentReference":[{"ID":[{"_":"E12345678912"}]}]}],"AdditionalDocumentReference":[{"ID":[{"_":"E12345678912"}],"DocumentType":[{"_":"CustomsImportForm"}]},{"ID":[{"_":"sa313321312"}],"DocumentType":[{"_":"213312dddddd"}],"DocumentDescription":[{"_":"asddasdwqfd ddq"}]},{"ID":[{"_":"E12345678912"}],"DocumentType":[{"_":"K2"}]},{"ID":[{"_":"CIF"}]}],"AccountingSupplierParty":[{"AdditionalAccountID":[{"_":"CPT-CCN-W-211111-KL-000002","schemeAgencyName":"CertEX"}],"Party":[{"IndustryClassificationCode":[{"_":"46510","name":"Wholesale of computer hardware, software and peripherals"}],"PartyIdentification":[{"ID":[{"_":"Supplier's TIN","schemeID":"TIN"}]},{"ID":[{"_":"Supplier's BRN","schemeID":"BRN"}]},{"ID":[{"_":"NA","schemeID":"SST"}]},{"ID":[{"_":"NA","schemeID":"TTX"}]}],"PostalAddress":[{"CityName":[{"_":"Kuala Lumpur"}],"PostalZone":[{"_":"50480"}],"CountrySubentityCode":[{"_":"10"}],"AddressLine":[{"Line":[{"_":"Lot 66"}]},{"Line":[{"_":"Bangunan Merdeka"}]},{"Line":[{"_":"Persiaran Jaya"}]}],"Country":[{"IdentificationCode":[{"_":"MYS","listID":"ISO3166-1","listAgencyID":"6"}]}]}],"PartyLegalEntity":[{"RegistrationName":[{"_":"Supplier's Name"}]}],"Contact":[{"Telephone":[{"_":"+60-123456789"}],"ElectronicMail":[{"_":"supplier@email.com"}]}]}]}],"AccountingCustomerParty":[{"Party":[{"PostalAddress":[{"CityName":[{"_":"Kuala Lumpur"}],"PostalZone":[{"_":"50480"}],"CountrySubentityCode":[{"_":"10"}],"AddressLine":[{"Line":[{"_":"Lot 66"}]},{"Line":[{"_":"Bangunan Merdeka"}]},{"Line":[{"_":"Persiaran Jaya"}]}],"Country":[{"IdentificationCode":[{"_":"MYS","listID":"ISO3166-1","listAgencyID":"6"}]}]}],"PartyLegalEntity":[{"RegistrationName":[{"_":"Buyer's Name"}]}],"PartyIdentification":[{"ID":[{"_":"Buyer's TIN","schemeID":"TIN"}]},{"ID":[{"_":"Buyer's BRN","schemeID":"BRN"}]},{"ID":[{"_":"NA","schemeID":"SST"}]},{"ID":[{"_":"NA","schemeID":"TTX"}]}],"Contact":[{"Telephone":[{"_":"+60-123456789"}],"ElectronicMail":[{"_":"buyer@email.com"}]}]}]}],"Delivery":[{"DeliveryParty":[{"PartyLegalEntity":[{"RegistrationName":[{"_":"Recipient's Name"}]}],"PostalAddress":[{"CityName":[{"_":"Kuala Lumpur"}],"PostalZone":[{"_":"50480"}],"CountrySubentityCode":[{"_":"10"}],"AddressLine":[{"Line":[{"_":"Lot 66"}]},{"Line":[{"_":"Bangunan Merdeka"}]},{"Line":[{"_":"Persiaran Jaya"}]}],"Country":[{"IdentificationCode":[{"_":"MYS","listID":"ISO3166-1","listAgencyID":"6"}]}]}],"PartyIdentification":[{"ID":[{"_":"Recipient's TIN","schemeID":"TIN"}]},{"ID":[{"_":"Recipient's BRN","schemeID":"BRN"}]}]}],"Shipment":[{"ID":[{"_":"1234"}],"FreightAllowanceCharge":[{"ChargeIndicator":[{"_":true}],"AllowanceChargeReason":[{"_":"Service charge"}],"Amount":[{"_":100,"currencyID":"MYR"}]}]}]}],"PaymentMeans":[{"PaymentMeansCode":[{"_":"03"}],"PayeeFinancialAccount":[{"ID":[{"_":"1234567890123"}]}]}],"PaymentTerms":[{"Note":[{"_":"Payment method is cash"}]}],"PrepaidPayment":[{"ID":[{"_":"E12345678912"}],"PaidAmount":[{"_":1,"currencyID":"MYR"}],"PaidDate":[{"_":"2024-07-23"}],"PaidTime":[{"_":"00:30:00Z"}]}],"AllowanceCharge":[{"ChargeIndicator":[{"_":false}],"AllowanceChargeReason":[{"_":"Sample Description"}],"Amount":[{"_":100,"currencyID":"MYR"}]},{"ChargeIndicator":[{"_":true}],"AllowanceChargeReason":[{"_":"Service charge"}],"Amount":[{"_":100,"currencyID":"MYR"}]}],"TaxTotal":[{"TaxAmount":[{"_":87.63,"currencyID":"MYR"}],"TaxSubtotal":[{"TaxableAmount":[{"_":87.63,"currencyID":"MYR"}],"TaxAmount":[{"_":87.63,"currencyID":"MYR"}],"TaxCategory":[{"ID":[{"_":"01"}],"TaxScheme":[{"ID":[{"_":"OTH","schemeID":"UN/ECE 5153","schemeAgencyID":"6"}]}]}]}]}],"LegalMonetaryTotal":[{"LineExtensionAmount":[{"_":1436.5,"currencyID":"MYR"}],"TaxExclusiveAmount":[{"_":1436.5,"currencyID":"MYR"}],"TaxInclusiveAmount":[{"_":1436.5,"currencyID":"MYR"}],"AllowanceTotalAmount":[{"_":1436.5,"currencyID":"MYR"}],"ChargeTotalAmount":[{"_":1436.5,"currencyID":"MYR"}],"PayableRoundingAmount":[{"_":0.3,"currencyID":"MYR"}],"PayableAmount":[{"_":1436.5,"currencyID":"MYR"}]}],"InvoiceLine":[{"ID":[{"_":"1234"}],"InvoicedQuantity":[{"_":1,"unitCode":"C62"}],"LineExtensionAmount":[{"_":1436.5,"currencyID":"MYR"}],"AllowanceCharge":[{"ChargeIndicator":[{"_":false}],"AllowanceChargeReason":[{"_":"Sample Description"}],"MultiplierFactorNumeric":[{"_":0.15}],"Amount":[{"_":100,"currencyID":"MYR"}]},{"ChargeIndicator":[{"_":true}],"AllowanceChargeReason":[{"_":"Sample Description"}],"MultiplierFactorNumeric":[{"_":0.1}],"Amount":[{"_":100,"currencyID":"MYR"}]}],"TaxTotal":[{"TaxAmount":[{"_":1460.5,"currencyID":"MYR"}],"TaxSubtotal":[{"TaxableAmount":[{"_":1460.5,"currencyID":"MYR"}],"TaxAmount":[{"_":1460.5,"currencyID":"MYR"}],"Percent":[{"_":6}],"TaxCategory":[{"ID":[{"_":"E"}],"TaxExemptionReason":[{"_":"Exempt New Means of Transport"}],"TaxScheme":[{"ID":[{"_":"OTH","schemeID":"UN/ECE 5153","schemeAgencyID":"6"}]}]}]}]}],"Item":[{"CommodityClassification":[{"ItemClassificationCode":[{"_":"9800.00.0010","listID":"PTC"}]},{"ItemClassificationCode":[{"_":"003","listID":"CLASS"}]}],"Description":[{"_":"Laptop Peripherals"}],"OriginCountry":[{"IdentificationCode":[{"_":"MYS"}]}]}],"Price":[{"PriceAmount":[{"_":17,"currencyID":"MYR"}]}],"ItemPriceExtension":[{"Amount":[{"_":100,"currencyID":"MYR"}]}]}]}]}
```

## Step 2: Calculate the document digest

The next step is to calculate the document digest to be signed using the certificate. The document digest is calculated using the following steps.

```
// assume document is loaded in the variable document
using (SHA256 sha256 = SHA256.Create())
{
  var hash = sha256.ComputeHash(System.Text.Encoding.UTF8.GetBytes(document));
  var docdigest = Convert.ToBase64String(hash);
}
```

## Step 3: Sign the document digest using the certificate

The next step is to sign the property **docdigest** that is calculated in the previous step. This should be performed by loading the certificate to be used to sign the document as per the certificate generation guidance. Then use this certificate to sign the document digest value.

The following is example pseudo code that shows how this can be performed.

```
X509Certificate2 cert = new X509Certificate2();
// Load the certificate from the file store or the key store as required
RSACryptoServiceProvider privateKeyProvider = (RSACryptoServiceProvider)cert.PrivateKey;
var sign = privateKeyProvider.SignHash(hash, HashAlgorithmName.SHA256, RSASignaturePadding.Pkcs1);
var signature = Convert.ToBase64String(sign);
```

## Step 4: Calculate the certificate digest

The next step is to calculate the certificate digest value. The following is example pseudo code that shows how this can be performed.

```
X509Certificate cert = new X509Certificate();
// Load the certificate from the file store or the key store as required
var certash = cert.GetCertHash(System.Security.Cryptography.HashAlgorithmName.SHA256);
var certdigest = Convert.ToBase64String(certash);
```

## Step 5: Populate the signed properties section

The next step is to populate the signed properties section using the values that are already calculated previously. A sample of the output for the signed properties section for the input sample document as per below.

```
"SignedProperties": [
  {
    "Id": "id-xades-signed-props",
    "SignedSignatureProperties": [
      {
        "SigningTime": [
          {
            "_": "2024-07-23T15:14:54Z"
          }
        ],
        "SigningCertificate": [
          {
            "Cert": [
              {
                "CertDigest": [
                  {
                    "DigestMethod": [
                      {
                        "_": "",
                        "Algorithm": "http://www.w3.org/2001/04/xmlenc#sha256"
                      }
                    ],
                    "DigestValue": [
                      {
                        "_": "KKBSTyiPKGkGl1AFqcPziKCEIDYGtnYUTQN4ukO7G40="
                      }
                    ]
                  }
                ],
                "IssuerSerial": [
                  {
                    "X509IssuerName": [
                      {
                        "_": "CN=Trial LHDNM Sub CA V1, OU=Terms of use at http://www.posdigicert.com.my, O=LHDNM, C=MY"
                      }
                    ],
                    "X509SerialNumber": [
                      {
                        "_": "162880276254639189035871514749820882117"
                      }
                    ]
                  }
                ]
              }
            ]
          }
        ]
      }
    ]
  }
]
```

## Step 6: Calculate the signed properties section digest

The signed properties section that is prepared in the previous is then used to calculate the digest. This has to be performed by taking the outer content of the signed properties section and minify the section. The following is an example of the signed properties section that would be then used to calculate the signed properties section digest from the sample already provided above.

```
{"Target":"signature","SignedProperties":[{"Id":"id-xades-signed-props","SignedSignatureProperties":[{"SigningTime":[{"_":"2024-07-23T15:14:54Z"}],"SigningCertificate":[{"Cert":[{"CertDigest":[{"DigestMethod":[{"_":"","Algorithm":"http://www.w3.org/2001/04/xmlenc#sha256"}],"DigestValue":[{"_":"KKBSTyiPKGkGl1AFqcPziKCEIDYGtnYUTQN4ukO7G40="}]}],"IssuerSerial":[{"X509IssuerName":[{"_":"CN=Trial LHDNM Sub CA V1, OU=Terms of use at http://www.posdigicert.com.my, O=LHDNM, C=MY"}],"X509SerialNumber":[{"_":"162880276254639189035871514749820882117"}]}]}]}]}]}]}
```

Then this content is used to calculate the digest of this content which would be the signed properties section digest value.

The following is example pseudo code that shows how this can be performed.

```
// assume signedprops is loaded in the variable signedprops
using (SHA256 sha256 = SHA256.Create())
{
  var signedpropshash = sha256.ComputeHash(System.Text.Encoding.UTF8.GetBytes(signedprops));
  var signedpropsdigest = Convert.ToBase64String(signedpropshash);
}
```

## Step 7: Create the signed JSON document

Using the properties and digests calculated in the previous steps the sections âUBLExtensionsâ, and âSignatureâ. The following is an example of the âUBLExtensionsâ section content given the sample that is provided above.

```
"UBLExtensions": [
  {
    "UBLExtension": [
      {
        "ExtensionURI": [
          {
            "_": "urn:oasis:names:specification:ubl:dsig:enveloped:xades"
          }
        ],
        "ExtensionContent": [
          {
            "UBLDocumentSignatures": [
              {
                "SignatureInformation": [
                  {
                    "ID": [
                      {
                        "_": "urn:oasis:names:specification:ubl:signature:1"
                      }
                    ],
                    "ReferencedSignatureID": [
                      {
                        "_": "urn:oasis:names:specification:ubl:signature:Invoice"
                      }
                    ],
                    "Signature": [
                      {
                        "Id": "signature",
                        "Object": [
                          {
                            "QualifyingProperties": [
                              {
                                "Target": "signature",
                                "SignedProperties": [
                                  {
                                    "Id": "id-xades-signed-props",
                                    "SignedSignatureProperties": [
                                      {
                                        "SigningTime": [
                                          {
                                            "_": "2024-07-23T15:14:54Z"
                                          }
                                        ],
                                        "SigningCertificate": [
                                          {
                                            "Cert": [
                                              {
                                                "CertDigest": [
                                                  {
                                                    "DigestMethod": [
                                                      {
                                                        "_": "",
                                                        "Algorithm": "http://www.w3.org/2001/04/xmlenc#sha256"
                                                      }
                                                    ],
                                                    "DigestValue": [
                                                      {
                                                        "_": "KKBSTyiPKGkGl1AFqcPziKCEIDYGtnYUTQN4ukO7G40="
                                                      }
                                                    ]
                                                  }
                                                ],
                                                "IssuerSerial": [
                                                  {
                                                    "X509IssuerName": [
                                                      {
                                                        "_": "CN=Trial LHDNM Sub CA V1, OU=Terms of use at http://www.posdigicert.com.my, O=LHDNM, C=MY"
                                                      }
                                                    ],
                                                    "X509SerialNumber": [
                                                      {
                                                        "_": "162880276254639189035871514749820882117"
                                                      }
                                                    ]
                                                  }
                                                ]
                                              }
                                            ]
                                          }
                                        ]
                                      }
                                    ]
                                  }
                                ]
                              }
                            ]
                          }
                        ],
                        "KeyInfo": [
                          {
                            "X509Data": [
                              {
                                "X509Certificate": [
                                  {
                                    "_": "MIIFlDCCA3ygAwIBAgIQeomZorO+0AwmW2BRdWJMxTANBgkqhkiG9w0BAQsFADB1MQswCQYDVQQGEwJNWTEOMAwGA1UEChMFTEhETk0xNjA0BgNVBAsTLVRlcm1zIG9mIHVzZSBhdCBodHRwOi8vd3d3LnBvc2RpZ2ljZXJ0LmNvbS5teTEeMBwGA1UEAxMVVHJpYWwgTEhETk0gU3ViIENBIFYxMB4XDTI0MDYwNjAyNTIzNloXDTI0MDkwNjAyNTIzNlowgZwxCzAJBgNVBAYTAk1ZMQ4wDAYDVQQKEwVEdW1teTEVMBMGA1UEYRMMQzI5NzAyNjM1MDYwMRswGQYDVQQLExJUZXN0IFVuaXQgZUludm9pY2UxDjAMBgNVBAMTBUR1bW15MRIwEAYDVQQFEwlEMTIzNDU2NzgxJTAjBgkqhkiG9w0BCQEWFmFuYXMuYUBmZ3Zob2xkaW5ncy5jb20wggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQChvfOzAofnU60xFO7NcmF2WRi+dgor1D7ccISgRVfZC30Fdxnt1S6ZNf78Lbrz8TbWMicS8plh/pHy96OJvEBplsAgcZTd6WvaMUB2oInC86D3YShlthR6EzhwXgBmg/g9xprwlRqXMT2p4+K8zmyJZ9pIb8Y+tQNjm/uYNudtwGVm8A4hEhlRHbgfUXRzT19QZml6V2Ea0wQI8VyWWa8phCIkBD2w4F8jG4eP5/0XSQkTfBHHf+GV/YDJx5KiiYfmB1nGfwoPHix6Gey+wRjIq87on8Dm5+8ei8/bOhcuuSlpxgwphAP3rZrNbRN9LNVLSQ5md41asoBHfaDIVPVpAgMBAAGjgfcwgfQwHwYDVR0lBBgwFgYIKwYBBQUHAwQGCisGAQQBgjcKAwwwEQYDVR0OBAoECEDwms66hrpiMFMGA1UdIARMMEowSAYJKwYBBAGDikUBMDswOQYIKwYBBQUHAgEWLWh0dHBzOi8vd3d3LnBvc2RpZ2ljZXJ0LmNvbS5teS9yZXBvc2l0b3J5L2NwczATBgNVHSMEDDAKgAhNf9lrtsUI0DAOBgNVHQ8BAf8EBAMCBkAwRAYDVR0fBD0wOzA5oDegNYYzaHR0cDovL3RyaWFsY3JsLnBvc2RpZ2ljZXJ0LmNvbS5teS9UcmlhbExIRE5NVjEuY3JsMA0GCSqGSIb3DQEBCwUAA4ICAQBwptnIb1OA8NNVotgVIjOnpQtowew87Y0EBWAnVhOsMDlWXD/s+BL7vIEbX/BYa0TjakQ7qo4riSHyUkQ+X+pNsPEqolC4uFOp0pDsIdjsNB+WG15itnghkI99c6YZmbXcSFw9E160c7vG25gIL6zBPculHx5+laE59YkmDLdxx27e0TltUbFmuq3diYBOOf7NswFcDXCo+kXOwFfgmpbzYS0qfSoh3eZZtVHg0r6uga1UsMGb90+IRsk4st99EOVENvo0290lWhPBVK2G34+2TzbbYnVkoxnq6uDMw3cRpXX/oSfya+tyF51kT3iXvpmQ9OMF3wMlfKwCS7BZB2+iRja/1WHkAP7QW7/+0zRBcGQzY7AYsdZUllwYapsLEtbZBrTiH12X4XnZjny9rLfQLzJsFGT7Q+e02GiCsBrK7ZHNTindLRnJYAo4U2at5+SjqBiXSmz0DG+juOyFkwiWyD0xeheg4tMMO2pZ7clQzKflYnvFTEFnt+d+tvVwNjTboxfVxEv2qWF6qcMJeMvXwKTXuwVI2iUqmJSzJbUY+w3OeG7fvrhUfMJPM9XZBOp7CEI1QHfHrtyjlKNhYzG3IgHcfAZUURO16gFmWgzAZLkJSmCIxaIty/EmvG5N3ZePolBOa7lNEH/eSBMGAQteH+Twtiu0Y2xSwmmsxnfJyw=="
                                  }
                                ],
                                "X509SubjectName": [
                                  {
                                    "_": "CN=Trial LHDNM Sub CA V1, OU=Terms of use at http://www.posdigicert.com.my, O=LHDNM, C=MY"
                                  }
                                ],
                                "X509IssuerSerial": [
                                  {
                                    "X509IssuerName": [
                                      {
                                        "_": "CN=Trial LHDNM Sub CA V1, OU=Terms of use at http://www.posdigicert.com.my, O=LHDNM, C=MY"
                                      }
                                    ],
                                    "X509SerialNumber": [
                                      {
                                        "_": "162880276254639189035871514749820882117"
                                      }
                                    ]
                                  }
                                ]
                              }
                            ]
                          }
                        ],
                        "SignatureValue": [
                          {
                            "_": "QTvntg4opuS7ZYWmly/iAO2OnLVJcKylYuF+QJKZdx9BkFVglmVuFtEtwoqgNsbsKaaEDinTSUAVStRJs2tiU1Jdryd4hoZ/Hc5TAvFnThpauVOLsc3j07cUB1+zhNjENmFeI9yzTGjr8XfNi4mNPspnhFAT4QGbRpxkWiIsKj762p3dhCwUNAuNLjunVaosYQ5lvSzGt4B9TF/1xJ7Z6kdcJTmBeltTWErSRA2EOMzWsGWGZVvyPLnXfnlIBQItTvARXveafxFdS1iw91g7mSEEYeqEviI0b4FUmkwH8ed0boFc6EHl1VF+2uVxBtHeKf31FqTQl/6/pF4Qgpn6Hg=="
                          }
                        ],
                        "SignedInfo": [
                          {
                            "SignatureMethod": [
                              {
                                "_": "",
                                "Algorithm": "http://www.w3.org/2001/04/xmldsig-more#rsa-sha256"
                              }
                            ],
                            "Reference": [
                              {
                                "Type": "http://uri.etsi.org/01903/v1.3.2#SignedProperties",
                                "URI": "#id-xades-signed-props",
                                "DigestMethod": [
                                  {
                                    "_": "",
                                    "Algorithm": "http://www.w3.org/2001/04/xmlenc#sha256"
                                  }
                                ],
                                "DigestValue": [
                                  {
                                    "_": "Rzuzz+70GSnGBF1YxhHnjSzFpQ1MW4vyX/Q9bTHkE2c="
                                  }
                                ]
                              },
                              {
                                "Type": "",
                                "URI": "",
                                "DigestMethod": [
                                  {
                                    "_": "",
                                    "Algorithm": "http://www.w3.org/2001/04/xmlenc#sha256"
                                  }
                                ],
                                "DigestValue": [
                                  {
                                    "_": "vMs/IdnS7isftqrBDr4F1LK/CkvBkW5Gb3Wn6OVzAxo="
                                  }
                                ]
                              }
                            ]
                          }
                        ]
                      }
                    ]
                  }
                ]
              }
            ]
          }
        ]
      }
    ]
  }
]
```

The following is an example of the âSignatureâ section given the sample that is provided above.

```
"Signature": [
  {
    "ID": [
      {
        "_": "urn:oasis:names:specification:ubl:signature:Invoice"
      }
    ],
    "SignatureMethod": [
      {
        "_": "urn:oasis:names:specification:ubl:dsig:enveloped:xades"
      }
    ]
  }
]
```

The final signed document should be already minified including all the content to make sure the document is still valid according to the signature and the signature doesnât break. The following is a sample of the output signed document including all the sections.

```
{"_D":"urn:oasis:names:specification:ubl:schema:xsd:Invoice-2","_A":"urn:oasis:names:specification:ubl:schema:xsd:CommonAggregateComponents-2","_B":"urn:oasis:names:specification:ubl:schema:xsd:CommonBasicComponents-2","Invoice":[{"ID":[{"_":"JSON-INV12345"}],"IssueDate":[{"_":"2024-07-23"}],"IssueTime":[{"_":"00:30:00Z"}],"InvoiceTypeCode":[{"_":"01","listVersionID":"1.1"}],"DocumentCurrencyCode":[{"_":"MYR"}],"TaxCurrencyCode":[{"_":"MYR"}],"InvoicePeriod":[{"StartDate":[{"_":"2024-01-01"}],"EndDate":[{"_":"2024-07-31"}],"Description":[{"_":"Monthly"}]}],"BillingReference":[{"AdditionalDocumentReference":[{"ID":[{"_":"E12345678912"}]}]}],"AdditionalDocumentReference":[{"ID":[{"_":"E12345678912"}],"DocumentType":[{"_":"CustomsImportForm"}]},{"ID":[{"_":"sa313321312"}],"DocumentType":[{"_":"213312dddddd"}],"DocumentDescription":[{"_":"asddasdwqfd ddq"}]},{"ID":[{"_":"E12345678912"}],"DocumentType":[{"_":"K2"}]},{"ID":[{"_":"CIF"}]}],"AccountingSupplierParty":[{"AdditionalAccountID":[{"_":"CPT-CCN-W-211111-KL-000002","schemeAgencyName":"CertEX"}],"Party":[{"IndustryClassificationCode":[{"_":"46510","name":"Wholesale of computer hardware, software and peripherals"}],"PartyIdentification":[{"ID":[{"_":"Supplier's TIN","schemeID":"TIN"}]},{"ID":[{"_":"Supplier's BRN","schemeID":"BRN"}]},{"ID":[{"_":"NA","schemeID":"SST"}]},{"ID":[{"_":"NA","schemeID":"TTX"}]}],"PostalAddress":[{"CityName":[{"_":"Kuala Lumpur"}],"PostalZone":[{"_":"50480"}],"CountrySubentityCode":[{"_":"10"}],"AddressLine":[{"Line":[{"_":"Lot 66"}]},{"Line":[{"_":"Bangunan Merdeka"}]},{"Line":[{"_":"Persiaran Jaya"}]}],"Country":[{"IdentificationCode":[{"_":"MYS","listID":"ISO3166-1","listAgencyID":"6"}]}]}],"PartyLegalEntity":[{"RegistrationName":[{"_":"Supplier's Name"}]}],"Contact":[{"Telephone":[{"_":"+60-123456789"}],"ElectronicMail":[{"_":"supplier@email.com"}]}]}]}],"AccountingCustomerParty":[{"Party":[{"PostalAddress":[{"CityName":[{"_":"Kuala Lumpur"}],"PostalZone":[{"_":"50480"}],"CountrySubentityCode":[{"_":"10"}],"AddressLine":[{"Line":[{"_":"Lot 66"}]},{"Line":[{"_":"Bangunan Merdeka"}]},{"Line":[{"_":"Persiaran Jaya"}]}],"Country":[{"IdentificationCode":[{"_":"MYS","listID":"ISO3166-1","listAgencyID":"6"}]}]}],"PartyLegalEntity":[{"RegistrationName":[{"_":"Buyer's Name"}]}],"PartyIdentification":[{"ID":[{"_":"Buyer's TIN","schemeID":"TIN"}]},{"ID":[{"_":"Buyer's BRN","schemeID":"BRN"}]},{"ID":[{"_":"NA","schemeID":"SST"}]},{"ID":[{"_":"NA","schemeID":"TTX"}]}],"Contact":[{"Telephone":[{"_":"+60-123456789"}],"ElectronicMail":[{"_":"buyer@email.com"}]}]}]}],"Delivery":[{"DeliveryParty":[{"PartyLegalEntity":[{"RegistrationName":[{"_":"Recipient's Name"}]}],"PostalAddress":[{"CityName":[{"_":"Kuala Lumpur"}],"PostalZone":[{"_":"50480"}],"CountrySubentityCode":[{"_":"10"}],"AddressLine":[{"Line":[{"_":"Lot 66"}]},{"Line":[{"_":"Bangunan Merdeka"}]},{"Line":[{"_":"Persiaran Jaya"}]}],"Country":[{"IdentificationCode":[{"_":"MYS","listID":"ISO3166-1","listAgencyID":"6"}]}]}],"PartyIdentification":[{"ID":[{"_":"Recipient's TIN","schemeID":"TIN"}]},{"ID":[{"_":"Recipient's BRN","schemeID":"BRN"}]}]}],"Shipment":[{"ID":[{"_":"1234"}],"FreightAllowanceCharge":[{"ChargeIndicator":[{"_":true}],"AllowanceChargeReason":[{"_":"Service charge"}],"Amount":[{"_":100,"currencyID":"MYR"}]}]}]}],"PaymentMeans":[{"PaymentMeansCode":[{"_":"03"}],"PayeeFinancialAccount":[{"ID":[{"_":"1234567890123"}]}]}],"PaymentTerms":[{"Note":[{"_":"Payment method is cash"}]}],"PrepaidPayment":[{"ID":[{"_":"E12345678912"}],"PaidAmount":[{"_":1,"currencyID":"MYR"}],"PaidDate":[{"_":"2024-07-23"}],"PaidTime":[{"_":"00:30:00Z"}]}],"AllowanceCharge":[{"ChargeIndicator":[{"_":false}],"AllowanceChargeReason":[{"_":"Sample Description"}],"Amount":[{"_":100,"currencyID":"MYR"}]},{"ChargeIndicator":[{"_":true}],"AllowanceChargeReason":[{"_":"Service charge"}],"Amount":[{"_":100,"currencyID":"MYR"}]}],"TaxTotal":[{"TaxAmount":[{"_":87.63,"currencyID":"MYR"}],"TaxSubtotal":[{"TaxableAmount":[{"_":87.63,"currencyID":"MYR"}],"TaxAmount":[{"_":87.63,"currencyID":"MYR"}],"TaxCategory":[{"ID":[{"_":"01"}],"TaxScheme":[{"ID":[{"_":"OTH","schemeID":"UN/ECE 5153","schemeAgencyID":"6"}]}]}]}]}],"LegalMonetaryTotal":[{"LineExtensionAmount":[{"_":1436.5,"currencyID":"MYR"}],"TaxExclusiveAmount":[{"_":1436.5,"currencyID":"MYR"}],"TaxInclusiveAmount":[{"_":1436.5,"currencyID":"MYR"}],"AllowanceTotalAmount":[{"_":1436.5,"currencyID":"MYR"}],"ChargeTotalAmount":[{"_":1436.5,"currencyID":"MYR"}],"PayableRoundingAmount":[{"_":0.3,"currencyID":"MYR"}],"PayableAmount":[{"_":1436.5,"currencyID":"MYR"}]}],"InvoiceLine":[{"ID":[{"_":"1234"}],"InvoicedQuantity":[{"_":1,"unitCode":"C62"}],"LineExtensionAmount":[{"_":1436.5,"currencyID":"MYR"}],"AllowanceCharge":[{"ChargeIndicator":[{"_":false}],"AllowanceChargeReason":[{"_":"Sample Description"}],"MultiplierFactorNumeric":[{"_":0.15}],"Amount":[{"_":100,"currencyID":"MYR"}]},{"ChargeIndicator":[{"_":true}],"AllowanceChargeReason":[{"_":"Sample Description"}],"MultiplierFactorNumeric":[{"_":0.1}],"Amount":[{"_":100,"currencyID":"MYR"}]}],"TaxTotal":[{"TaxAmount":[{"_":1460.5,"currencyID":"MYR"}],"TaxSubtotal":[{"TaxableAmount":[{"_":1460.5,"currencyID":"MYR"}],"TaxAmount":[{"_":1460.5,"currencyID":"MYR"}],"Percent":[{"_":6}],"TaxCategory":[{"ID":[{"_":"E"}],"TaxExemptionReason":[{"_":"Exempt New Means of Transport"}],"TaxScheme":[{"ID":[{"_":"OTH","schemeID":"UN/ECE 5153","schemeAgencyID":"6"}]}]}]}]}],"Item":[{"CommodityClassification":[{"ItemClassificationCode":[{"_":"9800.00.0010","listID":"PTC"}]},{"ItemClassificationCode":[{"_":"003","listID":"CLASS"}]}],"Description":[{"_":"Laptop Peripherals"}],"OriginCountry":[{"IdentificationCode":[{"_":"MYS"}]}]}],"Price":[{"PriceAmount":[{"_":17,"currencyID":"MYR"}]}],"ItemPriceExtension":[{"Amount":[{"_":100,"currencyID":"MYR"}]}]}],"UBLExtensions":[{"UBLExtension":[{"ExtensionURI":[{"_":"urn:oasis:names:specification:ubl:dsig:enveloped:xades"}],"ExtensionContent":[{"UBLDocumentSignatures":[{"SignatureInformation":[{"ID":[{"_":"urn:oasis:names:specification:ubl:signature:1"}],"ReferencedSignatureID":[{"_":"urn:oasis:names:specification:ubl:signature:Invoice"}],"Signature":[{"Id":"signature","Object":[{"QualifyingProperties":[{"Target":"signature","SignedProperties":[{"Id":"id-xades-signed-props","SignedSignatureProperties":[{"SigningTime":[{"_":"2024-07-23T15:14:54Z"}],"SigningCertificate":[{"Cert":[{"CertDigest":[{"DigestMethod":[{"_":"","Algorithm":"http://www.w3.org/2001/04/xmlenc#sha256"}],"DigestValue":[{"_":"KKBSTyiPKGkGl1AFqcPziKCEIDYGtnYUTQN4ukO7G40="}]}],"IssuerSerial":[{"X509IssuerName":[{"_":"CN=Trial LHDNM Sub CA V1, OU=Terms of use at http://www.posdigicert.com.my, O=LHDNM, C=MY"}],"X509SerialNumber":[{"_":"162880276254639189035871514749820882117"}]}]}]}]}]}]}]}],"KeyInfo":[{"X509Data":[{"X509Certificate":[{"_":"MIIFlDCCA3ygAwIBAgIQeomZorO+0AwmW2BRdWJMxTANBgkqhkiG9w0BAQsFADB1MQswCQYDVQQGEwJNWTEOMAwGA1UEChMFTEhETk0xNjA0BgNVBAsTLVRlcm1zIG9mIHVzZSBhdCBodHRwOi8vd3d3LnBvc2RpZ2ljZXJ0LmNvbS5teTEeMBwGA1UEAxMVVHJpYWwgTEhETk0gU3ViIENBIFYxMB4XDTI0MDYwNjAyNTIzNloXDTI0MDkwNjAyNTIzNlowgZwxCzAJBgNVBAYTAk1ZMQ4wDAYDVQQKEwVEdW1teTEVMBMGA1UEYRMMQzI5NzAyNjM1MDYwMRswGQYDVQQLExJUZXN0IFVuaXQgZUludm9pY2UxDjAMBgNVBAMTBUR1bW15MRIwEAYDVQQFEwlEMTIzNDU2NzgxJTAjBgkqhkiG9w0BCQEWFmFuYXMuYUBmZ3Zob2xkaW5ncy5jb20wggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQChvfOzAofnU60xFO7NcmF2WRi+dgor1D7ccISgRVfZC30Fdxnt1S6ZNf78Lbrz8TbWMicS8plh/pHy96OJvEBplsAgcZTd6WvaMUB2oInC86D3YShlthR6EzhwXgBmg/g9xprwlRqXMT2p4+K8zmyJZ9pIb8Y+tQNjm/uYNudtwGVm8A4hEhlRHbgfUXRzT19QZml6V2Ea0wQI8VyWWa8phCIkBD2w4F8jG4eP5/0XSQkTfBHHf+GV/YDJx5KiiYfmB1nGfwoPHix6Gey+wRjIq87on8Dm5+8ei8/bOhcuuSlpxgwphAP3rZrNbRN9LNVLSQ5md41asoBHfaDIVPVpAgMBAAGjgfcwgfQwHwYDVR0lBBgwFgYIKwYBBQUHAwQGCisGAQQBgjcKAwwwEQYDVR0OBAoECEDwms66hrpiMFMGA1UdIARMMEowSAYJKwYBBAGDikUBMDswOQYIKwYBBQUHAgEWLWh0dHBzOi8vd3d3LnBvc2RpZ2ljZXJ0LmNvbS5teS9yZXBvc2l0b3J5L2NwczATBgNVHSMEDDAKgAhNf9lrtsUI0DAOBgNVHQ8BAf8EBAMCBkAwRAYDVR0fBD0wOzA5oDegNYYzaHR0cDovL3RyaWFsY3JsLnBvc2RpZ2ljZXJ0LmNvbS5teS9UcmlhbExIRE5NVjEuY3JsMA0GCSqGSIb3DQEBCwUAA4ICAQBwptnIb1OA8NNVotgVIjOnpQtowew87Y0EBWAnVhOsMDlWXD/s+BL7vIEbX/BYa0TjakQ7qo4riSHyUkQ+X+pNsPEqolC4uFOp0pDsIdjsNB+WG15itnghkI99c6YZmbXcSFw9E160c7vG25gIL6zBPculHx5+laE59YkmDLdxx27e0TltUbFmuq3diYBOOf7NswFcDXCo+kXOwFfgmpbzYS0qfSoh3eZZtVHg0r6uga1UsMGb90+IRsk4st99EOVENvo0290lWhPBVK2G34+2TzbbYnVkoxnq6uDMw3cRpXX/oSfya+tyF51kT3iXvpmQ9OMF3wMlfKwCS7BZB2+iRja/1WHkAP7QW7/+0zRBcGQzY7AYsdZUllwYapsLEtbZBrTiH12X4XnZjny9rLfQLzJsFGT7Q+e02GiCsBrK7ZHNTindLRnJYAo4U2at5+SjqBiXSmz0DG+juOyFkwiWyD0xeheg4tMMO2pZ7clQzKflYnvFTEFnt+d+tvVwNjTboxfVxEv2qWF6qcMJeMvXwKTXuwVI2iUqmJSzJbUY+w3OeG7fvrhUfMJPM9XZBOp7CEI1QHfHrtyjlKNhYzG3IgHcfAZUURO16gFmWgzAZLkJSmCIxaIty/EmvG5N3ZePolBOa7lNEH/eSBMGAQteH+Twtiu0Y2xSwmmsxnfJyw=="}],"X509SubjectName":[{"_":"CN=Trial LHDNM Sub CA V1, OU=Terms of use at http://www.posdigicert.com.my, O=LHDNM, C=MY"}],"X509IssuerSerial":[{"X509IssuerName":[{"_":"CN=Trial LHDNM Sub CA V1, OU=Terms of use at http://www.posdigicert.com.my, O=LHDNM, C=MY"}],"X509SerialNumber":[{"_":"162880276254639189035871514749820882117"}]}]}]}],"SignatureValue":[{"_":"QTvntg4opuS7ZYWmly/iAO2OnLVJcKylYuF+QJKZdx9BkFVglmVuFtEtwoqgNsbsKaaEDinTSUAVStRJs2tiU1Jdryd4hoZ/Hc5TAvFnThpauVOLsc3j07cUB1+zhNjENmFeI9yzTGjr8XfNi4mNPspnhFAT4QGbRpxkWiIsKj762p3dhCwUNAuNLjunVaosYQ5lvSzGt4B9TF/1xJ7Z6kdcJTmBeltTWErSRA2EOMzWsGWGZVvyPLnXfnlIBQItTvARXveafxFdS1iw91g7mSEEYeqEviI0b4FUmkwH8ed0boFc6EHl1VF+2uVxBtHeKf31FqTQl/6/pF4Qgpn6Hg=="}],"SignedInfo":[{"SignatureMethod":[{"_":"","Algorithm":"http://www.w3.org/2001/04/xmldsig-more#rsa-sha256"}],"Reference":[{"Type":"http://uri.etsi.org/01903/v1.3.2#SignedProperties","URI":"#id-xades-signed-props","DigestMethod":[{"_":"","Algorithm":"http://www.w3.org/2001/04/xmlenc#sha256"}],"DigestValue":[{"_":"Rzuzz+70GSnGBF1YxhHnjSzFpQ1MW4vyX/Q9bTHkE2c="}]},{"Type":"","URI":"","DigestMethod":[{"_":"","Algorithm":"http://www.w3.org/2001/04/xmlenc#sha256"}],"DigestValue":[{"_":"vMs/IdnS7isftqrBDr4F1LK/CkvBkW5Gb3Wn6OVzAxo="}]}]}]}]}]}]}]}]}],"Signature":[{"ID":[{"_":"urn:oasis:names:specification:ubl:signature:Invoice"}],"SignatureMethod":[{"_":"urn:oasis:names:specification:ubl:dsig:enveloped:xades"}]}]}]}
```


---

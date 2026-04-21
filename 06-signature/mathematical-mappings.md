<!-- source: https://sdk.myinvois.hasil.gov.my/mathematical-mappings/ -->

# Mathematical Mappings And Calculations

In order to perform mathematical mapping and calculation, follow these guidelines.

| Element | Calculation Equation |
| --- | --- |
| Subtotal | (/ ubl:Invoice / cac:InvoiceLine / cac:Price / cbc:PriceAmount (Unit Price)) \* (/ ubl:Invoice / cac:InvoiceLine / cbc:InvoicedQuantity (Quantity)) |
| Total Excluding Tax (Invoice Line Level) | (/ ubl:Invoice / cac:InvoiceLine / cac:ItemPriceExtension / cbc:Amount) (Subtotal) - SUM(/ ubl:Invoice / cac:InvoiceLine / cac:AllowanceCharge[ChargeIndicator=âfalseâ] / cbc:Amount) (Discount Amount) + SUM(/ ubl:Invoice / cac:InvoiceLine / cac:AllowanceCharge[ChargeIndicator=âtrueâ] / cbc:Amount) (Charge / Fee Amount) |
| Total Excluding Tax (Invoice Level) | (/ ubl:Invoice / cac:LegalMonetaryTotal / cbc:LineExtensionAmount (Total net amount)) - (/ ubl:Invoice / cac:LegalMonetaryTotal / cbc:AllowanceTotalAmount (Total discount value)) + (/ ubl:Invoice / cac:LegalMonetaryTotal / cbc:ChargeTotalAmount (Total charge value(Total fee amount))) |
| Total Including Tax | (/ ubl:Invoice / cac:LegalMonetaryTotal / cbc:TaxExclusiveAmount (Total Excluding Tax)) + (/ ubl:Invoice / cac:TaxTotal / cbc:TaxAmount (Total tax amount on invoice level)) |
| Total Payable Amount (Total Amount) | (/ ubl:Invoice / cac:LegalMonetaryTotal / cbc:TaxInclusiveAmount (Total Including Tax)) - (/ ubl:Invoice / cac:PrepaidPayment / cbc:PaidAmount (Pre-Payment Amount)) + (/ ubl:Invoice / cac:LegalMonetaryTotal / cbc:PayableRoundingAmount) |
| Total net amount | SUM(/ ubl:Invoice / cac:InvoiceLine / cbc:LineExtensionAmount) (Total Excluding Tax) |
| Total discount value | SUM(/ ubl:Invoice / cac:AllowanceCharge[ChargeIndicator=âfalseâ] / cbc:Amount) (Invoice additional discount amount) |
| Total charge value (Total fee amount) | SUM(/ ubl:Invoice / cac:AllowanceCharge[ChargeIndicator=âtrueâ] / cbc:Amount) (Invoice additional fee amount) |
| Total taxable amount per tax type | SUM(/ ubl:Invoice / cac:InvoiceLine / cbc:LineExtensionAmount (Total Excluding Tax) [same tax type]) - (/ ubl:Invoice / cac:InvoiceLine / cac:TaxTotal / cac:TaxSubtotal / cbc:TaxableAmount[cac:TaxCategory / cbc:ID) = âEâ] (Amount Exempted from Tax)) |
| Total tax amount per tax type | SUM(/ ubl:Invoice / cac:InvoiceLine / cac:TaxTotal / cbc:TaxAmount (Tax Amount) [same tax type]) |
| Total tax amount (on invoice level) | SUM(/ ubl:Invoice / cac:TaxTotal / cac:TaxSubtotal / cbc:TaxAmount) (Total tax amount per tax type) |


---

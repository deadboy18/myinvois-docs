<!-- source: https://sdk.myinvois.hasil.gov.my/start/ -->

# Getting started...

This page provides more information regarding the MyInvois System

# LHDNM MyInvois System

To support the growth of the digital economy, the Government intends to implement
e-Invoice in stages in an effort to enhance the efficiency of Malaysia's tax administration management. This initiative is aligned with the Twelfth Malaysia Plan, which aims to strengthen the digital services infrastructure and modernise the tax administration system through digitalisation.

The e-Invoice will enable near real-time validation and storage of transactions, catering to Business-to-Business (B2B), Business-to-Consumer (B2C) and Business-to-Government (B2G) transactions.

## Benefits of Adopting e-Invoice

The implementation of e-Invoice not only provides seamless experience to taxpayers, but also improves business efficiency and increases tax compliance.

Overall benefits of adopting e-Invoice include:

1. Unified invoicing process through the streamlining of transaction document creation, and submission of data electronically to LHDNM. The automation of data entry for transactions reduces manual efforts and human errors;
2. Facilitate tax return filing through seamless system integration for efficient and accurate tax reporting;
3. For larger businesses, the adoption of e-Invoice enables the streamlining of operations, resulting enhanced efficiency and significant time as well as cost savings through automated processes, seamless data integration, and improved invoice management; and
4. For micro, small and medium-sized enterprises (MSMEs), the phased implementation offers a progressive and manageable transition to e-Invoice, allowing MSMEs to align their financial reporting and processes to be digitalised with industry standards, ensuring that MSMEs to adapt over a longer period and mitigating potential disruptions.

## System Overview

The following illustration showcases the primary user groups who will utilise the system, along with the external interaction components. These include:

• taxpayers who may act as the issuer and the receiver   
• tax authority employees tasked in defining and overseeing the process configurations  
• a variety of interactions available   
• authenticating the ERP system associated with an intermediary that is representing a taxpayer (acting on behalf of a specific taxpayer)

For businesses integrating their ERPs with the MyInvois System, this SDK contains two (2) main elements, namely System Integration and Notifications Management:

• System integration provides a set of APIs defined in this SDK, made accessible for taxpayers' use   
• Notifications are sent to selected channels based on the preference of taxpayers' representatives.

The main prerequisite element for interaction is the digital profile of the taxpayer (taxpayer identity), which must be established and set up to enable system access to the aforementioned APIs. This component is part of the taxpayer identity management module within the solution, which is also accessible to taxpayers' representatives via web interfaces. The high-level architecture of the solution, depicting internal modules such as the identity management module, is illustrated in the figure below. Additionally, the figure displays developer resources that highlights e-Invoicing APIs, with the SDK being one of the resources.

# Integration Approach

This section defines multiple aspects of the integration approach that applies to all APIs defined in this SDK.
The approach to integrate ERP systems with the MyInvois System adheres to the following modern principles:

• **externalised identity** - the identities of users and systems interacting with the solution's APIs and user interface are maintained independently from the actual document processing modules.  
• **standards based interfaces** - APIs are designed using RESTful principles, accepting and returning data in JSON format, while the supported document formats include both XML and JSON.   
• **asynchronous processing** - APIs executing more intricate operations, such as validating submitted documents. This improves the response times and reduces potential errors. To facilitate an asynchronous processing approach, the solution exposes APIs that enable result polling.   
• **data protection** - Our solution's APIs prioritises the confidentiality, integrity, and availability of data through stringent data protection measures. To safeguard all interactions with our APIs, we mandate the use of Transport Layer Security (TLS) protocols, establishing secure and encrypted communication channels.
This ensures that data transmitted between clients and our servers are protected against interception and tampering by unauthorised parties. Beyond the secure transmission, we implement an additional layer of security for documents submitted to our system. Each document must be accompanied by a digital signature offering authentication, non-repudiation and integrity verification.

# Feedback or queries

LHDNM is committed to providing exceptional service and solutions to taxpayers while endeavouring to ensure a successful e-Invoice implementation. We appreciate your understanding and welcome your valuable feedback. Please refer to our [contacts page](../contacts.md) to share your thoughts and feedback.

For additional information on e-Invoice and system integration, please refer to the [FAQ](../08-faq/README.md) section of the microsite.


---

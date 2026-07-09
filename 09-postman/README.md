<!-- source: https://sdk.myinvois.hasil.gov.my/postman/ -->

# Postman

> 📦 A ready-to-import Postman collection and environment files are included locally in [`collection/`](collection/). Import `e-Invoice-SDK.postman_collection.json` and whichever environment file you need.


Page describes about the Postman elements, creation and validation.

[Digital Signature](../06-signature/README.md)
[Integration Practices](../01-getting-started/integration-practices.md)

# How to test APIs via Postman?

For those that are using [Postman](https://www.postman.com/) to do quick tests and see how the APIs should be called both on identity service and actual document exchange, please use the Postman collection [e-Invoice SDK.postman\_collection.json](collection/e-Invoice-SDK.postman_collection.json) (right-click to Save ) and use Postman as per the environment below (right-click to Save ).

| Environment | Postman Env. File |
| --- | --- |
| PROD | [PROD Env.postman\_environment.json](collection/PROD-Env.postman_environment.json) |
| SANDBOX | [Sandbox Env.postman\_environment.json](collection/Sandbox-Env.postman_environment.json) |

To install the environments file, follow these steps:

1. On the top right-hand corner, click on the gear icon named `Manage Environments`
2. A `Manage Environments` window will pop-up, click on the `Import` button and select the right environment file and click `Ok`
3. A new environment named `Sandbox Env` or `Prod Env` should be added to the drop down list on the top right corner, click again on the `gear icon` and click on the newly added environment named `Sandbox Env` or `Prod Env` (Based on the environment you want to work on)

**Note**: If you encounter an error message "SSL Error: Self signed certificate in certificate chain" (see image below) on PostMan, click "Disable SSL Verification".

Alternatively, click on the `Gear Icon` button at the top right-hand corner to open the Settings. Turn off "SSL Certificate Verification" as shown in the image below.

[Digital Signature](../06-signature/README.md)
[Integration Practices](../01-getting-started/integration-practices.md)


---

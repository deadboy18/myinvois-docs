# klsheng/myinvois-php-sdk

**Language:** PHP ≥ 7.4  
**License:** MIT  
**Packagist:** [`klsheng/myinvois-php-sdk`](https://packagist.org/packages/klsheng/myinvois-php-sdk)  
**Repo:** <https://github.com/klsheng/myinvois-php-sdk>  
**Author:** Sean Kau (klsheng)

The most mature and complete of the community SDKs. If you're on PHP, start here.

## Install

```bash
composer require klsheng/myinvois-php-sdk "*"
```

### System requirements

- PHP ≥ 7.4
- Extensions: `curl`, `json`, `openssl`
- Composer dependencies (handled automatically):
  - `guzzlehttp/guzzle` ~7.0
  - `sabre/xml` ^4.0
  - `psr/http-client` ^1.0 || ^2.0

## Authentication

### As a taxpayer

```php
use Klsheng\Myinvois\MyInvoisClient;

$prodMode = false;  // false = sandbox, true = production
$client = new MyInvoisClient('client_id', 'client_secret', $prodMode);

$client->login();
$access_token = $client->getAccessToken();
// Store $access_token somewhere to re-use it within 1 hour

// To restore from cache later:
$client->setAccessToken('cached_access_token');
```

### As an intermediary

```php
$client = new MyInvoisClient('client_id', 'client_secret', $prodMode);
$client->login($onbehalfof);  // TIN of the taxpayer you're representing
// Or, if reusing token:
$client->setAccessToken('cached_access_token');
$client->setOnbehalfof($onbehalfof);
```

## Endpoint coverage

Every documented MyInvois endpoint is wrapped:

| Category | Methods |
|---|---|
| Document types | `getAllDocumentTypes()`, `getDocumentType($id)`, `getDocumentTypeVersion($id, $versionId)` |
| Notifications | `getNotifications($dateFrom, $dateTo, $type, $language, $status, $pageNo, $pageSize)` |
| Taxpayer | `validateTaxPayerTin($tin, $idType, $idValue)`, `searchTaxPayerTin(...)`, `getTaxPayerFromQrcode($qrCodeText)` |
| Submission | `submitDocument($documents)` |
| Document ops | `cancelDocument($id, $reason)`, `rejectDocument($id, $reason)` |
| Retrieval | `getRecentDocuments(...)`, `getSubmission($id)`, `getDocument($id)`, `getDocumentDetail($id)`, `searchDocuments(...)` |
| QR | `generateDocumentQrCodeUrl($id, $longId)` |

## Creating a UBL document

The SDK ships with a re-created UBL-Invoice package because, per the
maintainer's note:

> MyInvois System doesn't fully support UBL v2.1 format, so UBL package
> re-create based on original author to support MyInvois System.

Example (JSON):

```php
use Klsheng\Myinvois\Helper\MyInvoisHelper;
use Klsheng\Myinvois\Example\Ubl\CreateDocumentExample;
use Klsheng\Myinvois\Ubl\Constant\InvoiceTypeCodes;

$id = 'INV20240418105410';
$supplier = ['TIN' => 'C00000000000', 'BRN' => '0000000-T'];
$customer = ['TIN' => 'C00000000000', 'BRN' => '0000000-T'];
$delivery = ['TIN' => 'C00000000000', 'BRN' => '0000000-T'];

$example = new CreateDocumentExample();
$invoice = $example->createJsonDocument(
    InvoiceTypeCodes::INVOICE,
    $id, $supplier, $customer, $delivery,
    true,                             // include signature
    '/path/to/eInvoice.crt',
    '/path/to/eInvoice.key'
);

// For .p12 / .pfx instead:
// $invoice = $example->createJsonDocument(
//     InvoiceTypeCodes::INVOICE, $id, $supplier, $customer, $delivery,
//     true, '/path/to/eInvoice.p12', null, 'passphrase'
// );

$documents = [MyInvoisHelper::getSubmitDocument($id, $invoice)];
$response = $client->submitDocument($documents);
```

XML submission uses `createXmlDocument(...)` with the same signature.

### Handling DS326 (issuer name mismatch)

The README explicitly calls this out as a known issue — one of the reasons
the SDK is so valuable. See [02-signature-errors.md](../02-signature-errors.md#ds326-deep-dive)
for full context.

```php
// Default RDN sequence: ['CN', 'E', 'OU', 'O', 'C']
// If you get DS326, try reversing:
$invoice = $example->createJsonDocument(
    InvoiceTypeCodes::INVOICE, $id, $supplier, $customer, $delivery,
    true, '/path/to/eInvoice.p12', null, 'passphrase',
    ['C', 'O', 'OU', 'E', 'CN']  // custom RDN order
);
```

## Supported invoice types

Per the TODO in the README (all completed):

- Standard Invoice
- Credit Note
- Debit Note
- Refund Note
- Self-Billed Invoice
- Self-Billed Credit Note
- Self-Billed Debit Note
- Self-Billed Refund Note

## Number formatting

Control decimal precision and thousands separator globally:

```php
use Klsheng\Myinvois\Ubl\Configuration\NumberFormatConfiguration;

// Call before any build() operations
NumberFormatConfiguration::setPrecision(3);
NumberFormatConfiguration::setThousandsSeparator(',');
// 1436.5153 → "1,436.515"
```

Default formatting is 2 decimal places, no thousands separator.

## QR code URL

```php
$id = '0000000000000';          // Document UUID
$longId = '11111111111111';     // Long ID
$url = $client->generateDocumentQrCodeUrl($id, $longId);
```

Produces the validation URL — feed into any QR code generator.

## Search documents

```php
$submissionDateFrom = new \DateTime('2024-12-31T00:00:00Z');
$submissionDateTo = '2025-01-02T00:00:00Z';

$response = $client->searchDocuments(
    null, $submissionDateFrom, $submissionDateTo
);

// With all filters:
$response = $client->searchDocuments(
    'F9D425P6DS7D8IU',              // UUID
    $submissionDateFrom, $submissionDateTo,
    1, 100,                         // pageNo, pageSize
    null, null,                     // issueDateFrom, issueDateTo
    'Sent',                         // direction
    'Valid',                        // status
    '01',                           // documentType
    null                            // searchQuery
);
```

## Upgrade path (v1.0.6+)

Per [UPGRADE.md](https://github.com/klsheng/myinvois-php-sdk/blob/main/UPGRADE.md):

### Upgrading to 1.0.6

- `getNotifications()` in `NotificationService`: the `channel` parameter was
  removed. Remove it from your calls.
- `searchDocuments()` in `DocumentService`: the `continuationToken` parameter
  was removed and `pageNo` was added. Adjust pagination logic.

## Without a framework (Pure PHP)

There's a `PurePHPExample` folder in `src/Example/` with a README showing
usage outside Laravel/Yii/etc. Useful if you're integrating into a legacy
PHP codebase.

## Strengths

- ✅ Complete endpoint coverage
- ✅ Actively maintained (v1.0.14 as of January 2026, 31 releases total)
- ✅ DS326 workaround built in
- ✅ Supports `.crt + .key` and `.p12 / .pfx`
- ✅ Re-created UBL library handles MyInvois-specific quirks
- ✅ Number formatting is configurable globally
- ✅ Upgrade notes are published per version
- ✅ MIT license

## Weaknesses

- ❌ No auto-retry on 429 (you implement this yourself)
- ❌ No built-in observability / correlation-ID logging
- ❌ Examples use hardcoded test data — need to study the code carefully to
  find where your own values go
- ❌ No async / Promise support (blocking Guzzle calls only)
- ⚠️ PHP 7.4 minimum; no PHP 8.x-specific features

## Internal architecture notes

Namespace layout:

```
Klsheng\Myinvois\
├── MyInvoisClient          (main entry point)
├── Helper\
│   └── MyInvoisHelper      (utility methods like getSubmitDocument)
├── Service\
│   ├── Notification\       (Notification endpoints)
│   └── Document\           (Document endpoints)
├── Ubl\
│   ├── Constant\           (InvoiceTypeCodes, etc.)
│   └── Configuration\      (NumberFormatConfiguration)
└── Example\
    ├── PurePHP\            (example without framework)
    └── Ubl\
        └── CreateDocumentExample
```

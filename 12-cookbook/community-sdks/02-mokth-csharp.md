# mokth/einvoice

**Language:** C# / .NET  
**License:** MIT (implied — see repo)  
**Repo:** <https://github.com/mokth/einvoice>  
**Author:** mokth (Angel May)

A collection of working C# / .NET samples for MyInvois integration. Not
packaged as a NuGet library — you clone and adapt. Strongest at signature
handling (multiple X509 tools) and cross-platform console apps.

## Repo layout

The repo is structured as **multiple sample projects**, not a single library:

```
einvoice/
├── EDocumentApp/              (main .NET console app)
├── WinEInvoiceApp/            (Windows desktop GUI app)
├── X509CertificateTool/       (certificate manipulation utilities)
├── XmlSignatureTest/          (XAdES signature test harness)
├── hashingUtilities/          (SHA-256 + Base64 helpers)
├── jsonsamples/               (JSON invoice sample files)
├── xmlsample/                 (XML invoice sample files)
├── documents/                 (reference docs)
├── middleware/                (sample middleware integration)
├── linux/einvoiceapp/         (Linux build of the console app)
├── Step to Generate Signature (updated).pdf
└── stepToGenerateJsonDC.pdf
```

## What each sub-project does

### `EDocumentApp/`
The main console app. Orchestrates end-to-end: login → build invoice → sign →
submit. Your starting point for understanding the flow.

### `WinEInvoiceApp/`
A Windows Forms desktop app that wraps the console logic in a UI. Good for
testing interactively without writing integration code.

### `X509CertificateTool/`
Standalone utility for inspecting and manipulating X.509 certificates. Shows
how to:
- Load `.p12` / `.pfx` with passphrase
- Extract the subject's RDN components (for DS326 debugging)
- Export public cert in DER / PEM formats
- Compute cert hash (for the `CertDigest` field)

### `XmlSignatureTest/`
XAdES enveloped signature implementation in pure .NET. Reference code if
you're building your own signer. Pay attention to:
- Canonicalization (`xml-c14n11`)
- SignedProperties generation
- Digest chaining between `DocDigest`, `CertDigest`, `PropsDigest`

### `hashingUtilities/`
Minimal SHA-256 + Base64 helpers. Mostly a reminder of the common mistake
pattern: hash the **bytes** of the canonicalized content, then base64-encode
the hash bytes. Don't base64 first.

### `middleware/`
Sample integration middleware showing how to insert the e-Invoice submission
into an existing ERP workflow. Generic enough to adapt to any .NET ERP.

### `linux/einvoiceapp/`
Cross-platform build (Linux, uses .NET Core / 5+). Confirms the whole
pipeline works on Linux — useful if you're deploying on Linux containers.

## Supplementary PDFs

Two embedded reference PDFs in the repo root:

- **Step to Generate Signature (updated).pdf** — the author's walkthrough of
  XAdES signing, customised for MyInvois
- **stepToGenerateJsonDC.pdf** — same but for JSON invoices (not XML)

These are community-written, not official — but they're concrete step-by-step.

## Using the repo

It's not a NuGet package, so:

```bash
git clone https://github.com/mokth/einvoice.git
cd einvoice
# open in Visual Studio, JetBrains Rider, or VS Code
# pick a sub-project and either run or extract components
```

Typical extraction approach:

1. Take the `X509CertificateTool/` → use its cert-loading code.
2. Take the `hashingUtilities/` → use its SHA256 helpers.
3. Take the `XmlSignatureTest/` → use its signature composer.
4. Wire them into **your own** submission logic (the repo's `EDocumentApp/`
   is a reference, not a drop-in library).

## Strengths

- ✅ Strong signature handling — the author has clearly done deep work on XAdES
- ✅ Multiple runtime targets (console, Windows GUI, Linux)
- ✅ Includes X509 debugging tools (useful when fighting DS326 / DS329)
- ✅ Mature (181 commits as of April 2026)
- ✅ Sample JSON + XML files in repo for quick testing

## Weaknesses

- ❌ **Not a library.** No NuGet package. You copy-paste code.
- ❌ No unified class / API — pick components from different sub-projects
- ❌ Documentation is sparse (README is one line). Reliance on the embedded PDFs.
- ❌ No test suite visible
- ❌ Error handling varies per project
- ❌ No upgrade notes if the author revises the signing pipeline

## When to pick this

- You're on .NET and want **full control** of the pipeline
- You need to **understand** XAdES signing deeply (not just consume an API)
- You're building an ERP plugin in C# and want reference code for each stage
- You're troubleshooting a signature issue in another SDK and need a
  reference implementation to compare digest values against

## When to skip this

- You want a plug-and-play SDK — there isn't one here
- You need production-grade error handling out of the box
- You're in a hurry and just want to submit invoices

## A note on `X509CertificateTool`

If you're debugging DS326, you can use this tool to verify what
`X509IssuerName` your cert actually emits. Run it with your `.p12` and note
the Issuer string exactly — compare to what your SDK wrote into the signed
properties. They must match character-for-character.

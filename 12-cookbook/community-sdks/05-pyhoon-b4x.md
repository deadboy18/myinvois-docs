# pyhoon/lhdn-einvoice-api-client-b4x

**Language:** B4X (Basic4X — cross-platform BASIC-dialect)  
**License:** MIT  
**Repo:** <https://github.com/pyhoon/lhdn-einvoice-api-client-b4x>  
**Author:** Aeric Poon (pyhoon)  
**Version:** 1.10  
**Forum thread:** <https://www.b4x.com/android/forum/threads/b4x-malaysian-e-invoice-api-client.162649/>

**Not a library.** This is a **reference UI client app** that calls the MyInvois
API. Useful as:
- A working demo you can run on Windows / Linux / macOS / Android / iOS
- A reference for the minimum HTTP flow if you're writing a client from scratch
- A quick-test sandbox before you commit to an SDK

If you've never heard of B4X: it's a free BASIC-like RAD tool from Anywhere
Software. Cross-compiles to native apps on all five major platforms from a
single codebase.

## Prerequisites

Different platforms need different B4X variants:

| Platform | Tool | License |
|---|---|---|
| Windows / Linux / Mac desktop | [B4J](https://www.b4x.com/b4j.html) | Free |
| Android | [B4A](https://www.b4x.com/b4a.html) | Free |
| iOS | [B4i](https://www.b4x.com/b4i.html) | Paid — [B4i store](https://www.b4x.com/store.html) |

Plus:
- **Java JDK** (11 / 14 / 19) — required by the IDE
- **Apple Developer account** — for iOS builds
- **Mac + Xcode** — to build iOS. Alternatively, use B4X's [hosted builder](https://www.b4x.com/store.html).

### Additional library

- The project needs B4X's [Encryption library](https://www.b4x.com/android/forum/attachments/encryption1-1-zip.6666/).
- Download and unzip into your B4A/B4J/B4X additional-libraries folder.
- Refresh the IDE's Libraries Manager tab.

## Setup

```bash
git clone https://github.com/pyhoon/lhdn-einvoice-api-client-b4x.git
# or download the ZIP
```

### Configure

1. Open `Shared Files/config.properties` in a text editor.
2. Fill in your sandbox `clientId` and `clientSecret`.
3. Save.

### Build profiles

The project ships with a `Default` build configuration pointing at the LHDN
**sandbox** environment. For production:

1. In the IDE, next to the Debug/Release dropdown, choose the **`Production`**
   build configuration.
2. Make sure `config.properties` has production credentials.
3. Rebuild.

Same applies to all platforms (B4J, B4A, B4i).

> **Safety note from the author:** Do not distribute the binary app to the
> public with real production credentials embedded. This is a test tool.

## Project files

Open the per-platform project file:

| Platform | File |
|---|---|
| B4J (desktop) | `lhdn-einvoice-api-client-b4x.b4j` |
| B4A (Android) | `lhdn-einvoice-api-client-b4x.b4a` |
| B4i (iOS) | `lhdn-einvoice-api-client-b4x.b4i` |

### Running the app

1. Click **Run** (▶).
2. Wait for compilation.
3. The UI launches with a dropdown/ComboBox at the top.

### Workflow in the UI

1. Select an API from the dropdown (e.g. "Login as Taxpayer System").
2. Click the green **SUBMIT** button.
3. For the login flow, the app obtains an `access_token`.
4. Select another API from the dropdown and click SUBMIT again to invoke it.

### Where to plug in your own code

Per the README, the subs (functions) to focus on:
- `B4XComboBox1_SelectedIndexChanged` — when you pick an API from the dropdown
- `BtnSubmit_Click` — when you click SUBMIT

Replace the placeholder TIN / UUID values in those subs with your real values.

### Replacing the sample payloads

Replace:
- `1.x-Invoice-Sample.xml` — the XML sample payload
- `1.x-Invoice-Sample.json` — the JSON sample payload

These are referenced during the Submit Documents flow.

## Supported endpoints

The app demonstrates all the fundamental flows:

- Login as Taxpayer System
- Login as Intermediary System
- Validate Taxpayer TIN
- Submit Documents (XML + JSON)
- Cancel Document
- Get Document, Get Submission (varies by project version)

It's not exhaustive — think of it as a **touring guide** through the API, not
a reference for every endpoint.

## Strengths

- ✅ **Truly cross-platform** — single codebase runs on 5 OSes
- ✅ Interactive UI for manual exploration — great for pre-integration learning
- ✅ MIT licensed
- ✅ Low-friction initial run — just open and click
- ✅ Source code is readable even if you don't know B4X (BASIC-like)

## Weaknesses

- ❌ **Not a library.** You cannot `npm install` / `composer require` it.
- ❌ Niche toolchain — B4X has a small community, and onboarding takes time
- ❌ iOS builds require Mac + Apple Dev account (standard iOS limitations)
- ❌ Low activity (9 stars) — you're largely on your own for support
- ❌ UI is minimal; no production-grade error display or retry handling
- ❌ No unit tests

## Troubleshooting

From the README:

### "File 'xxx.xxx' is missing from the Files tab (warning #17)"

First-run compile copies files from `Shared Files/` → `Files/`. Either:
- Compile once (IDE auto-copies), or
- Run the `#Macro: Sync Files` (Ctrl + 3)

### "File 'xxx.xxx' in Files folder was not added to the Files tab (warning #14)"

Files Manager tab → click **Sync**.

### B4J compilation error

Verify JDK version. The project needs the specific Oracle JDK 11 / 14 / 19 listed
on the B4X install page — bundled **with** JavaFX. OpenJDK without JavaFX will
fail.

## When to pick this

- You want to **see the API work end-to-end** before writing your own client
- You need an installable demo app you can share with stakeholders (finance,
  accountants) to let them verify flows
- You're **curious about B4X** and have a reason to try it

## When to skip this

- You want a library to call from your code
- You need production-grade error handling
- You don't want to install another IDE

## Companion — the B4X forum thread

The author's [forum thread](https://www.b4x.com/android/forum/threads/b4x-malaysian-e-invoice-api-client.162649/)
has some version history, community questions, and screenshots. Worth skimming
if you're actually going to build the project.

## Lesson for everyone else

Even if you're not using B4X, this project is a useful **minimum-viable-client
reference**. It shows what the smallest functional MyInvois client looks like:

1. HTTP client (any)
2. JSON / XML serializer (any)
3. Config loader
4. Token cache
5. SHA-256 + Base64 helpers
6. A handful of API-call wrappers

That's it. No magic, no UBL library even — the sample XML/JSON files are
hand-crafted. If you're thinking "how much do I really need to build?" — this
is your answer at the low end.

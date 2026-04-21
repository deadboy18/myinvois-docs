# 07 — Special Characters Encoding

When your document content contains characters that are reserved in XML or
JSON, they must be escaped. Failing to do this produces `DS300` / "Failed to
parse input document" errors from the Structure Validator.

## XML escape sequences (required)

| Character | Escape sequence | Meaning |
|---|---|---|
| `<` | `&lt;` | Less than |
| `>` | `&gt;` | Greater than |
| `&` | `&amp;` | Ampersand |
| `"` | `&quot;` | Double quote |
| `'` | `&apos;` | Single quote |

**Note:** in element text content, only `<`, `>`, and `&` are *strictly*
required to be escaped. In attribute values, `"` and `'` additionally matter
depending on which quote delimits the attribute. Escape all five to be safe.

### Examples

```xml
<!-- Wrong (malformed XML) -->
<cbc:Name>Smith & Associates</cbc:Name>
<cbc:Description>Price < $100 and quality > standard</cbc:Description>

<!-- Correct -->
<cbc:Name>Smith &amp; Associates</cbc:Name>
<cbc:Description>Price &lt; $100 and quality &gt; standard</cbc:Description>
```

### Use CDATA for long free text

If a field has lots of special characters (e.g. a note field with HTML-ish
text), use CDATA sections:

```xml
<cbc:Note><![CDATA[This is <complex> & messy text with "quotes"]]></cbc:Note>
```

**But note:** CDATA affects canonicalization. XAdES canonicalization (c14n11)
converts CDATA back to escaped text before hashing, so the doc digest is the
same either way. Still, some parsers behave differently — test both forms if
you're unsure.

## JSON escape sequences (required)

| Character | Escape sequence | Meaning |
|---|---|---|
| `"` | `\"` | Double quote |
| `\` | `\\` | Backslash |
| `/` | `\/` | Forward slash (optional in JSON spec, but MyInvois docs list it) |
| (backspace) | `\b` | Backspace |
| (form feed) | `\f` | Form feed |
| (newline) | `\n` | Newline |
| (carriage return) | `\r` | Carriage return |
| (tab) | `\t` | Tab |

### Examples

```json
// Wrong (invalid JSON)
{ "Description": "Line 1
Line 2" }
{ "Path": "C:\Users\Data" }

// Correct
{ "Description": "Line 1\nLine 2" }
{ "Path": "C:\\Users\\Data" }
```

### The forward slash thing

JSON spec allows `/` unescaped. MyInvois docs list `\/` as a "required" escape.
In practice, most parsers accept both. Any standard JSON library
(`JSON.stringify` in JS, `json.dumps` in Python, `json_encode` in PHP) will
produce valid output — you don't need to hand-escape.

## Unicode characters

- **UTF-8 encoding is required** for both XML and JSON.
- Non-ASCII characters (e.g. Malay characters with diacritics, Chinese
  characters in business names) should be in native UTF-8 form — do not
  escape them with `\u` sequences unless your platform requires it.
- Remove any **BOM (byte-order mark)** at the start of the file. Some editors
  add it silently; it breaks canonicalization.

### Checking for BOM

```bash
# First 3 bytes of a BOM'd UTF-8 file are EF BB BF
head -c 3 your-file.xml | xxd
# If output shows 'efbbbf' you have a BOM to strip
```

```python
# Strip BOM in Python
content = open("file.xml", "rb").read()
if content.startswith(b'\xef\xbb\xbf'):
    content = content[3:]
```

## XML declaration handling

Per the PosDigicert signature guideline, **before canonicalization you must
remove the XML declaration** (`<?xml version="1.0" encoding="UTF-8"?>`).

Most signing libraries handle this automatically as part of canonicalization.
But if you're building the document manually:

```xml
<!-- Wrong — has the XML declaration as first line -->
<?xml version="1.0" encoding="UTF-8"?>
<Invoice xmlns="urn:oasis:names:specification:ubl:schema:xsd:Invoice-2">
...

<!-- Right for signing — no XML declaration, starts with root element -->
<Invoice xmlns="urn:oasis:names:specification:ubl:schema:xsd:Invoice-2">
...
```

Note: when you **submit** the signed document over HTTP, you may include the
XML declaration back at the top (and often should, for correctness). The
removal is specifically for the canonicalization step before hashing.

## Whitespace in signed properties

Step 7 of signature creation requires:

> Linearize the XML block (properties tag) and **remove the spaces**

This is not the same as canonicalization. For the `SignedProperties` section
specifically, strip all whitespace including newlines and indentation before
hashing. If you hash a "pretty-printed" version, you'll get `DS320`.

```java
// Strip whitespace between tags
String linearized = signedPropsXml.replaceAll(">\\s+<", "><").trim();
```

## `_` key in UBL JSON

Not technically an escape, but worth repeating — every scalar value in UBL
JSON must be wrapped in an object keyed by `_`:

```json
// Wrong
{ "InvoiceTypeCode": "02" }

// Right
{ "InvoiceTypeCode": { "_": "02" } }
```

This is a UBL 2.1 JSON binding convention, not something MyInvois invented —
but it catches everyone out on their first integration.

## Common encoding-related error patterns

| Symptom | Likely cause |
|---|---|
| `DS300 Failed to parse input document` | Unescaped `&`, `<`, or `>` in element text |
| `DS322 Document digest value doesn't match` | Canonicalization differs between your side and theirs — BOM, XML declaration not stripped, or whitespace handling |
| `DS320 Signed properties digest value doesn't match` | SignedProperties not whitespace-stripped before hashing |
| Parser error from the client side before submission | Your XML is malformed — usually a missing close tag or an unescaped `&` |
| `Error01 / Invalid Structure` | Element order wrong, or unexpected element (not an encoding issue per se, but sometimes misreported) |

## Reference

- [Official FAQ → Special characters](https://sdk.myinvois.hasil.gov.my/faq/#how-to-handle-special-characters-in-xml-and-json-documents-when-submitting-e-invoices)
- [Signature → Creation](https://sdk.myinvois.hasil.gov.my/signature/)
- [PosDigicert Signature Guideline v1.2](https://www.posdigicert.com.my/public/uploads/files/PosDigicert-Document_Signature_Creation_Guideline_Rev1.pdf)

# Contributing

Thanks for helping improve this developer reference! This is a
community-maintained resource — every contribution keeps it useful for the
next integrator.

## Quick links

- [What to contribute](#what-to-contribute)
- [How to open a PR](#how-to-open-a-pr)
- [Style & formatting guide](#style--formatting-guide)
- [Keeping in sync with upstream](#keeping-in-sync-with-upstream)
- [Link-check script](#link-check-script)

---

## What to contribute

### High-value additions

- **New error codes you encountered and resolved** — the single most
  valuable type of contribution. Include:
  - The exact error code (e.g. `CF421`)
  - The full error message (English + Malay if available)
  - What triggered it in your case
  - How you fixed it
  - When you hit it (month + year — LHDN adds codes over time)
  - Place it in [`12-cookbook/03-submission-errors.md`](12-cookbook/03-submission-errors.md)
    or the appropriate cookbook page
- **Newly discovered community SDKs** — add to
  [`12-cookbook/community-sdks/`](12-cookbook/community-sdks/) with a writeup
  matching the existing templates
- **Industry-specific gotchas** — banking, insurance, construction,
  healthcare, aviation, etc.
- **Real-world integration stories** — what went wrong on your first day
  in production, what you wish you'd known
- **Code examples** in languages not yet covered (Go, Java, Ruby, Rust)
- **Diagrams** — ASCII art is fine; SVG/Mermaid also welcome

### LHDN publication updates

When LHDN publishes:

- **A new Guideline version** (General or Specific) → update the rendered
  markdown in [`11-irbm-specific-guideline/`](11-irbm-specific-guideline/)
  or [`12-cookbook/irbm-general-guideline/`](12-cookbook/irbm-general-guideline/);
  bump versions in README + CHANGELOG
- **New release notes** → add to [`release-notes/`](release-notes/)
- **New code-table entries** (currencies, tax types, etc.) → update the
  relevant file in [`05-code-tables/`](05-code-tables/)
- **New API endpoint or parameter** → add / update the appropriate page in
  [`02-platform-api/`](02-platform-api/) or [`03-einvoice-api/`](03-einvoice-api/)
- **Updated FAQ** → refresh [`08-faq/`](08-faq/)

---

## How to open a PR

1. **Fork** the repo.
2. **Branch** from `main` — use a descriptive name:
   `feat/cf421-error-code`, `fix/ds326-rdn-examples`, `docs/general-guideline-v4-7`
3. **Make your changes** — one logical change per PR where possible.
4. **Verify all links still resolve** — see the [link-check script](#link-check-script) below.
5. **Update `CHANGELOG.md`** under an "Unreleased" section.
6. **Open the PR** with:
   - Clear summary of what changed and why
   - If adding an error code, include: evidence (redacted response body),
     remediation, date you hit it
   - If updating for a new Guideline version, link to the official PDF
7. **Respond to review comments.** Reviewers may ask for source links.

### For large PRs

Open an issue first to discuss the shape of the change. Makes sure your
effort isn't wasted on an approach the maintainers don't want to merge.

---

## Style & formatting guide

- **Use consistent English within a page.** LHDN's own docs mix styles;
  follow their term-by-term (e.g. "e-Invoice" with hyphen and capital I is
  the official spelling).
- **Preserve official terminology** — use "taxpayer", "supplier", "buyer",
  "intermediary" as IRBM does. Do not paraphrase technical terms.
- **Link relatively** — `[Submit Documents](03-einvoice-api/02-submit-documents.md)`
  from root, `[Submit Documents](../03-einvoice-api/02-submit-documents.md)`
  from a subfolder — not absolute URLs.
- **Code blocks** — fence with triple backticks and language hint
  (`http`, `json`, `xml`, `bash`, `python`, `javascript`, `php`).
- **Tables** — use markdown table syntax (`| col | col |`) unless data is
  too wide, then use fenced preformatted blocks.
- **External links** — always to official sources when possible (OASIS for
  UBL, IRBM for tax rules, MCMC for certificate authorities).
- **No screenshots** — they go stale and aren't searchable. Prefer
  well-formatted tables or code samples.
- **Honest scope** — say what you don't know. Mark open questions with
  `> TODO` so the next contributor can pick them up.

### Naming conventions

- Files: `kebab-case.md`
- Folder prefixes: numeric (`01-`, `02-`) for ordered folders
- Error code callouts: use inline `code` formatting for codes
  (`` `DS326` ``, `` `CF401` ``)

### What NOT to do

- **Don't paste copyrighted PDF content verbatim.** Summarize and paraphrase
  with attribution instead.
- **Don't include real TINs / credentials / cert private keys.** Even in
  examples — use obvious placeholders like `C00000000000`, `cert_public_xyz`.
- **Don't make promises we can't keep.** No "this works in 100% of cases"
  language. Always say "observed as of [date]" when reporting behavior.
- **Don't rewrite whole sections for style alone.** Churn makes review
  harder. Tighten in-place.

---

## Keeping in sync with upstream

The content here was scraped from the official SDK site and the IRBM PDFs.
When upstream updates:

1. **Check** <https://sdk.myinvois.hasil.gov.my/release-notes/> for changes.
2. **Re-scrape** the affected pages.
3. **Diff** against this repo's version.
4. **Update** the markdown. Preserve our in-repo link style — don't replace
   with absolute SDK URLs.
5. **Log** the update in `CHANGELOG.md`.
6. **Bump** the "Last verified" date in the main README.

### Scrape notes

Official SDK uses a Material-for-MkDocs-style site. Each page is a standalone
HTML that can be converted to markdown. A simple approach:

```bash
# Mirror the site
wget --mirror --convert-links --adjust-extension --page-requisites \
     --no-parent https://sdk.myinvois.hasil.gov.my/

# Convert each HTML page to markdown (example using pandoc)
find sdk.myinvois.hasil.gov.my -name "*.html" -exec \
  bash -c 'pandoc -f html -t markdown_strict "$1" -o "${1%.html}.md"' _ {} \;
```

Then manually clean up the output — the scraper is never perfect.

---

## Link-check script

Run this before submitting a PR to catch broken internal links:

```bash
python3 - << 'EOF'
import re
from pathlib import Path

root = Path(".").resolve()
link_re = re.compile(r'\[([^\]]+)\]\(([^\)]+)\)')
broken = []

for f in sorted(root.rglob("*.md")):
    for m in link_re.finditer(f.read_text(encoding="utf-8")):
        target = m.group(2)
        if target.startswith(("http://", "https://", "mailto:", "#", "tel:")):
            continue
        path = target.split("#")[0]
        if not path:
            continue
        if not (f.parent / path).resolve().exists():
            broken.append(f"{f.relative_to(root)} → {target}")

if broken:
    print("Broken links:")
    print("\n".join(broken))
    exit(1)
else:
    print("All internal links OK")
EOF
```

---

## Maintainers

Currently self-maintained by the initial author. If you'd like to become a
maintainer, open an issue describing your interest and contribution history.

## License

By contributing, you agree that your contributions will be licensed under
the same **MIT License** that covers the repository. See [`LICENSE.md`](LICENSE.md).

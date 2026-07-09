# 17 — Automated Update Checking with Claude

This repo includes a **Claude Skill** that enables AI-assisted auditing of the
MyInvois SDK portal and IRBM guideline PDFs against this repository. When
activated, Claude can automatically scrape the live SDK site, compare it against
the repo, identify gaps, generate patch files, and provide exact git commands
to push updates.

## What the skill does

When you say things like "check for SDK updates", "is the repo up to date",
or "scrape the MyInvois portal", Claude will:

1. **Check SDK release notes** — compare dated entries on the live page against
   `release-notes/sdk-1-0.md` to find missing entries
2. **Check the FAQ** — compare the live FAQ against `08-faq/README.md` for
   new Q&A entries
3. **Check IRBM guideline versions** — verify whether LHDN has published a
   newer General or Specific Guideline PDF
4. **Spot-check high-change pages** — code tables, validation rules, API
   endpoints that the release notes reference
5. **Build patches** — generate updated files, CHANGELOG entry, and a zip
   you can extract and push

The skill file is at `.claude/skills/myinvois-sdk-auditor/SKILL.md` and contains
the complete URL-to-file mapping, audit workflow, encoding fix patterns, and
version baseline.

## How to use it

### Option A: Claude.ai (web / app)

1. Go to [claude.ai](https://claude.ai) → **Settings** → **Skills**
2. Upload the `SKILL.md` file from `.claude/skills/myinvois-sdk-auditor/`
3. Start a new conversation and say: **"check for SDK updates"**
4. Upload your current repo as a zip if Claude needs to compare files
5. Claude will run the audit and produce patch files if anything is out of date

### Option B: Claude Code (terminal)

If you use [Claude Code](https://docs.anthropic.com/en/docs/claude-code),
the skill is auto-detected from the `.claude/skills/` directory when you run
Claude Code inside the repo:

```bash
cd myinvois-docs
claude "check for SDK updates"
```

### Option C: Any Claude conversation (manual)

Even without installing the skill, you can copy-paste the contents of
`SKILL.md` into a Claude conversation as context, then ask it to audit the repo.
The skill is just a structured prompt — no special access required.

## Example prompts

| What you want | What to say |
|---|---|
| Full audit | "Check for SDK updates" |
| Just release notes | "Are there new SDK release notes since the last update?" |
| Just guidelines | "Has LHDN published a new e-Invoice guideline version?" |
| Fix encoding | "Check for mojibake / encoding issues in the repo" |
| Specific page | "Compare the live FAQ page against 08-faq/README.md" |

## What it checks (35+ pages)

The skill maps every page on the SDK portal to its corresponding repo file:

- **17 API endpoint pages** (platform + e-invoice APIs)
- **9 code table pages** (countries, currencies, MSIC, state codes, etc.)
- **Signature, validation, FAQ, contacts, integration practices**
- **SDK release notes** (the fastest signal for what changed)
- **IRBM guideline PDFs** (General + Specific Guideline versions)

## Limitations

- **Claude can't always fetch the latest content** — the SDK portal sometimes
  serves cached pages. If Claude reports "no changes" but you see something
  new on the live site, copy-paste or screenshot the content for Claude.
- **Guideline PDFs require upload** — Claude can detect a new version number
  but needs the actual PDF uploaded to extract and convert the content.
- **Code table JSON files** (`10-sample-documents/samples/*.json`) are not
  automatically updated — these are lower priority and need manual download.
- **The skill is a knowledge shortcut, not a permissions upgrade** — Claude
  uses the same web search and fetch tools with or without the skill. The skill
  just pre-loads the URL map, workflow, and conventions so Claude doesn't
  have to figure them out from scratch.

## Updating the skill

When the repo structure changes (new sections added, files renamed), update
the `SKILL.md` file's URL mapping table and "Current repo state" section to
keep the audit accurate. The skill should also be updated after each major
content push to reflect the new version baseline.

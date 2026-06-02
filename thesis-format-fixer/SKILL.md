---
name: thesis-format-fixer
description: Audit and normalize formatting in university thesis and dissertation DOCX files. Use when a user asks to check, standardize, repair, or adjust graduation thesis formatting; compare a thesis against university layout requirements; create a formatted DOCX copy; or produce a thesis-format review checklist. Support Word .docx files only. Preserve the source file and treat content rewriting, citation correctness, and university-specific interpretation as manual-review tasks.
---

# Thesis Format Fixer

Audit first, explain detected risks, then create a separate formatted copy only when the user requests changes. Never overwrite the source thesis.

## Workflow

1. Locate the source `.docx` and any official university specification or template supplied by the user.
2. If the user supplies an official `.docx` template, run `scripts/extract_template_rules.py` to create a university-specific rules profile. Prefer this profile over generic rules.
3. Use `references/default-rules.json` only when no official template or university-specific rules file exists.
4. If the user supplies written formatting requirements, adjust a separate JSON rules file by following `references/rules-schema.md`. Keep ambiguous rules in the manual-review checklist.
5. Run an audit and inspect the Markdown report.
6. Summarize automatic findings and limits. Treat table-of-contents style changes carefully because Word may modify generated TOC styles when fields are updated. Ask for confirmation before applying changes if the user requested inspection only.
7. Run the fixer with a new output path when formatting changes are requested.
8. Tell the user to open the formatted copy in Word, update fields such as the table of contents, and complete the manual-review checklist.

## Commands

Audit with default rules:

```bash
python3 scripts/thesis_format_fixer.py audit thesis.docx \
  --report outputs/thesis-format-report.md
```

Extract a rules profile from an official university template:

```bash
python3 scripts/extract_template_rules.py university-template.docx \
  --output outputs/university-rules.json
```

Create a formatted copy with default rules:

```bash
python3 scripts/thesis_format_fixer.py fix thesis.docx \
  --output outputs/thesis-formatted.docx \
  --report outputs/thesis-format-report.md
```

Use university-specific rules:

```bash
python3 scripts/thesis_format_fixer.py fix thesis.docx \
  --rules path/to/university-rules.json \
  --output outputs/thesis-formatted.docx \
  --report outputs/thesis-format-report.md
```

Resolve script and reference paths relative to this Skill folder when invoking the commands from another working directory.

## Safety Rules

- Preserve the user's original DOCX. Refuse to use the same path for input and output.
- Prefer audit mode when requirements are incomplete or ambiguous.
- Create a university-specific rules copy instead of modifying `references/default-rules.json`.
- State clearly that the default rules are a generic starting point, not an official university standard.
- Prefer a rules profile extracted from an official DOCX template when one is available.
- Treat generated table-of-contents style differences as reviewable findings, not proof of an error.
- Keep content edits out of scope. Do not rewrite arguments, fabricate citations, or silently alter thesis text.
- Treat cover pages, declarations, page numbering, generated fields, citations, equations, appendices, and unusual layouts as manual-review items.

## Scope

The zero-dependency formatter operates directly on DOCX OOXML. It can extract a reusable rules profile from an official template, normalize existing paragraph style definitions and per-section layouts, detect heading-level jumps, detect automatic table-of-contents fields, summarize style usage, and surface manual-review items for tables and figures.

It does not create missing Word styles, remove direct formatting applied to individual text runs, verify bibliography semantics, or guarantee compliance with an institution's unpublished rules.

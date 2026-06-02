# Thesis Format Fixer

`thesis-format-fixer` is an open-source Codex Skill for auditing and normalizing common formatting rules in university thesis and dissertation `.docx` files.

It is designed for a practical, narrow job: help students spend less time repeatedly adjusting Word formatting while keeping the original thesis intact.

## What It Does

- Audits thesis structure and style usage.
- Detects heading-level jumps and missing automatic table-of-contents fields.
- Extracts reusable rules from an official university `.docx` template.
- Normalizes existing Word paragraph styles and per-section layouts.
- Creates a separate formatted `.docx` copy.
- Produces a Markdown report with a manual-review checklist.
- Runs with Python 3 and no third-party packages.

## Quick Start

Audit a thesis:

```bash
python3 thesis-format-fixer/scripts/thesis_format_fixer.py audit thesis.docx \
  --report thesis-format-report.md
```

Create a formatted copy:

```bash
python3 thesis-format-fixer/scripts/thesis_format_fixer.py fix thesis.docx \
  --output thesis-formatted.docx \
  --report thesis-format-report.md
```

## University-Specific Rules

When your university provides an official `.docx` template, extract a rules profile:

```bash
python3 thesis-format-fixer/scripts/extract_template_rules.py university-template.docx \
  --output university-rules.json
```

Then use the profile:

```bash
python3 thesis-format-fixer/scripts/thesis_format_fixer.py audit thesis.docx \
  --rules university-rules.json \
  --report thesis-format-report.md
```

The bundled rules are intentionally generic. When no official DOCX template exists, copy `thesis-format-fixer/references/default-rules.json`, adjust the copy against your university's written specification, and pass it with `--rules`.

See `thesis-format-fixer/references/rules-schema.md` for configuration details.

## Important Limits

This project is a formatting assistant, not a guarantee of institutional compliance. Always review the generated file in Microsoft Word, update generated fields such as the table of contents, and compare the result with your university's official requirements.

The tool does not write thesis content or fabricate citations.

## Local Smoke Test

```bash
python3 thesis-format-fixer/scripts/create_sample_docx.py work/sample-thesis.docx
python3 thesis-format-fixer/scripts/thesis_format_fixer.py audit work/sample-thesis.docx
python3 thesis-format-fixer/scripts/thesis_format_fixer.py fix work/sample-thesis.docx \
  --output work/sample-thesis-formatted.docx \
  --report work/sample-thesis-report.md
```

## License

MIT

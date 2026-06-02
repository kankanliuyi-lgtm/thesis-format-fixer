# Rules Configuration

Use `default-rules.json` as a starting point when only written requirements are available. When the university provides an official DOCX template, prefer:

```bash
python3 scripts/extract_template_rules.py university-template.docx \
  --output university-rules.json
```

Review the extracted profile and keep it as a university-specific file instead of editing the default file.

## Document Rules

- `document.require_toc`: Report a warning when the DOCX does not contain an automatic table-of-contents field.
- `document.margins_twips`: Set margins on every document section. Word measures margins in twips: `1440` twips equals one inch.
- `document.sections`: Normalize each section independently. Preserve section count, page size, margins, page-number format, page-number restart, and title-page setting. Prefer this over `margins_twips` for official templates.

## Style Rules

Each key under `styles` must match an existing Word style ID such as `Normal`, `Heading1`, or `Caption`.
Official templates may use numeric IDs such as `1`, `84`, or `89`; use the extracted IDs without renaming them.

- `font_ascii`: Set Latin-character font.
- `font_east_asia`: Set East Asian font.
- `size_half_points`: Set font size in half-points. For example, `21` means `10.5 pt`.
- `alignment`: Use a WordprocessingML value such as `left`, `center`, or `both`.
- `line`, `before`, `after`, `first_line`: Set paragraph spacing or indentation in twips.
- `line_rule`: Use a WordprocessingML value such as `auto` or `exact`.
- `bold`: Add or remove bold formatting at style level.
- `style_name`: Preserve the human-readable style name for reports.
- `heading_level`: Identify heading hierarchy when the template uses nonstandard style IDs.
- `spacing`, `indent`, `fonts`: Store complete OOXML attribute maps extracted from an official template.

## Limits

The formatter normalizes style definitions and section layouts. It does not infer university-specific semantics, rewrite thesis content, remove direct formatting, copy headers or footers between unrelated files, or guarantee that Word fields render identically across office suites. Keep manual review items explicit. Word may change generated table-of-contents styles when TOC fields are updated; review those differences before applying fixes.

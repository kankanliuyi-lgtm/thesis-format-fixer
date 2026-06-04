# 规则配置说明

当只有文字版格式要求时，可以从 `default-rules.json` 开始改。

如果学校提供了官方 `.docx` 模板，优先使用模板提取规则：

```bash
python3 scripts/extract_template_rules.py university-template.docx \
  --output university-rules.json
```

提取后建议人工快速检查一遍 `university-rules.json`，再把它作为学校专属规则文件使用。不要直接修改默认规则文件。

## 文档规则

- `document.require_toc`：如果 DOCX 中没有自动目录字段，报告警告。
- `document.margins_twips`：给所有分节设置统一页边距。Word 使用 twip 作为单位，`1440` twips 等于 1 英寸。
- `document.sections`：逐个分节规范页面设置。可保留分节数量、纸张大小、页边距、页码格式、页码重启和首页不同设置。使用官方模板时，优先使用这个字段，而不是统一的 `margins_twips`。

## 样式规则

`styles` 下的每个 key 必须对应 Word 文档中真实存在的样式 ID，例如 `Normal`、`Heading1`、`Caption`。

注意：很多学校模板不会使用英文样式 ID，而是使用 `1`、`84`、`89` 这类数字 ID。使用模板提取出的 ID 即可，不要手动改名。

- `font_ascii`：设置西文字体。
- `font_east_asia`：设置东亚文字体。
- `size_half_points`：字号，单位是半磅。例如 `21` 表示 `10.5 pt`。
- `alignment`：段落对齐方式，例如 `left`、`center`、`both`。
- `line`、`before`、`after`、`first_line`：段前段后、行距或首行缩进，单位是 twip。
- `line_rule`：行距规则，例如 `auto` 或 `exact`。
- `bold`：是否加粗。
- `style_name`：样式的人类可读名称，用于报告展示。
- `heading_level`：标题层级。模板使用非标准样式 ID 时，用它识别标题结构。
- `spacing`、`indent`、`fonts`：从官方模板提取出的完整 OOXML 属性映射。

## 处理边界

formatter 会规范样式定义和分节布局，但不会：

- 推断学校没有明确说明的格式语义。
- 改写论文内容。
- 移除每个文字片段上的直接格式。
- 在无关文件之间复制页眉页脚。
- 保证不同 Office 软件渲染完全一致。

Word 更新目录时，可能会自动改动 `toc 1`、`toc 2`、`toc 3` 等目录样式。遇到这类差异，应先人工判断是否确实需要修复。

# Thesis Format Fixer

一个用于检查和修复毕业论文 `.docx` 格式的 Codex Skill。

English: A Codex Skill for auditing and normalizing thesis DOCX formatting.

它解决的是一个很具体、但很折磨人的问题：论文内容已经写得差不多了，却还要反复调整标题、正文、目录、页码、页边距、图表题注和学校模板格式。

## 它能做什么

- 审计论文结构和 Word 样式使用情况。
- 检测标题层级跳跃、缺少自动目录等常见问题。
- 从学校官方 `.docx` 模板中提取可复用的格式规则。
- 按规则规范现有段落样式和分节布局。
- 生成新的格式化 `.docx` 副本，不覆盖原文件。
- 输出 Markdown 格式检查报告和人工复核清单。
- 使用 Python 3 标准库运行，不依赖第三方包。

## 安装到 Agent

这个仓库的根目录是开源项目，真正的 Skill 文件夹是里面的 `thesis-format-fixer/`。

安装后目录结构应该长这样：

```text
skills/
└── thesis-format-fixer/
    ├── SKILL.md
    ├── scripts/
    └── references/
```

### 安装到 Codex

```bash
git clone https://github.com/kankanliuyi-lgtm/thesis-format-fixer.git
mkdir -p "${CODEX_HOME:-$HOME/.codex}/skills/thesis-format-fixer"
cp -R thesis-format-fixer/thesis-format-fixer/. "${CODEX_HOME:-$HOME/.codex}/skills/thesis-format-fixer/"
```

重启 Codex，或开启一个新会话后即可使用。你可以这样触发：

```text
Use $thesis-format-fixer to audit my thesis DOCX and create a formatted copy with a review report.
```

中文也可以：

```text
用 thesis-format-fixer 检查我的毕业论文格式，并根据学校模板生成修复副本。
```

### 安装到 Claude Code

个人全局安装：

```bash
git clone https://github.com/kankanliuyi-lgtm/thesis-format-fixer.git
mkdir -p "$HOME/.claude/skills/thesis-format-fixer"
cp -R thesis-format-fixer/thesis-format-fixer/. "$HOME/.claude/skills/thesis-format-fixer/"
```

只安装到当前项目：

```bash
git clone https://github.com/kankanliuyi-lgtm/thesis-format-fixer.git
mkdir -p .claude/skills/thesis-format-fixer
cp -R thesis-format-fixer/thesis-format-fixer/. .claude/skills/thesis-format-fixer/
```

然后在 Claude Code 中请求：

```text
Use $thesis-format-fixer to check and normalize this thesis DOCX.
```

### 安装到其他支持 Skill 的 Agent

如果你的 Agent 支持 `SKILL.md` 约定，把仓库里的 `thesis-format-fixer/` 文件夹复制到该 Agent 的 skills 目录即可。关键是确保路径中直接包含：

```text
thesis-format-fixer/SKILL.md
```

而不是：

```text
thesis-format-fixer/thesis-format-fixer/SKILL.md
```

## 快速开始

审计一篇论文：

```bash
python3 thesis-format-fixer/scripts/thesis_format_fixer.py audit thesis.docx \
  --report thesis-format-report.md
```

生成格式化副本：

```bash
python3 thesis-format-fixer/scripts/thesis_format_fixer.py fix thesis.docx \
  --output thesis-formatted.docx \
  --report thesis-format-report.md
```

## 根据学校模板生成规则

如果学校提供了官方 `.docx` 论文模板，推荐先从模板提取规则：

```bash
python3 thesis-format-fixer/scripts/extract_template_rules.py university-template.docx \
  --output university-rules.json
```

然后用这份规则审计论文：

```bash
python3 thesis-format-fixer/scripts/thesis_format_fixer.py audit thesis.docx \
  --rules university-rules.json \
  --report thesis-format-report.md
```

也可以直接生成格式化副本：

```bash
python3 thesis-format-fixer/scripts/thesis_format_fixer.py fix thesis.docx \
  --rules university-rules.json \
  --output thesis-formatted.docx \
  --report thesis-format-report.md
```

内置的 `thesis-format-fixer/references/default-rules.json` 只是通用起点，不代表任何学校的官方规范。没有官方模板时，可以复制它，再根据学校发布的文字版格式要求调整。

规则配置说明见 `thesis-format-fixer/references/rules-schema.md`。

## 重要限制

这个项目是格式助手，不是学校格式合规保证器。生成文件后，仍然需要用 Microsoft Word 打开检查，并更新目录等自动字段。

它不会：

- 代写或改写论文内容。
- 编造引用或参考文献。
- 自动判断学校未明确说明的格式语义。
- 保证不同 Office 软件渲染完全一致。

强烈建议人工复核：

- 封面、声明页、中英文摘要、关键词。
- 页眉、页脚、页码格式和分节符。
- 自动目录、图表题注、公式、脚注、附录。
- 引用和参考文献格式。

## 本地烟雾测试

```bash
python3 thesis-format-fixer/scripts/create_sample_docx.py work/sample-thesis.docx
python3 thesis-format-fixer/scripts/thesis_format_fixer.py audit work/sample-thesis.docx
python3 thesis-format-fixer/scripts/thesis_format_fixer.py fix work/sample-thesis.docx \
  --output work/sample-thesis-formatted.docx \
  --report work/sample-thesis-report.md
```

## 适合谁

- 正在改毕业论文格式的本科生、研究生。
- 想把学校论文模板转成可复用规则的同学。
- 想做论文格式检查自动化的 AI 工具玩家。
- 想给 Codex 增加论文排版能力的 Skill 作者。

## License

MIT

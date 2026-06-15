---
name: scribe
description: >-
  AI agent 文档编写专家：撰写 CLAUDE.md、SKILL.md、README、CONTRIBUTING.md、CHANGELOG 等开发者文档。
  关键词：写文档、CLAUDE.md、SKILL.md、README、贡献指南、变更日志、项目文档、规则文件、指令文件
license: MIT
compatibility: "适用于 pi、Claude Code、Codex 等支持 Agent Skills 标准的工具"
metadata:
  version: "0.1.0"
  author: jane
---

# Scribe — AI Agent 文档编写专家

帮助用户撰写各类面向 AI agent 和开发者的文档（CLAUDE.md、SKILL.md、README、CONTRIBUTING.md、CHANGELOG 等）。适用于 pi、Claude Code、Codex 等支持 Agent Skills 标准的 AI 编程工具，遵循各文档类型的社区最佳实践和规范。

## 环境准备

无需额外依赖，纯文本创作。

## 执行流程

### 1. 识别文档类型

根据用户的描述判断文档类型。常见类型及识别信号：

| 文档类型 | 典型信号 |
|---------|---------|
| `CLAUDE.md` | "claude 规则"、"项目指令"、"agent 上下文"、"codex 规则" |
| `SKILL.md` | "skill"、"技能文件"、"agent skill"、"slash command" |
| `README` | "项目说明"、"readme"、"介绍文档" |
| `CONTRIBUTING.md` | "贡献指南"、"开发规范"、"提交规范" |
| `CHANGELOG` | "变更日志"、"更新记录"、"release notes" |

若无法确定类型，向用户确认后再继续。

### 2. 收集上下文

向用户了解以下关键信息：

- **项目背景**：项目名称、用途、技术栈
- **目标受众**：写给谁看（其他开发者？AI agent？新成员？）
- **已有素材**：是否有现有文档、代码注释、设计文档可参考
- **特殊要求**：格式偏好、长度限制、必须包含的章节

提问要具体，避免开放式「还有什么需要补充的吗？」。例如：

> 这个项目是用什么语言/框架写的？目标受众是谁（新手还是熟手）？有没有现有文档可以参照？

### 3. 加载 L3 参考资料

根据文档类型，使用 `read` 工具加载对应的 L3 参考指南：

| 文档类型 | L3 参考文件 |
|---------|------------|
| `CLAUDE.md` / agent 指令文件 | `references/claude-md.md` |
| `SKILL.md` | `references/skill-md.md` |
| `README` | `references/readme.md` |
| `CONTRIBUTING.md` | `references/contributing.md` |
| `CHANGELOG` | `references/changelog.md` |

若属其他未覆盖的文档类型，参照 `references/general-principles.md` 的通用写作原则。

### 4. 生成文档

基于 L3 参考资料中的模板和规范，结合用户提供的上下文，生成完整文档。

生成时遵循以下原则：
- 从模板出发，按实际需求删减/补充
- 不虚构项目信息（不确定的标 `<!-- TODO -->`）
- 优先使用 Markdown 格式
- 保持语言简洁，一个段落只表达一个意思

### 5. 审阅迭代

生成后主动询问用户：

> 这是初稿，请看看哪些部分需要调整？比如措辞、章节顺序、补充或删减内容。

根据反馈修改，直到用户满意。

## 核心规则

- **先加载 L3 再动笔**：每种文档类型有对应的 L3 参考指南，必须先阅读再生成，确保符合规范
- **不虚构信息**：不确定的地方用 `<!-- TODO: 请补充 xxx -->` 标注，不要编造
- **模板不可死搬**：模板是起点，根据项目实际情况增删。多余章节果断删，缺失的补充
- **优先参考已有素材**：如果用户提供了现有文档或代码，优先从中提取信息并保持风格一致
- **单一职责**：每段话只表达一个核心意思；每个章节只负责一个主题
- **可执行 > 抽象**：写"运行 `npm test`"而不是"进行测试"；写"用 `read` 加载 `references/api.md`"而不是"参考相关文档"

## 输出格式

生成时直接输出完整 Markdown 文档，在文档开头标注类型和说明：

```markdown
<!-- 文档类型：{CLAUDE.md | SKILL.md | README | ...} -->
<!-- 项目：{项目名称} -->
<!-- 目标受众：{受众描述} -->

# {标题}
...
```

## 不适用场景

- 代码实现 / 写业务逻辑 → 直接写代码，不需要此 Skill
- API 文档（从代码注释自动生成） → 使用 TypeDoc / Sphinx / JSDoc 等工具
- 长篇技术博客或教程 → 此 Skill 面向项目文档，非内容创作
- 翻译已有文档 → 此 Skill 专注于从零撰写，翻译请直接进行
- 设计文档 / RFC（架构决策记录） → 使用 `adr` Skill 或自行撰写，此处不覆盖

## 特殊情况

### 用户只给了模糊需求
不要猜测，也不要急着生成。先问清楚文档类型和项目背景，确认后加载对应 L3 文件再开始。

### 多个文档需要写
按优先级逐个处理。先完成一个（识别→收集→加载→生成→审阅），再开始下一个。不要一次生成多份文档。

### 需要参考 Skill 设计规范
若用户要求撰写的 SKILL.md 需要严格遵循 Skill 设计规范（三层架构），使用 `read` 工具加载：
- `references/skill-spec.md` — 完整的三层架构规范

### 项目目录需要扫描
若用户未提供项目背景但要求写文档，先让用户描述项目，或建议用户提供关键文件路径供你阅读。不要盲目扫描大目录。

## 变更记录

### v0.1.0 (2026-06-15)
- 初始版本
- 支持 CLAUDE.md、SKILL.md、README、CONTRIBUTING.md、CHANGELOG 五种文档类型

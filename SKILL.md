---
name: scribe
description: >-
  AI agent 文档编写专家：撰写、修改、规范、重构 CLAUDE.md、SKILL.md、README、CONTRIBUTING.md、CHANGELOG 等开发者文档。
  关键词：写文档、改文档、规范文档、重构文档、CLAUDE.md、SKILL.md、README、贡献指南、变更日志、指令文件
license: MIT
---

# Scribe — AI Agent 文档编写专家

帮助用户撰写、修改、规范化、重构各类面向 AI agent 和开发者的文档（CLAUDE.md、SKILL.md、README、CONTRIBUTING.md、CHANGELOG 等）。遵循各文档类型的社区最佳实践和规范，兼容任何支持 Agent Skills 标准的 AI 编程工具。

## 执行流程

### 1. 判断工作模式

根据用户意图确定工作模式：

| 模式 | 典型信号 |
|------|---------|
| **新建**（单文档） | "写个"、"创建"、"帮我生成"、"还没有文档" |
| **修改/规范** | "改一下"、"优化"、"规范一下"、"重构"、"review 一下"、"看看哪里有问题"、"这里不对"、"错了"、"帮我看看这个文档" |
| **项目级编排**（多文档） | "我新建了个项目，帮我定规范"、"初始化项目文档"、"项目要哪些文档"、"看看我项目缺哪些文档"、"补齐文档" |

**跳转规则：**
- **修改模式** → 跳转到下方「修改文档流程」section，不要走新建流程。
- **项目级编排模式** → 跳转到下方「项目级文档编排流程」section，加载 `references/project-setup.md`。
- **新建模式** → 继续往下走。

---

## 新建文档流程

### 2. 识别文档类型和 agent 环境

根据用户的描述判断文档类型。常见类型及识别信号：

| 文档类型 | 典型信号 |
|---------|---------|
| `CLAUDE.md` / `AGENTS.md` | "claude 规则"、"项目指令"、"agent 上下文"、"agents.md"、"codex 规则"、"agent 指令文件" |
| `SKILL.md` | "skill"、"技能文件"、"agent skill"、"slash command" |
| `README` | "项目说明"、"readme"、"介绍文档" |
| `CONTRIBUTING.md` | "贡献指南"、"开发规范"、"提交规范" |
| `CHANGELOG` | "变更日志"、"更新记录"、"release notes" |
| `PROGRESS.md` | "项目进度"、"进度文档"、"待办清单"、"roadmap"、"路线图"、"todo 列表"、"分阶段计划" |
| API 文档 | "接口文档"、"REST API"、"API 参考"、"OpenAPI"、"swagger"、"endpoint 文档" |
| 设计/架构文档 | "设计文档"、"架构文档"、"系统设计"、"技术方案"、"RFC"、"提案" |
| ADR | "架构决策记录"、"adr"、"决策记录"、"技术选型记录" |
| `SECURITY.md` | "安全策略"、"漏洞上报"、"security.md"、"安全披露" |
| 部署/运维文档 | "部署文档"、"运维文档"、"上线步骤"、"配置说明"、"deployment" |
| FAQ / 故障排查 | "faq"、"常见问题"、"故障排查"、"troubleshooting"、"排错指南" |
| 治理小文档 | "issue 模板"、"pr 模板"、"行为准则"、"code of conduct"、".github 模板" |
| 提交信息规范 | "提交规范"、"commit message"、"commit 规范"、"conventional commits"、"提交信息格式" |

若无法确定类型，或用户在两类文档之间犹豫（该写 README 还是设计文档？记进 CHANGELOG 还是 PROGRESS？），加载 `references/doc-map.md`（文档职责总览）辅助选型，再向用户确认。

**当文档类型是 SKILL.md 时**，额外确认目标 agent：

> 这个 skill 是给哪个 agent 用的？pi、Claude Code、还是 Codex？如果是多个，我会按通用规范写。

| 目标 agent | 加载文件 |
|-----------|---------|
| 不确定 / 多 agent | 仅 `skill-spec.md`（通用规范） |
| pi | `skill-spec.md` + `skill-pi.md` |
| Claude Code | `skill-spec.md` + `skill-claude.md` |

**核心原则**：不确定 agent 时，绝不添加任何 agent 特有的字段或约定。只输出 name + description + 通用 L2 内容。

### 3. 收集上下文

向用户了解以下关键信息：

- **项目背景**：项目名称、用途、技术栈
- **目标受众**：写给谁看（其他开发者？AI agent？新成员？）
- **已有素材**：是否有现有文档、代码注释、设计文档可参考
- **特殊要求**：格式偏好、长度限制、必须包含的章节

提问要具体，避免开放式「还有什么需要补充的吗？」。例如：

> 这个项目是用什么语言/框架写的？目标受众是谁（新手还是熟手）？有没有现有文档可以参照？

### 4. 加载 L3 参考资料

根据文档类型，加载对应的 L3 参考指南（所有文件相对于本 skill 的 `references/` 目录）：

| 文档类型 | L3 参考文件 |
|---------|------------|
| `CLAUDE.md` / `AGENTS.md` / agent 指令文件 | `references/agent-instructions.md` |
| `SKILL.md`（通用） | `references/skill-spec.md` |
| `SKILL.md`（pi） | `references/skill-spec.md` + `references/skill-pi.md` |
| `SKILL.md`（Claude Code） | `references/skill-spec.md` + `references/skill-claude.md` |
| `SKILL.md`（多 agent） | `references/skill-spec.md` + `references/skill-agents.md` |
| `README` | `references/readme.md` |
| `CONTRIBUTING.md` | `references/contributing.md` |
| `CHANGELOG` | `references/changelog.md` |
| `PROGRESS.md` / `ROADMAP.md` / `TODO.md` | `references/progress.md` |
| API 文档（REST / SDK / OpenAPI） | `references/api-doc.md` |
| 设计 / 架构文档 / RFC | `references/design-doc.md` |
| ADR（架构决策记录） | `references/adr.md` |
| `SECURITY.md` | `references/security.md` |
| 部署 / 运维文档 | `references/deployment.md` |
| FAQ / 故障排查 | `references/faq.md` |
| 治理小文档（Issue/PR 模板、CODE_OF_CONDUCT） | `references/governance.md` |
| 提交信息规范（Commit Message / Conventional Commits） | `references/commit-message.md` |
| **修改现有文档** | `references/modify-doc.md` |
| **项目级编排（初始化 / 补齐多文档）** | `references/project-setup.md` |
| **文档选型 / 职责边界不清** | `references/doc-map.md` |
| 其他未覆盖的文档类型 | `references/general-principles.md` |

### 5. 生成文档

基于 L3 参考资料中的模板和规范，结合用户提供的上下文，生成完整文档。

生成时遵循以下原则：
- 从模板出发，按实际需求删减/补充
- 不虚构项目信息（不确定的标 `<!-- TODO -->`）
- 优先使用 Markdown 格式
- 保持语言简洁，一个段落只表达一个意思

### 6. 审阅迭代

生成后主动询问用户：

> 这是初稿，请看看哪些部分需要调整？比如措辞、章节顺序、补充或删减内容。

根据反馈修改，直到用户满意。

---

## 修改文档流程

> 详细操作指南在 L3 中，按需加载。

当用户要求修改、规范、重构、优化现有文档时，加载 `references/modify-doc.md`。

该文件包含：
- 修改前的文档类型 + agent 环境识别（与新建流程同样的确认逻辑）
- 四种修改模式识别（局部修改 / 规范化 / 重构 / 审查）
- 完整操作步骤（读取→加载规范→诊断→确认→执行→审阅）
- 修改场景核心规则（先诊后治、最小改动、每改必解释、不推断意图）

---

## 项目级文档编排流程

> 详细操作指南在 L3 中，按需加载。

当用户的需求是**整个项目层面**的多文档处理时（初始化一套项目文档、检查并补齐缺失文档），加载 `references/project-setup.md`。

该文件包含：
- 两个场景（初始化 / 补齐）共用的统一流程：定项目类型 → 扫描根目录现状 → 算出推荐集与缺口 → **列清单交用户确认** → 逐个生成
- 推荐集模型：通用基础集（README / AGENTS.md / 提交规范 / CHANGELOG / PROGRESS）+ 按项目类型的附加项
- 核心约束：**不自作主张全建，由用户勾选补哪些**；只扫顶层文档不递归遍历；每份文档的写法仍走对应单类 L3

---

## 核心规则

- **先加载 L3 再动笔**：每种文档类型有对应的 L3 参考指南，必须先阅读再操作
- **不虚构信息**：不确定的地方用 `<!-- TODO: 请补充 xxx -->` 标注，不要编造
- **模板不可死搬**：模板是起点，根据项目实际情况增删。多余章节果断删，缺失的补充
- **优先参考已有素材**：如果用户提供了现有文档或代码，优先从中提取信息并保持风格一致
- **单一职责**：每段话只表达一个核心意思；每个章节只负责一个主题
- **可执行 > 抽象**：写"运行 `npm test`"而不是"进行测试"
- **修改文档时，先加载 `references/modify-doc.md`**：修改场景有独立的规则和步骤，不要用新建流程去改文档

## 输出格式

新建模式直接输出完整 Markdown 文档，在文档开头标注类型和说明：

```markdown
<!-- 文档类型：{CLAUDE.md | SKILL.md | README | ...} -->
<!-- 项目：{项目名称} -->
<!-- 目标受众：{受众描述} -->

# {标题}
...
```

修改模式先输出诊断清单，用户确认后再输出修改后的文档。详见 `references/modify-doc.md`。

## 不适用场景

- 代码实现 / 写业务逻辑 → 直接写代码，不需要此 Skill
- 从代码注释**全量自动生成** API 文档 → 使用 TypeDoc / Sphinx / JSDoc 等工具（手写 REST 接口文档 / SDK 参考 / OpenAPI 规范由本 Skill 覆盖，见 `references/api-doc.md`）
- 长篇技术博客或教程 → 此 Skill 面向项目文档，非内容创作

## 特殊情况

### 用户只给了模糊需求
不要猜测，也不要急着生成。先问清楚文档类型和项目背景，确认后加载对应 L3 文件再开始。

### 多个文档需要写/改
按优先级逐个处理。先完成一个，再开始下一个。不要一次生成多份文档。

### 需要参考 Skill 设计规范
若用户要求撰写的 SKILL.md 需要严格遵循规范，根据用户使用的 agent 加载对应文件：

| 用户确认的 agent | 加载文件 |
|----------------|---------|
| 不确定 | `references/skill-spec.md`（只使用通用规范） |
| pi | `references/skill-spec.md` + `references/skill-pi.md` |
| Claude Code | `references/skill-spec.md` + `references/skill-claude.md` |
| 多 agent 兼容 | `references/skill-spec.md` + `references/skill-agents.md` |

### 项目目录需要扫描
若用户未提供项目背景但要求写**单份**文档，先让用户描述项目，或建议用户提供关键文件路径供你阅读。不要盲目递归遍历大代码库。

> 例外：**项目级编排流程**会扫描项目根目录的顶层文档（看缺哪些 README/AGENTS.md 等），这是轻量、有界的检查，属于该流程的正常步骤，不在此限。

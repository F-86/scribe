---
name: scribe
description: >-
  AI agent 文档编写专家：撰写、修改、规范、重构、补齐各类开发者文档。
  关键词：写文档、改文档、规范文档、初始化项目文档、补齐文档、缺哪些文档、CLAUDE.md、AGENTS.md、SKILL.md、README、贡献指南、变更日志、项目进度、API 文档、设计文档、ADR、提交规范、测试文档、测试用例
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
- **新建模式** → 先做下方「新建前的存在性检查」，再继续。

> **新建前的存在性检查（必做）**：用户说"写个/创建 X"不代表 X 不存在。动笔前先确认目标文件是否已存在（如 `README.md`、`CHANGELOG.md` 等固定文件名的文档）。**若已存在，不要直接覆盖**——告知用户并询问：是在现有基础上修改/补充（转入修改流程），还是确实要重写覆盖？由用户决定。覆盖现有文档是不可逆操作，绝不擅自进行。

---

## 新建文档流程

### 2. 识别文档类型和 agent 环境

根据用户的描述判断文档类型。下表同时给出**识别信号**与**对应的 L3 参考文件**（第 4 步据此加载，所有文件在本 skill 的 `references/` 目录下）：

| 文档类型 | 典型信号 | L3 参考文件 |
|---------|---------|------------|
| `CLAUDE.md` / `AGENTS.md` | "claude 规则"、"项目指令"、"agent 上下文"、"agents.md"、"codex 规则"、"agent 指令文件" | `agent-instructions.md` |
| `SKILL.md` | "skill"、"技能文件"、"agent skill"、"slash command" | `skill-spec.md`（agent 变体见下方 agent 表追加） |
| `README` | "项目说明"、"readme"、"介绍文档" | `readme.md` |
| `CONTRIBUTING.md` | "贡献指南"、"开发规范"、"怎么参与贡献"、"PR 流程" | `contributing.md` |
| `CHANGELOG` | "变更日志"、"更新记录"、"release notes" | `changelog.md` |
| `PROGRESS.md` | "项目进度"、"进度文档"、"待办清单"、"roadmap"、"路线图"、"todo 列表"、"分阶段计划" | `progress.md` |
| API 文档 | "接口文档"、"REST API"、"API 参考"、"OpenAPI"、"swagger"、"endpoint 文档" | `api-doc.md` |
| 设计/架构文档 | "设计文档"、"架构文档"、"系统设计"、"技术方案"、"RFC"、"提案" | `design-doc.md` |
| ADR | "架构决策记录"、"adr"、"决策记录"、"技术选型记录" | `adr.md` |
| `SECURITY.md` | "安全策略"、"漏洞上报"、"security.md"、"安全披露" | `security.md` |
| 部署/运维文档 | "部署文档"、"运维文档"、"上线步骤"、"配置说明"、"deployment" | `deployment.md` |
| FAQ / 故障排查 | "faq"、"常见问题"、"故障排查"、"troubleshooting"、"排错指南" | `faq.md` |
| 治理小文档 | "issue 模板"、"pr 模板"、"行为准则"、"code of conduct"、".github 模板" | `governance.md` |
| 提交信息规范 | "提交规范"、"commit message"、"commit 规范"、"conventional commits"、"提交信息格式" | `commit-message.md` |
| 测试文档 | "测试用例"、"测试文档"、"测试计划"、"测试策略"、"test case"、"怎么写测试"、"测试规范" | `test-doc.md` |

若无法确定类型，或用户在两类文档之间犹豫（该写 README 还是设计文档？记进 CHANGELOG 还是 PROGRESS？），加载 `references/doc-map.md`（文档职责总览）辅助选型，再向用户确认。

**当文档类型是 SKILL.md 时**，额外确认目标 agent，按下表追加文件：

> 这个 skill 是给哪个 agent 用的？pi、Claude Code、还是 Codex？如果是多个，我会按通用规范写。

| 目标 agent | 加载文件 |
|-----------|---------|
| 不确定 / 多 agent | `skill-spec.md`（通用规范）；多 agent 兼容可加 `skill-agents.md`（差异对照） |
| pi | `skill-spec.md` + `skill-pi.md` |
| Claude Code | `skill-spec.md` + `skill-claude.md` |

**核心原则**：不确定 agent 时，绝不添加任何 agent 特有的字段或约定。只输出 name + description + 通用 L2 内容。

### 3. 收集上下文

向用户了解以下关键信息：

- **项目背景**：项目名称、用途、技术栈
- **目标受众**：写给谁看（其他开发者？AI agent？新成员？）
- **项目背景**：项目名称、用途、技术栈
- **目标受众**：写给谁看（其他开发者？AI agent？新成员？）
- **已有素材**：是否有现有文档、代码注释、设计文档可参考；若文档要封装项目已有能力，参照 `references/new-doc-capability-check.md`
- **特殊要求**：格式偏好、长度限制、必须包含的章节

提问要具体，避免开放式「还有什么需要补充的吗？」。例如：

> 这个项目是用什么语言/框架写的？目标受众是谁（新手还是熟手）？有没有现有文档可以参照？

### 4. 加载 L3 参考资料

根据第 2 步识别表的「L3 参考文件」列，用 `read` 工具加载对应文件。此外这几种**非文档类型**的场景按需加载：

| 场景 | L3 参考文件 |
|------|------------|
| **修改现有文档** | `references/modify-doc.md` |
| **项目级编排（初始化 / 补齐多文档）** | `references/project-setup.md` |
| **文档选型 / 职责边界不清** | `references/doc-map.md` |
| 其他未覆盖的文档类型 | `references/general-principles.md` |
| **新建文档时封装项目已有能力** | `references/new-doc-capability-check.md` |

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
- **不覆盖未确认的现有文档**：新建前先查目标文件是否已存在；已存在则先问用户"改还是重写"，绝不擅自覆盖（不可逆）
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

### 项目目录需要扫描
若用户未提供项目背景但要求写**单份**文档，先让用户描述项目，或建议用户提供关键文件路径供你阅读。不要盲目递归遍历大代码库。

> 例外：**项目级编排流程**会扫描项目根目录的顶层文档（看缺哪些 README/AGENTS.md 等），这是轻量、有界的检查，属于该流程的正常步骤，不在此限。

## references/ 目录索引

| 文件 | 用途 |
|------|------|
| `references/agent-instructions.md` | CLAUDE.md / AGENTS.md 规范 |
| `references/skill-spec.md` | SKILL.md 通用规范 |
| `references/readme.md` | README 规范 |
| `references/contributing.md` | 贡献指南规范 |
| `references/changelog.md` | CHANGELOG 规范 |
| `references/progress.md` | PROGRESS.md 进度文档规范 |
| `references/api-doc.md` | API 文档规范 |
| `references/design-doc.md` | 设计/架构文档规范 |
| `references/adr.md` | ADR 架构决策记录规范 |
| `references/security.md` | SECURITY.md 规范 |
| `references/deployment.md` | 部署/运维文档规范 |
| `references/faq.md` | FAQ / 故障排查文档规范 |
| `references/governance.md` | 治理小文档规范（Issue/PR 模板、行为准则等） |
| `references/commit-message.md` | 提交信息规范 |
| `references/test-doc.md` | 测试文档规范 |
| `references/doc-map.md` | 文档类型选型/职责边界不清时参考 |
| `references/modify-doc.md` | 修改现有文档的操作指南 |
| `references/project-setup.md` | 项目级多文档编排流程 |
| `references/general-principles.md` | 未覆盖文档类型通用原则 |
| `references/skill-agents.md` | 多 agent 兼容差异对照 |
| `references/skill-pi.md` | pi agent 特有规范 |
| `references/skill-claude.md` | Claude Code 特有规范 |
| `references/new-doc-capability-check.md` | 新建文档时封装项目已有能力的检查流程 |

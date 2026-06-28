---
name: scribe
description: >-
  AI agent 文档编写专家：撰写、修改、规范、重构、补齐各类开发者文档。
  关键词：写文档、改文档、规范文档、初始化项目文档、补齐文档、缺哪些文档、CLAUDE.md、AGENTS.md、SKILL.md、README、贡献指南、变更日志、项目进度、API 文档、设计文档、多文件设计笔记、agent-design-study、ADR、提交规范、测试文档、测试用例
license: MIT
---

# Scribe — AI Agent 文档编写专家

帮助用户撰写、修改、规范化、重构各类面向 AI agent 和开发者的文档（CLAUDE.md、SKILL.md、README、CONTRIBUTING.md、CHANGELOG 等）。遵循各文档类型的社区最佳实践和规范，兼容任何支持 Agent Skills 标准的 AI 编程工具。

## 边界（先判断要不要进这个 Skill）

- 最终主产物是 **项目文档**（CLAUDE.md / SKILL.md / README / CONTRIBUTING / CHANGELOG / PROGRESS / API 文档 / 设计文档 / ADR / SECURITY / 部署/运维 / FAQ / 治理小文档 / 提交规范 / 测试文档）。
- 产物直接写入用户项目的指定路径（如 `README.md`、`docs/api.md`）。
- **不**生成：代码实现 / 业务逻辑、长篇技术博客或教程（非项目文档）、从代码注释全量自动生成 API 文档（用 TypeDoc / Sphinx 等工具）。
- **修改模式不要走新建流程** — 改文档有独立的规则和步骤。

如果用户要的是代码而不是文档，停下来澄清，不要进入本 Skill。

---

## Harness 视角

本 Skill 的重点不是"提示词"，而是一个小型 harness。它要回答六个问题：

| Harness 部分 | 本 Skill 解决的问题 | 设计手段 |
|---|---|---|
| 上下文管理 | 模型到底看到了什么 | 渐进加载 references：Intake 只看识别表，Plan 才加载对应 L3 模板，Review 才读质检清单 |
| 工具系统 | 能处理什么输入/输出 | 用户粘贴 / 文件路径读取 / 项目根目录扫描；输出为项目文档文件 |
| 执行编排 | 下一步该做什么 | 分 Phase、检查点、先写计划再出初稿再审阅修复 |
| 状态与记忆 | 决策如何跨步骤保持 | `.scribe/` 工作区：`plan.md`、`review.md`、`draft.md` |
| 评估与观测 | 怎么知道文档写得好不好 | 内联自查清单 + 可选的 SubAgent 评审（复杂文档） |
| 约束与恢复 | 跑偏后怎么修 | Checkpoint 用户确认制、不覆盖已存在文件、修复不走整篇重写 |

---

## 工作流总览

```
Phase 0  Intake            判断模式 + 目标文件存在性检查
   ▼
Phase 1  Context           收集项目上下文 + 识别文档类型（加载对应识别表）
   ▼
Phase 2  Plan              输出写作计划 plan.md（Brief / Outline / 章节列表）
         ★ Checkpoint 1 — 必须停。用户逐项确认计划
   ▼
Phase 3  Draft             生成文档初稿（新建/修改/项目编排）
         └ 主 Agent 内联 5 条 checklist 自查，按结论修复后进入 Phase 4
   ▼
Phase 4  Review            质检：内联自查 + 可选 SubAgent 评审
         ★ Checkpoint 2 — 必须停。用户确认初稿 / 评审结论
   ▼
Phase 5  Revise            最小切片修复（禁止整篇重写），有修复才写 review.md
   ▼
Phase 6  Deliver           最终确认 → 写入目标路径
         ★ Checkpoint 3 — 确认交付
```

---

## 硬性质检协议

**质检方式按节点区分 — 不是所有质检都要开 SubAgent，也不是所有质检都要写文件。**

| 节点 | 质检方式 | 产物 | 为什么 |
|---|---|---|---|
| **Phase 3 Draft（默认）** | 主 Agent 内联 5 条 checklist | 无文件 | 初稿刚写完，上下文热，自查效率最高 |
| **Phase 4 Review（仅复杂文档）** | Reviewer SubAgent | `.scribe/review.md` | 复杂文档（API 文档、设计文档、项目级编排）多一道独立审视更稳 |
| **Phase 4 Review（简单文档）** | 主 Agent 内联 checklist | 无文件 | README、CHANGELOG 等简单类型不值得开 SubAgent |

**铁律：**

1. **Phase 2 Plan → Checkpoint 1 严禁开 SubAgent 做质检**。写完后主 Agent 就地对照 5 条清单核查（见下方 Plan 自查段），按结论改完 plan.md，然后进入 Checkpoint 1。
2. **Phase 4 Review 默认不开 SubAgent**——只读 references/review-checklist.md 做内联自查。仅当文档类型为「复杂类型」（API 文档、设计文档、ADR、项目级编排）且用户有 SubAgent 环境时，才升级为独立 Reviewer SubAgent。
3. **拿到任何质检结论——先按 fail 项把产出改完，再汇报"做完了 + 自检结论 + 改了什么"**。直接拿原始结论汇报但不修复 = 违规。
4. **决策收集铁律·禁止静默替用户选择**：在每个 Checkpoint，所有需要用户确认的决策项**必须每项独立列出 + 等用户答复**。Agent **可以推荐**（"我推荐 X，因为…"），但**不能"已经替你定了 X，如果不对再说"**。

---

## 各阶段文件读取指南（渐进加载，别一次全读）

| 阶段 | 必读 | 按需查 |
|---|---|---|
| Phase 0 Intake | 本 SKILL.md（工作流 + 边界 + 核心规则） | —— |
| Phase 1 Context | `references/doc-map.md`（仅当文档类型不清时） | — |
| Phase 2 Plan | 对应文档类型的 L3 模板（见下方识别表） | `references/general-principles.md` |
| Phase 3 Draft | 同 Phase 2 的 L3 模板（"必读"列） | `references/new-doc-capability-check.md`（新建时封装项目已有能力） |
| Phase 4 Review | `references/review-checklist.md` | — |
| Phase 5 Revise | — | `references/review-checklist.md`（只看修复段） |
| Phase 6 Deliver | — | — |

## Phase 0 — Intake

### 判断工作模式

| 模式 | 典型信号 |
|------|---------|
| **新建**（单文档） | "写个"、"创建"、"帮我生成"、"还没有文档" |
| **修改/规范** | "改一下"、"优化"、"规范一下"、"重构"、"review 一下"、"看看哪里有问题"、"这里不对"、"错了"、"帮我看看这个文档" |
| **项目级编排**（多文档） | "我新建了个项目，帮我定规范"、"初始化项目文档"、"项目要哪些文档"、"看看我项目缺哪些文档"、"补齐文档" |

**跳转规则：**
- **修改模式** → 加载 `references/modify-doc.md`，走修改流程（Phase 1→2→3→4→5→6，但 Phase 2 Plan 基于现有文档诊断而非从零规划）。
- **项目级编排模式** → 加载 `references/project-setup.md`，走项目编排流程。
- **新建模式** → 先做下方「新建前的存在性检查」，再继续。

### 新建前的存在性检查（必做）

用户说"写个/创建 X"不代表 X 不存在。动笔前先确认目标文件是否已存在（如 `README.md`、`CHANGELOG.md` 等固定文件名的文档）。**若已存在，不要直接覆盖**——告知用户并询问：是在现有基础上修改/补充（转入修改流程），还是确实要重写覆盖？由用户决定。覆盖现有文档是不可逆操作，绝不擅自进行。

---

## Phase 1 — Context

收集项目上下文。提问要具体，避免开放式"还有什么需要补充的吗？"：

- **项目背景**：项目名称、用途、技术栈
- **目标受众**：写给谁看（其他开发者？AI agent？新成员？）
- **已有素材**：是否有现有文档、代码注释、设计文档可参考
- **特殊要求**：格式偏好、长度限制、必须包含的章节

### 识别文档类型

根据用户的描述判断文档类型：

| 文档类型 | 典型信号 | L3 参考文件 |
|---------|---------|------------|
| `CLAUDE.md` / `AGENTS.md` | "claude 规则"、"项目指令"、"agent 上下文"、"agents.md"、"codex 规则"、"agent 指令文件" | `agent-instructions.md` |
| `SKILL.md` | "skill"、"技能文件"、"agent skill"、"slash command" | `skill-spec.md` |
| `README` | "项目说明"、"readme"、"介绍文档" | `readme.md` |
| `CONTRIBUTING.md` | "贡献指南"、"开发规范"、"怎么参与贡献"、"PR 流程" | `contributing.md` |
| `CHANGELOG` | "变更日志"、"更新记录"、"release notes" | `changelog.md` |
| `PROGRESS.md` | "项目进度"、"进度文档"、"待办清单"、"roadmap"、"路线图"、"todo 列表"、"分阶段计划" | `progress.md` |
| API 文档 | "接口文档"、"REST API"、"API 参考"、"OpenAPI"、"swagger"、"endpoint 文档" | `api-doc.md` |
| 设计/架构文档 | "设计文档"、"架构文档"、"系统设计"、"技术方案"、"RFC"、"提案" | `design-doc.md` |
| Multi-file 设计笔记 | "多文件设计笔记"、"项目设计笔记"、"agent-design-study"、"按主题分组的设计文档"、"源码 ≥ 5000 行出笔记" | `design-doc-multi-file.md` |
| ADR | "架构决策记录"、"adr"、"决策记录"、"技术选型记录" | `adr.md` |
| `SECURITY.md` | "安全策略"、"漏洞上报"、"security.md"、"安全披露" | `security.md` |
| 部署/运维文档 | "部署文档"、"运维文档"、"上线步骤"、"配置说明"、"deployment" | `deployment.md` |
| FAQ / 故障排查 | "faq"、"常见问题"、"故障排查"、"troubleshooting"、"排错指南" | `faq.md` |
| 治理小文档 | "issue 模板"、"pr 模板"、"行为准则"、"code of conduct"、".github 模板" | `governance.md` |
| 提交信息规范 | "提交规范"、"commit message"、"commit 规范"、"conventional commits"、"提交信息格式" | `commit-message.md` |
| 测试文档 | "测试用例"、"测试文档"、"测试计划"、"测试策略"、"test case"、"怎么写测试"、"测试规范" | `test-doc.md` |

若无法确定类型，或用户在两类文档之间犹豫，加载 `references/doc-map.md`（文档职责总览）辅助选型，再向用户确认。

**当文档类型是 SKILL.md 时**，额外确认目标 agent：

> 这个 skill 是给哪个 agent 用的？pi、Claude Code、还是 Codex？如果是多个，我会按通用规范写。

| 目标 agent | 加载文件 |
|-----------|---------|
| 不确定 / 多 agent | `skill-spec.md`（通用规范） |
| pi | `skill-spec.md` + `skill-pi.md` |
| Claude Code | `skill-spec.md` + `skill-claude.md` |
| Codex | `skill-spec.md`（Codex 暂无独立文件，走通用） |

**核心原则**：不确定 agent 时，只输出 name + description + 通用 L2 内容，不添加任何 agent 特有的字段或约定。

---

## Phase 2 — Plan

> 计划是决策的容器，不是写给自己看的笔记——必须让用户看得懂、能确认。

形成写作方案，**不直接写文档**。在 `.scribe/plan.md` 中输出三段内容：

- **Brief**：目标受众 / 文档核心目标 / 必须包含的信息 / 可删减的信息 / 语气风格 / 目标语言
- **Outline**：章节列表，每节一句话说明包含什么
- **特殊注意事项**：容易遗漏的边角、需要额外确认的点

> 项目级编排模式不走此处的 Plan，由 `references/project-setup.md` 的流程覆盖（先扫描现状再列推荐清单交用户勾选）。

### Plan 自查清单（主 Agent 内联，不开 SubAgent）

写完后主 Agent 就地逐项检查：

1. **目标受众是否明确？** 写给开发者、AI agent、还是新成员？不同受众导致完全不同的结构。
2. **信息范围是否界定？** 哪些信息必须保留、哪些可删——和用户确认过吗？
3. **Outline 是否覆盖了用户指定的所有重点？** ——不要遗漏用户提过的关键内容。
4. **章节之间有没有明显缺失？** 比如 README 少了安装步骤、SKILL.md 少了 When to Use。
5. **这份计划如果给用户看，用户能不能看懂并做决策？** ——避免内部术语和模糊表述。

按结论修复 plan.md，然后进入 Checkpoint 1。

### ★ Checkpoint 1 — 必须停

逐项向用户展示并确认：

- 文档类型和 agent 环境（如果是 SKILL.md）
- Brief 中的关键决策（受众、语言、包含/不包含什么）
- Outline 大纲
- **推荐 + 理由**，但用户说了算

> "我推荐按这个大纲展开，因为覆盖了你提到的所有重点。请看 Outline 有没有需要调整的地方？"

确认后继续 Phase 2（修改模式）→ 直接加载目标文件的现有内容做诊断 — 或 Phase 3（新建模式）→ 开始撰写。

---

## Phase 3 — Draft

基于 `plan.md` 和对应 L3 模板，生成完整文档。

生成时遵循以下原则：
- 从模板出发，按实际需求删减/补充
- 不虚构项目信息（不确定的标 `<!-- TODO -->`）
- 优先使用 Markdown 格式
- 保持语言简洁，一个段落只表达一个意思
- 模板不可死搬：多余章节果断删，缺失的补充

**新建模式**：输出完整文档到 `.scribe/draft.md`（先不写入最终路径）。
**修改模式**：输出修改后的完整文档到 `.scribe/draft.md`，保留诊断记录在 `.scribe/review.md`。
**项目级编排模式**：按 `references/project-setup.md` 流程，逐个生成、逐个交付。

### 生成质量硬性规则

以下规则是**必须遵守**的，不是建议。违反这些规则是导致文档需要返工的常见原因。

1. **表格必须对齐**：Markdown 表格的 `|` 分隔线必须与列宽对齐，表头与内容之间用 `|---|---|` 分隔。禁止不对齐的表格。
2. **代码块必须标注语言**：所有代码块（包括 shell 命令、配置文件片段）必须标注语言，如 ````bash`、````yaml`、````python`。禁止裸 ````。
3. **TODO 格式统一**：所有不确定的内容用 `<!-- TODO: 请补充 xxx -->`，不使用 `[TODO]`、`{TODO}`、`...` 等变体。
4. **禁止空段落占位符**：段落中不得出现 `...`、`xxx`、`<说明>` 等占位文字——要么写完整，要么标 `<!-- TODO -->`。
5. **链接必须有目标**：所有 `[文字]()` 必须填写真实 URL 或标注 `<!-- TODO: 补充链接 -->`。禁止空括号链接。
6. **标题层级必须递增**：`#` → `##` → `###`，禁止跳跃（如 `#` 后直接 `###`）。

### Draft 自查清单（主 Agent 内联，不开 SubAgent）

初稿写完后，主 Agent 就地逐项检查：

1. **信息完整性**：计划里确认要包含的内容都写进去了吗？有遗漏吗？
2. **不虚构信息**：有没有任何内容是推断的、没确认的？有则标 `<!-- TODO -->`。
3. **格式正确性**：Markdown 渲染有没有明显问题（表格对齐、代码块标记、链接语法）？
4. **语言一致性**：全文风格是否统一？中英文混合是否合理？
5. **可执行 > 抽象**：每条建议是否可操作？"运行 `npm test`"好于"进行测试"。

按结论修复 `draft.md`，然后进入 Phase 4。

---

## Phase 4 — Review

### 质检选择

| 条件 | 方式 |
|---|---|
| 简单文档（README、CHANGELOG、PROGRESS、提交规范、CONTRIBUTING） | 主 Agent 加载 `references/review-checklist.md` 做内联自查，结果写在消息里 |
| 复杂文档（API 文档、设计文档、ADR、项目级编排、用户特别要求审阅） | Reviewer SubAgent 加载 `references/review-checklist.md` 写 `.scribe/review.md` |

SubAgent 评审 prompt 模板：

> 你是一名文档审阅专家。请根据 project.md 中的约定和 review-checklist.md 中的清单，评审 draft.md，找出全部 issues。按优先级分组：🔴 必须修 / 🟡 建议改 / 🟢 可选优化。每条 issue 给出具体位置和修改建议。

### ★ Checkpoint 2 — 必须停

展示评审结论给用户：
- 如果是内联自查：列出发现的 issues + 修复建议
- 如果是 SubAgent 评审：展示 `.scribe/review.md` 内容

> "评审发现 2 个必须修复的问题 + 3 个建议优化项。你看要全修还是挑着修？"

用户确认修复范围和优先级后，进入 Phase 5。

---

## Phase 5 — Revise

**最小切片修复，禁止整篇无脑重写。**

- 每个 issue 单独修复，修一个确认一个
- 修改直接在 `.scribe/draft.md` 上做
- 有修复才写 `.scribe/review.md` 追加修复日志，没修复直接进 Phase 6

修复完成后进入 Phase 6。

---

## Phase 6 — Deliver

### ★ Checkpoint 3 — 最终确认

展示最终版本给用户确认。逐项问：

1. 内容是否满意？
2. 文件路径是否正确（如 `README.md`、`docs/api.md`）？
3. 还要不要做其他调整？

确认后写入目标路径。如果用户是**修改模式**且原文件已存在：
- 写入前询问：**覆盖原文件还是保留备份？**
- 保留备份方案：原文件重命名为 `README.md.bak`，再写入新版本

交付时附简短编辑说明，告知用户写了什么、改了哪里。

---

## 核心规则

- **先加载 L3 再动笔**：每种文档类型有对应的 L3 参考指南，必须先阅读再操作
- **不虚构信息**：不确定的地方用 `<!-- TODO: 请补充 xxx -->` 标注，不要编造
- **模板不可死搬**：模板是起点，根据项目实际情况增删
- **优先参考已有素材**：如果用户提供了现有文档或代码，优先从中提取信息并保持风格一致
- **单一职责**：每段话只表达一个核心意思；每个章节只负责一个主题
- **可执行 > 抽象**：写"运行 `npm test`"而不是"进行测试"
- **不覆盖未确认的现有文档**：新建前先查目标文件是否已存在；已存在则先问用户"改还是重写"
- **修改文档时**：先加载 `references/modify-doc.md`，修改场景有独立的规则和步骤

## 输出格式

新建模式直接输出完整 Markdown 文档到 `.scribe/draft.md`，交付时写入目标路径。在文档开头标注类型和说明：

```markdown
<!-- 文档类型：{CLAUDE.md | SKILL.md | README | ...} -->
<!-- 项目：{项目名称} -->
<!-- 目标受众：{受众描述} -->

# {标题}
...
```

修改模式先输出诊断清单，用户确认后再输出修改后的文档并交付。详见 `references/modify-doc.md`。

## 特殊情况

### 用户只给了模糊需求
不要猜测，也不要急着生成。先问清楚文档类型和项目背景，确认后加载对应 L3 文件再开始。

### 多个文档需要写/改
按优先级逐个处理。先完成一个，再开始下一个。不要一次生成多份文档。

### 项目目录需要扫描
若用户未提供项目背景但要求写**单份**文档，先让用户描述项目，或建议用户提供关键文件路径供阅读。不要盲目递归遍历大代码库。

> 例外：**项目级编排流程**会扫描项目根目录的顶层文档（看缺哪些 README/AGENTS.md 等），这是轻量、有界的检查，属于该流程的正常步骤，不在此限。

## references/ 目录索引

| 文件 | 用途 |
|------|------|
| `references/harness.md` | Harness 6 概念视角（本 Skill 的设计哲学） |
| `references/review-checklist.md` | 质检清单（Plan / Draft / Final 各阶段检查项） |
| `references/agent-instructions.md` | CLAUDE.md / AGENTS.md 规范 |
| `references/skill-spec.md` | SKILL.md 通用规范 |
| `references/readme.md` | README 规范 |
| `references/contributing.md` | 贡献指南规范 |
| `references/changelog.md` | CHANGELOG 规范 |
| `references/progress.md` | PROGRESS.md 进度文档规范 |
| `references/api-doc.md` | API 文档规范 |
| `references/design-doc.md` | 设计/架构文档规范 |
| `references/design-doc-multi-file.md` | Multi-file 设计笔记（项目设计学习笔记）规范 |
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

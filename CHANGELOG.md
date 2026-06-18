# Changelog

格式基于 [Keep a Changelog](https://keepachangelog.com/zh-CN/1.1.0/)，
版本号遵循 [Semantic Versioning](https://semver.org/lang/zh-CN/)。

## [Unreleased]

### Added
- 新增 **项目级文档编排**能力(`references/project-setup.md`):支持「项目初始化」与「文档补齐」两个场景,共用统一流程(定项目类型 → 扫描根目录现状 → 算推荐集与缺口 → 列清单交用户确认 → 逐个生成);推荐集采用「通用基础集 + 类型附加项」模型;**不自作主张全建,由用户勾选**
- SKILL.md 工作模式从「新建 / 修改」两种扩展为三种,新增「项目级编排」;新增对应流程 section
- 新增 **提交信息规范**撰写能力(`references/commit-message.md`,Conventional Commits 格式、类型表、好/坏示例、与 CHANGELOG 的边界),并接入完整同步清单(SKILL.md×2、modify-doc.md、README×2、trigger-examples、doc-map 边界)
- 为 scribe 自身新增 `docs/COMMIT_CONVENTION.md`,CONTRIBUTING「提交规范」节改为指向它(单一信息源),README「文档」节加入口

### Changed
- 更新 L1 description(触发判断的唯一依据):补全项目初始化/补齐、API/设计文档/ADR/项目进度/提交规范等能力关键词——此前 description 停留在早期版本,新增能力无匹配词可能导致 skill 不触发(召回率问题)
- 合并 SKILL.md 第 2 步「类型识别表」与第 4 步「L3 加载表」为一张三列表(类型 | 信号 | L3 文件):两表原以同一文档类型为主键并列维护,合并后新增文档类型由"改两处"降为"改一处";同步精简 CONTRIBUTING 同步清单(7→6 步)、AGENTS.md 与 PR 模板描述
- 精简 SKILL.md(209→199 行):删除「特殊情况 / 需要参考 Skill 设计规范」中与第 2 步重复的 agent 加载表,将其独有的「多 agent→skill-agents.md」信息合并进第 2 步,消除 L2 内的三表冗余
- 删除孤儿文件 `references/skill-md.md`:零路由引用,内容已被 `skill-spec.md` 完整覆盖,且其 frontmatter 示例含 `compatibility`/`metadata` 等已废弃字段(违反 ADR-0002);同步移除 README 结构树引用
- 规范化审查:AGENTS.md 文档类型清单补全至 13 类;AGENTS.md「同步清单」去重,改为指向 CONTRIBUTING.md 单一信息源
- CONTRIBUTING.md 项目结构树补全 `docs/`、`.github/`、PROGRESS/SECURITY 等;「5 处同步」更正为「同步清单」(实为多处,含 trigger-examples 与 doc-map 边界)
- README 新增「文档」导航节,指向 PROGRESS、ARCHITECTURE、ADR、FAQ 等
- CONTRIBUTING 引用路径检查脚本覆盖范围扩展到 `docs/`

## [0.7.0] - 2026-06-18

### Added
- 新增 **6 类文档**撰写能力,达成开发全周期覆盖:
  - 设计 / 架构文档(`references/design-doc.md`,含现状架构文档与 RFC 两种形态)
  - ADR 架构决策记录(`references/adr.md`,MADR 精简模板,一决策一文件、不可变)
  - SECURITY.md(`references/security.md`,私密上报渠道 + 响应流程 + 支持版本)
  - 部署 / 运维文档(`references/deployment.md`,环境/配置项/上线/回滚/排障)
  - FAQ / 故障排查(`references/faq.md`,FAQ 问答制 + 排障"症状→原因→解决"制)
  - 治理小文档(`references/governance.md`,Issue/PR 模板、CODE_OF_CONDUCT)
- `trigger-examples.md` 新增上述 6 类的触发示例
- 为 scribe 自身补齐适用文档:`docs/ARCHITECTURE.md`(三层架构)、`docs/adr/0001-0003`(三条关键决策)、`docs/FAQ.md`、`SECURITY.md`、`.github/` Issue/PR 模板

### Changed
- SKILL.md 文档类型识别表、L3 加载表接入上述 6 类
- **移除 SKILL.md「不适用场景」中"设计文档 / RFC / ADR 不覆盖"的声明**——这些现已由本 skill 覆盖
- `modify-doc.md` 类型识别表、README 支持类型表与项目结构树同步接入 6 类
- README 新增「维护与联系」小节(维护者、Issue、安全上报入口);`references/readme.md` 规范同步补入「维护与联系」章节与取舍建议
- 填写 SECURITY.md 安全联系邮箱
- 新增**图表规范**(`general-principles.md`):除目录结构用 ASCII 树外,所有图(流程/架构/时序/ER 等)一律用 mermaid;`design-doc.md` 同步强化,`docs/ARCHITECTURE.md` 的流程图改为 mermaid

## [0.6.0] - 2026-06-18

### Added
- 新增 **项目进度文档(PROGRESS.md)** 撰写能力:新建 L3 指南 `references/progress.md`(时序阶段制模板、状态标记约定、DO/DON'T、章节取舍、与 CHANGELOG 的边界)
- 新增 **文档职责总览** `references/doc-map.md`:横切所有文档类型的总纲,讲清每类文档管什么、易混边界(PROGRESS vs CHANGELOG、README vs 设计文档等)、选型决策与成熟度分阶段
- 新增项目自身的 `PROGRESS.md`,分阶段记录已完成能力与后续规划
- `trigger-examples.md` 新增「写项目进度文档」触发示例

### Changed
- SKILL.md 文档类型识别表、L3 加载表接入 PROGRESS.md 与 doc-map.md;类型不清/边界模糊时引导加载 doc-map.md
- `modify-doc.md` 类型识别表新增 PROGRESS.md / ROADMAP.md / TODO.md
- README 支持类型表、项目结构树同步接入 doc-map.md 与 progress.md,并链接文档职责总览

## [0.5.0] - 2026-06-18

### Added
- 新增 **API 文档**撰写能力，覆盖三类：REST/HTTP 接口文档、库/SDK API 参考、OpenAPI/Swagger 规范
- 新建 L3 参考指南 `references/api-doc.md`（含三类各自的结构模板、DO/DON'T、章节取舍、示例）
- `trigger-examples.md` 新增「写 API 文档」触发示例
- 支持 **AGENTS.md** 开放标准（agents.md）：指令文件指南泛化为「agent 指令文件」通用指南，覆盖 CLAUDE.md 与 AGENTS.md，补充两者选择建议与 monorepo 嵌套就近原则；L3 文件由 `claude-md.md` 重命名为 `references/agent-instructions.md`

### Changed
- 「不适用场景」调整 API 文档边界：手写接口文档/SDK 参考/OpenAPI 由本 skill 覆盖，仅「从注释全量自动生成」交给 TypeDoc/Sphinx/JSDoc
- SKILL.md 文档类型表、L3 加载表，`modify-doc.md` 类型识别表，README 支持类型表/项目结构同步接入 API 文档
- `agent-instructions.md`：目录结构从「建议放」降级为「默认不放」（agent 会自己扫目录，目录说明属于 README 职责），同步删除结构模板与示例中的目录段落
- `contributing.md`：PR 流程与 Issue 规范两节各补一个填好的示例，让生成的贡献指南更具体可仿照
- 新增项目自身的 `CONTRIBUTING.md`（针对纯文档 skill 裁剪，含「5 处同步」清单与四层验证方式）

## [0.4.0] - 2026-06-16

### Changed
- **通用化改造**：去掉 `compatibility`、`metadata` 等 agent 专属字段，仅保留 `name` + `description` + `license`
- 工具调用描述通用化，不再假设特定 agent 的工具名称

### Fixed
- 修复步骤编号 Bug：第 1 步的跳转目标从"第 6 步"改为"下方 section"
- `modify-doc.md` 新增步骤 A：修改前先识别文档类型 + agent 环境

### Added
- `modify-doc.md` 补充触发信号（错了/不太对/格式不统一/删掉/太长了拆一下）
- `skill-claude.md` 字段表从 5 个扩展到 14 个
- `skill-pi.md` 补回快速检查清单
- L3 加载表格补充 `modify-doc.md`

## [0.3.0] - 2026-06-16

### Changed
- 将「修改文档流程」从 L2 拆到 L3 `references/modify-doc.md`，L2 只保留加载指针
- SKILL.md 主体精简 ~80 行

## [0.2.0] - 2026-06-16

### Changed
- 分离通用规范与 agent 差异：`skill-spec.md` + `skill-agents.md` + `skill-pi.md` + `skill-claude.md`

### Added
- 新增「修改文档流程」：支持局部修改、规范化、重构、审查四种模式
- 新增修改场景核心规则：先诊后治、最小改动、每改必解释、不推断意图
- SKILL.md L2 增加 agent 环境识别步骤

## [0.1.0] - 2026-06-15

### Added
- 初始版本
- 支持 CLAUDE.md、SKILL.md、README、CONTRIBUTING.md、CHANGELOG 五种文档类型

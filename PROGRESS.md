# 项目进度

> 项目目标:让 scribe 覆盖开发全周期的文档撰写需求,每类文档职责清晰、边界不重叠。
> 当前阶段:阶段六(已完成,v0.10.0 发布) · 最后更新:2026-06-21

## 阶段一:基础文档能力 ✅ 已完成

**目标**:覆盖开发中最高频的文档类型,每类配 L3 参考指南并接入 skill 路由。(对应 CHANGELOG 0.1.0 → 0.5.0)

- [x] SKILL.md 三层架构 + 新建/修改两条流程
- [x] CLAUDE.md / AGENTS.md 指令文件指南(含 AGENTS.md 开放标准)
- [x] SKILL.md 编写指南(通用规范 + pi / Claude Code 差异)
- [x] README 编写指南
- [x] CONTRIBUTING.md 编写指南
- [x] CHANGELOG 编写指南(Keep a Changelog)
- [x] API 文档编写指南(REST / SDK / OpenAPI 三类)

## 阶段二:文档职责体系 + 进度文档能力 ✅ 已完成

**目标**:建立统领全局的文档职责总览,补齐"项目进度"文档类型。退出标准:doc-map 与 progress 两份规范就绪并完成 5 处路由接入。

- [x] 新增 `references/doc-map.md`(文档职责总览,横切总纲)
- [x] 新增 `references/progress.md`(项目进度文档编写指南)
- [x] 接入 skill 路由(SKILL.md×2、modify-doc.md、README×2、trigger-examples)
- [x] 为本项目编写 PROGRESS.md
- [x] CHANGELOG 记录本次新增能力(0.6.0)

## 阶段三:补齐治理与设计类文档 ✅ 已完成

**目标**:覆盖开发全周期剩余的文档类型,达成"全周期覆盖"。每类 = 新增 `references/*.md` + 同步 5 处接入点。

- [x] 设计文档 / 架构文档指南(已同步移除 SKILL.md「不适用场景」中的排除声明)
- [x] ADR 架构决策记录指南
- [x] SECURITY.md 指南
- [x] 部署 / 运维文档指南
- [x] 故障排查 / FAQ 指南
- [x] 治理小文档(Issue/PR 模板、CODE_OF_CONDUCT)
- [x] 给 scribe 自身补齐适用文档:`docs/ARCHITECTURE.md`、`docs/adr/0001-0003`、`docs/FAQ.md`、`SECURITY.md`、`.github/` 模板(部署/运维不适用,已排除)

## 阶段四:项目级能力 + 自审优化(v0.8.0) ✅ 已完成

**目标**:从"逐份写文档"升级到"项目级编排",并通过自审与端到端实测打磨流程质量。

- [x] 项目级文档编排能力(`project-setup.md`):初始化 + 补齐两场景,列清单交用户确认
- [x] 提交信息规范能力(`commit-message.md`)+ scribe 自身 `docs/COMMIT_CONVENTION.md`
- [x] 结构精简:删孤儿文件、合并 SKILL.md 识别表/加载表、L2 减重(209→195 行)
- [x] 立"多能力 Skill L2 膨胀阈值"通用规则;13 类文档规范边界声明全统一
- [x] 端到端实测修复 4 个流程盲点:信号歧义、多文档生成顺序、封装型文档定位实现、新建前覆盖检查

## 阶段五:补齐测试文档能力(v0.9.0) ✅ 已完成

**目标**:补上开发周期里高频但尚未覆盖的「测试文档」类型,文档类型从 13 类增至 14 类。

- [x] 新增 `references/test-doc.md`:一份指南统管三形态(测试用例规格 / 测试策略计划 / 测试编写指南),仿 `faq.md` 多形态结构
- [x] 取向定为**默认独立成文**,仅极小项目并入 CONTRIBUTING
- [x] 接入全部同步点(SKILL.md、modify-doc、doc-map ×3、README ×2、AGENTS、trigger-examples、L1 关键词)
- [x] 修复 doc-map 测试边界行重复;通过同步完整性与引用路径验证

## 阶段六: Harness 框架重构 (v0.10.0) ✅ 已完成

**目标**:以 Harness 6 概念重构 SKILL.md 核心流程，提升执行确定性、质检质量和跨阶段决策一致性。

- [x] 新增 `references/harness.md`：定义 6 个设计维度和本 Skill 的对应实现
- [x] 新增 `references/review-checklist.md`：Plan / Draft / Final 三阶段质检清单
- [x] 核心流程重构：6 Phase 线性管道 + 3 个 Checkpoint 检查点
- [x] 文件系统状态管理：`.scribe/plan.md`、`.scribe/draft.md`、`.scribe/review.md`
- [x] 渐进加载策略：每阶段指明必读 vs 按需查
- [x] 显式边界段：先判断是否进 Skill 再执行
- [x] 最小切片修复原则：禁止整篇重写

# 项目进度

> 项目目标:让 scribe 覆盖开发全周期的文档撰写需求,每类文档职责清晰、边界不重叠。
> 当前阶段:阶段四(已完成,v0.8.0 发布) · 最后更新:2026-06-18

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

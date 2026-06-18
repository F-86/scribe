# 项目进度

> 项目目标:让 scribe 覆盖开发全周期的文档撰写需求,每类文档职责清晰、边界不重叠。
> 当前阶段:阶段三(待开始) · 最后更新:2026-06-18

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

## 阶段三:补齐治理与设计类文档 ⬜ 待开始

**目标**:覆盖开发全周期剩余的文档类型,达成"全周期覆盖"。每类 = 新增 `references/*.md` + 同步 5 处接入点。

- [ ] 设计文档 / 架构文档指南(最大缺口,需同步移除 SKILL.md「不适用场景」中的排除声明)
- [ ] ADR 架构决策记录指南
- [ ] SECURITY.md 指南(模板化程度最高,优先)
- [ ] 部署 / 运维文档指南
- [ ] 故障排查 / FAQ 指南
- [ ] 治理小文档(Issue/PR 模板、CODE_OF_CONDUCT)

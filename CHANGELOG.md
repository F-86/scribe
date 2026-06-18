# Changelog

格式基于 [Keep a Changelog](https://keepachangelog.com/zh-CN/1.1.0/)，
版本号遵循 [Semantic Versioning](https://semver.org/lang/zh-CN/)。

## [0.5.0] - 2026-06-18

### Added
- 新增 **API 文档**撰写能力，覆盖三类：REST/HTTP 接口文档、库/SDK API 参考、OpenAPI/Swagger 规范
- 新建 L3 参考指南 `references/api-doc.md`（含三类各自的结构模板、DO/DON'T、章节取舍、示例）
- `trigger-examples.md` 新增「写 API 文档」触发示例

### Changed
- 「不适用场景」调整 API 文档边界：手写接口文档/SDK 参考/OpenAPI 由本 skill 覆盖，仅「从注释全量自动生成」交给 TypeDoc/Sphinx/JSDoc
- SKILL.md 文档类型表、L3 加载表，`modify-doc.md` 类型识别表，README 支持类型表/项目结构同步接入 API 文档
- `claude-md.md`：目录结构从「建议放」降级为「默认不放」（agent 会自己扫目录，目录说明属于 README 职责），同步删除结构模板与示例中的目录段落

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

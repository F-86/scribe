# scribe

AI agent 文档编写专家 Skill：撰写、修改、规范、重构 CLAUDE.md / SKILL.md / README / CONTRIBUTING / CHANGELOG / API 文档。纯 Markdown 项目，无代码、无构建、无依赖。

## 改动约定

- **新增文档类型 = 5 处同步**：缺一处就会流程断裂——
  1. 新建 `references/<type>.md`（L3 指南本体）
  2. `SKILL.md` 第 2 步「文档类型识别表」加一行
  3. `SKILL.md` 第 4 步「L3 加载表」加一行
  4. `references/modify-doc.md` 步骤 A 的类型识别表加一行
  5. `README.md` 支持类型表 + 项目结构树，`examples/trigger-examples.md` 触发示例
- **L3 指南统一结构**：定位 → 结构模板 → 编写原则（DO/DON'T）→ 章节取舍 → 示例。新建时套用 `references/readme.md` 的风格。
- **每次功能改动都记 CHANGELOG**：`CHANGELOG.md` 遵循 Keep a Changelog + SemVer，新增一个版本段。
- **提交信息**：Conventional Commits，中文描述（如 `feat: 新增 API 文档撰写能力 (v0.5.0)`）。

## 重要提示

- 这是 skill 的**源码**，改的是流程定义本身，落地效果靠真实对话验证（无自动化测试）。
- 不虚构信息是 skill 的核心规则，改文档时同样适用：不确定的内容用 `<!-- TODO -->` 标注。

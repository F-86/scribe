# Scribe 触发示例

## 新建场景

| 用户说 | 模式 | 处理方式 |
|--------|------|----------|
| "帮我写个 README" | 新建 | 判断文档类型为 README → 加载 `references/readme.md` |
| "给这个项目生成一份贡献指南" | 新建 | 判断文档类型为 CONTRIBUTING.md → 加载 `references/contributing.md` |
| "帮我写个 SKILL.md" | 新建 | 先确认目标 agent → 加载对应规范 |
| "写份 CHANGELOG 记录最近的更新" | 新建 | 判断文档类型为 CHANGELOG → 加载 `references/changelog.md` |
| "帮我写个设计文档" | 新建 | 判断文档类型为设计文档 → 加载 `references/design-doc.md` |
| "初始化一下项目文档" | 项目级编排 | 加载 `references/project-setup.md` |

## 修改/审查场景

| 用户说 | 模式 | 处理方式 |
|--------|------|----------|
| "帮我看看这个 README 有什么问题" | 审查 | 加载 `references/modify-doc.md` + `references/readme.md` |
| "优化一下这个 SKILL.md" | 规范化 | 加载 `references/modify-doc.md` + `references/skill-spec.md` |
| "这里写得不对，改一下" | 局部修改 | 定位具体问题 → 按最小改动原则处理 |
| "这个文档结构太乱了，重构一下" | 重构 | 保持内容，重排结构 |

## 补齐场景

| 用户说 | 模式 | 处理方式 |
|--------|------|----------|
| "看看我项目缺哪些文档" | 补齐 | 加载 `references/project-setup.md` |
| "帮我补齐项目文档" | 补齐 | 扫描根目录 → 列清单 → 用户确认 → 逐个生成 |

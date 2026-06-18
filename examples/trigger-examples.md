# 触发示例

以下示例展示了 scribe Skill 应该被触发的典型场景。这些示例供调试和测试参考，不包含在 SKILL.md 的 L2 中。

## 典型触发

### 写 CLAUDE.md
```
用户：帮我写个 CLAUDE.md，这是个 React + Express 的全栈项目
用户：为这个项目创建一个 rules 文件给 AI agent 用
用户：给项目写 claude 的指令文件
用户：写个 AGENTS.md，要跨工具通用
用户：给这个 monorepo 加一份 agents.md
```

### 写 SKILL.md
```
用户：帮我创建个 skill，用来做 PDF 处理
用户：写个 SKILL.md，功能是自动生成测试报告
用户：我需要建一个 agent skill
```

### 写 README
```
用户：帮我写个 README
用户：给这个库写个项目说明文档
用户：需要一份介绍文档放在 GitHub 首页
```

### 写 CONTRIBUTING.md
```
用户：写个贡献指南
用户：帮我建 CONTRIBUTING.md
用户：项目要开源了，写一份开发规范给贡献者
```

### 写 CHANGELOG
```
用户：帮我写个 CHANGELOG，从 v1.0 开始
用户：整理一下最近的变更记录
用户：创建 release notes
```

### 写 API 文档
```
用户：给这套 REST 接口写 API 文档
用户：帮我写一份 SDK 的 API 参考手册
用户：整理一个 OpenAPI 规范文件
```

### 写项目进度文档
```
用户：帮我写个项目进度文档
用户：建一个 PROGRESS.md，分阶段记录要做什么
用户：给项目加一份 roadmap
用户：整理一个分阶段的待办清单
```

## 不应该触发的场景

```
❌ 用户：帮我写个 API 接口      → 不是文档，是代码
❌ 用户：修复 README 里的错别字  → 直接改就行，不需要完整流程
❌ 用户：翻译 README.md          → 翻译任务，不是从零写文档
❌ 用户：从代码注释自动生成全量 API 文档 → 用 TypeDoc/JSDoc/Sphinx，非手写
```

> 注：手写 REST 接口文档 / SDK 参考 / OpenAPI 规范**应当触发**（见上方「写 API 文档」），仅「从注释全量自动生成」交给工具。

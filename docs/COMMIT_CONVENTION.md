# 提交信息规范

scribe 遵循 [Conventional Commits](https://www.conventionalcommits.org/),用**中文描述**。

## 格式

```
<类型>: <简短描述>(涉及版本变更时标注版本号)

<可选正文:解释为什么改>
```

scribe 是单一文档项目,模块边界不明显,**通常省略 scope**。

## 类型

| 类型 | 用于 | 示例 |
|------|------|------|
| `feat` | 新增文档能力 / 文档类型 | `feat: 新增 API 文档撰写能力 (v0.5.0)` |
| `fix` | 修复流程错误 / 路径错误 | `fix: 修复 L3 加载表的路径错误` |
| `docs` | 改进项目自身的对外文档 | `docs: 补充 README 安装说明` |
| `refactor` | 重构(不改 skill 行为) | `refactor: claude-md.md 重命名为 agent-instructions.md` |

> 这里的 `docs` 指改 scribe **自己的** README/CHANGELOG 等;而新增/改进 `references/*.md` 写作指南属于 skill 的能力变更,用 `feat`/`fix`。

## 规则

- 简短描述用祈使句、现在时:中文用"新增"而非"新增了",结尾不加句号
- **涉及版本号变更时,在描述末尾标注版本**(如 `(v0.7.0)`)——这是 scribe 的特有约定
- 新增文档类型的提交,正文可附「同步清单」勾选项(见 [CONTRIBUTING](../CONTRIBUTING.md#新增一种文档类型同步清单))
- 破坏性变更(如改动 skill 流程导致旧用法失效):在脚注写 `BREAKING CHANGE: ...`

## 示例

```
# ✅ 好
feat: 新增文档职责总览 + 项目进度文档能力 (v0.6.0)

新增 references/doc-map.md 与 references/progress.md,
并接入 SKILL.md 路由、README、trigger-examples。

# ✅ 好
fix: 修复步骤编号 Bug,跳转目标从"第 6 步"改为"下方 section"

# ❌ 差(无类型、过去时、含糊)
更新了一些文档
改了点东西
```

## 与其他文档的关系

- **提交信息 ≠ CHANGELOG**:提交是逐条原始记录;CHANGELOG 是面向用户的聚合摘要。每次功能改动两者都要更新。
- **本规范 ⊂ 贡献流程**:完整贡献流程见 [CONTRIBUTING.md](../CONTRIBUTING.md),其「提交规范」节指向本文件。

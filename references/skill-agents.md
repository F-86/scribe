# Skill Agent 差异

本文档列出各 AI agent 在编写 SKILL.md 时的差异。**不确定用户使用哪个 agent 时，只使用通用规范，并主动询问用户。**

## 通用基线

所有支持 Agent Skills 标准的 agent 共享以下基线（详见 `skill-spec.md`）：

```yaml
---
name: skill-name           # 必须，1-64 字符，小写字母+数字+连字符
description: >-            # 必须，≤ 1024 字符
  核心功能一句话描述。
  关键词：关键词1、关键词2
---
```

L1 只需要这两个字段即可在所有 agent 上运行。L2/L3 的 Markdown 主体对不同 agent 也基本一致。

---

## 差异对照表

| 维度 | pi | Claude Code | Codex |
|------|----|-------------|-------|
| **手动调用** | `/skill:name` | `/name` | `/name` |
| **全局安装路径** | `~/.pi/agent/skills/` | `~/.claude/skills/` | `~/.codex/skills/` |
| **项目安装路径** | `.pi/skills/` | `.claude/skills/` | `.agents/skills/` |
| **命令名来源** | front matter `name` | 目录名（非 front matter `name`） | front matter `name` |
| **L2 约定** | 必须包含环境准备、不适用场景、L3 加载指针 | 保持简洁，建议用 `when_to_use` 控制触发 | 无额外约定 |
| **L3 加载指针** | 写明 `read` 工具调用 | 不强制指定工具 | 不强制指定工具 |

### L1 字段兼容性

| 字段 | 标准 | pi | Claude Code | Codex | 建议 |
|------|------|----|-------------|-------|------|
| `name` | ✅ | ✅ | ✅（仅作显示名） | ✅ | 始终填写 |
| `description` | ✅ | ✅ | ✅（推荐） | ✅ | 始终填写 |
| `license` | 标准 | ✅ 识别 | ⚠️ 忽略 | ⚠️ 忽略 | 发布到 GitHub 时填写 |
| `compatibility` | 标准 | ✅ 显示 | ⚠️ 忽略 | ⚠️ 忽略 | pi 专属，跨 agent 时不写 |
| `metadata` | 标准 | ✅ 识别 | ⚠️ 忽略 | ⚠️ 忽略 | pi 专属 |
| `allowed-tools` | 标准 | ✅ 实验性 | ✅ | ⚠️ 实验性 | 谨慎使用 |
| `disable-model-invocation` | 标准 | ✅ | ✅ | ⚠️ 实验性 | 通用，可用 |
| `user-invocable` | — | ❌ | ✅ | ❌ | Claude Code 专属 |
| `when_to_use` | — | ❌ | ✅ | ❌ | Claude Code 专属 |
| `context: fork` | — | ❌ | ✅ | ❌ | Claude Code 专属 |

> **安全策略**：当不确定目标 agent 时，**只写 `name` + `description`**。其他字段可能被某些 agent 忽略（⚠️）或导致未知行为（❌）。

---

## 针对不同 agent 的详细规范

- **pi**：使用 `read` 加载 `references/skill-pi.md`
- **Claude Code**：使用 `read` 加载 `references/skill-claude.md`
- 不确定或需要多 agent 兼容：只使用通用规范 `skill-spec.md`

---

## 应避免的做法

### ❌ 在不确定 agent 时添加特有字段

```yaml
# ❌ 如果用户可能用 Claude Code，这些字段可能不被识别
compatibility: "需要 Python 3.10+"
metadata:
  version: "1.0.0"
```

### ❌ 在 L2 中写入 agent 特有的命令格式

```markdown
# ❌ pi 特有的命令格式
使用 `/skill:name` 手动触发

# ✅ 通用写法
手动调用此 skill
```

### ❌ 假设特定的工具名称

```markdown
# ❌ 假设 agent 一定有 read 工具
使用 `read` 工具加载 references/api.md

# ✅ 通用写法
加载 references/api.md 获取详细信息
```

---

## 检查清单

写 SKILL.md 前确认：

- [ ] 用户使用的 agent 是什么？（不确定就问）
- [ ] 如果目标是多 agent 兼容 → 只写通用规范 + `name`/`description`
- [ ] 如果只针对特定 agent → 加载对应的 agent 规范文件

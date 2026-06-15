# Claude Code Skill 编写约定

> 仅在**确认用户使用 Claude Code** 时加载此文件。通用规范见 `skill-spec.md`，多 agent 差异见 `skill-agents.md`。

Claude Code 遵循 [Agent Skills 标准](https://agentskills.io/specification)，沿用在 `skill-spec.md` 中定义的 L1/L2/L3 三层架构，此处只补充 Claude Code 特有的字段和约定。

## 关键差异

与 pi 最大的不同：**命令名来自目录名，而非 front matter 的 `name` 字段**。

```
.claude/skills/deploy-staging/SKILL.md  → 用户输入 /deploy-staging
```

front matter 中的 `name` 仅作为显示名。

## L1 front matter 字段

Claude Code 支持比 pi 更多的可选字段：

```yaml
---
name: skill-name               # 显示名（非命令名）
description: >-                # 推荐填写，Claude 据此判断是否自动触发
  功能描述，≤ 3 行。
when_to_use: >-                # Claude Code 专属，额外的触发条件描述
  用户提到"部署"、"发布"时触发。
argument-hint: "[name]"        # 自动补全提示
disable-model-invocation: true # 禁止 Claude 自动调用，仅手动 /name
user-invocable: false          # 从 / 菜单隐藏，但 Claude 可调用
allowed-tools: Bash(git *)     # 预批准工具
paths: "src/**/*.ts"           # 仅在操作匹配文件时激活
---
```

### 常用可选字段说明

| 字段 | 用途 |
|------|------|
| `when_to_use` | 补充 `description`，给 Claude 更多触发提示 |
| `argument-hint` | `/` 菜单自动补全时的参数提示 |
| `user-invocable: false` | 不让用户手动调用，只供 Claude 自动使用 |
| `paths` | 按文件 glob 限定激活范围 |
| `context: fork` | 在独立子 agent 中运行（不污染主会话上下文） |

## 命令调用

```bash
/name                              # 手动触发（name = 目录名）
/name arg1 arg2                    # 带参数
```

## L2 编写原则

Claude Code 官方建议 skill 主体保持简洁：

> "一旦 skill 加载，其内容会在整个会话中保留在上下文中，每一行都是持续的 token 成本。陈述要做什么，不要叙述怎么做或为什么。"

- 优先简短的指令而非长篇教程
- 用 `when_to_use` 而非冗长的描述来控制自动触发
- L3 加载方式不强制指定工具（写"参照 `references/api.md`"即可）

## 文件夹结构

与通用规范一致：

```
{skill-name}/
├── SKILL.md                  # 必须
├── references/               # 条件性知识
├── examples/                 # 示例
├── scripts/                  # 可执行脚本
└── assets/                   # 模板
```

# scribe

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Agent Skills](https://img.shields.io/badge/Agent%20Skills-compatible-6e56cf)](https://agentskills.io/specification)

AI agent 文档编写专家 — 撰写、修改、规范、重构开发者文档。遵循 [Agent Skills](https://agentskills.io/specification) 开放标准，兼容 pi、Claude Code、Codex 等工具。

## 安装

### pi

```bash
# 全局（所有项目可用）
git clone https://github.com/F-86/scribe.git ~/.pi/agent/skills/scribe

# 项目级
cd your-project
git clone https://github.com/F-86/scribe.git .pi/skills/scribe

# 临时加载
pi --skill /path/to/scribe
```

### Claude Code

```bash
# 全局
git clone https://github.com/F-86/scribe.git ~/.claude/skills/scribe

# 项目级
cd your-project
git clone https://github.com/F-86/scribe.git .claude/skills/scribe
```

### Codex (OpenAI)

```bash
# 全局
git clone https://github.com/F-86/scribe.git ~/.codex/skills/scribe

# 项目级
cd your-project
git clone https://github.com/F-86/scribe.git .agents/skills/scribe
```

> 安装后重启 agent 生效。

## 使用

scribe 会在文档相关任务中自动触发，也可手动调用：

| Agent | 手动调用 |
|-------|---------|
| pi | `/skill:scribe` |
| Claude Code | `/scribe` |
| Codex | `/scribe` |

### 典型用法

**新建文档**：
```
帮我写个 CLAUDE.md，这是个 React + Express 全栈项目
给这个项目写个 README
帮我建一个 PDF 处理的 skill
创建贡献指南
整理最近的变更日志
给这套 REST 接口写 API 文档
```

**修改 / 规范文档**：
```
帮我规范一下 CLAUDE.md，格式太乱了
review 一下这个 SKILL.md，看看哪里不对
重构一下 README，结构太长了
```

### 支持的文档类型

| 文档 | 说明 |
|------|------|
| `CLAUDE.md` / `AGENTS.md` | AI agent 项目上下文/指令文件（含 AGENTS.md 开放标准） |
| `SKILL.md` | Agent skill 技能包入口 |
| `README` | 项目首页介绍 |
| `CONTRIBUTING.md` | 贡献者指南 |
| `CHANGELOG` | 版本变更记录（Keep a Changelog 格式） |
| API 文档 | REST 接口 / 库 SDK 参考 / OpenAPI 规范 |

## 项目结构

```
scribe/
├── SKILL.md                      # Skill 主文件（L1 + L2）
├── README.md
├── CHANGELOG.md                  # 版本变更记录
├── references/                   # 按需加载的参考指南（L3）
│   ├── skill-spec.md             # 通用三层架构规范
│   ├── skill-agents.md           # 各 agent 差异对照表
│   ├── skill-pi.md               # pi 专属约定
│   ├── skill-claude.md           # Claude Code 专属约定
│   ├── skill-md.md               # SKILL.md 编写指南
│   ├── agent-instructions.md     # CLAUDE.md / AGENTS.md 编写指南
│   ├── readme.md                 # README 编写指南
│   ├── contributing.md           # CONTRIBUTING.md 编写指南
│   ├── changelog.md              # CHANGELOG 编写指南
│   ├── api-doc.md                # API 文档编写指南（REST / SDK / OpenAPI）
│   ├── modify-doc.md             # 修改/规范/重构文档指南
│   └── general-principles.md     # 通用写作原则
├── examples/
│   └── trigger-examples.md
└── assets/
```

## 许可证

MIT

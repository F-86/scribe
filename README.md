# scribe

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Agent Skills](https://img.shields.io/badge/Agent%20Skills-compatible-6e56cf)](https://agentskills.io/specification)

AI agent 文档编写专家 — 适用于 pi、Claude Code、Codex 等支持 Agent Skills 标准的 AI 编程工具。帮你撰写 CLAUDE.md、SKILL.md、README、CONTRIBUTING.md、CHANGELOG 等开发者文档。

## 安装

scribe 遵循 [Agent Skills](https://agentskills.io/specification) 开放标准，兼容以下 agent：

### pi

**全局安装**（所有项目可用）：
```bash
git clone https://github.com/F-86/scribe.git ~/.pi/agent/skills/scribe
```

**项目级安装**：
```bash
cd your-project
git clone https://github.com/F-86/scribe.git .pi/skills/scribe
```

**临时加载**（不修改配置）：
```bash
pi --skill /path/to/scribe
```

### Claude Code

**全局安装**（所有项目可用）：
```bash
git clone https://github.com/F-86/scribe.git ~/.claude/skills/scribe
```

**项目级安装**：
```bash
cd your-project
git clone https://github.com/F-86/scribe.git .claude/skills/scribe
```

### Codex (OpenAI)

**全局安装**：
```bash
git clone https://github.com/F-86/scribe.git ~/.codex/skills/scribe
```

**项目级安装**：
```bash
cd your-project
git clone https://github.com/F-86/scribe.git .agents/skills/scribe
```

> 以上安装完成后重启 agent 即可生效。

## 使用

scribe 会在相关任务中自动触发，也可以手动调用：

| Agent | 手动调用方式 |
|-------|------------|
| pi | `/skill:scribe` |
| Claude Code | `/scribe` |
| Codex | `/scribe` |

### 典型用法

```
帮我写个 CLAUDE.md，这是个 React + Express 全栈项目
给这个项目写个 README
创建贡献指南 CONTRIBUTING.md
帮我建一个 PDF 处理的 skill
整理最近的变更日志 CHANGELOG
```

### 支持的文档类型

| 文档 | 说明 |
|------|------|
| `CLAUDE.md` | AI agent 项目上下文/指令文件 |
| `SKILL.md` | Agent skill 技能包入口 |
| `README` | 项目首页介绍 |
| `CONTRIBUTING.md` | 贡献者指南 |
| `CHANGELOG` | 版本变更记录（Keep a Changelog 格式） |

## 项目结构

```
scribe/
├── SKILL.md                    # Skill 主文件（L1 + L2）
├── references/                 # 按需加载的参考指南（L3）
│   ├── claude-md.md
│   ├── skill-md.md
│   ├── readme.md
│   ├── contributing.md
│   ├── changelog.md
│   └── general-principles.md
├── examples/
│   └── trigger-examples.md
└── assets/
```

## 许可证

MIT

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

**项目级编排（一次处理多份文档）**：
```
我新建了个项目，帮我定一套基础文档和规范
初始化项目文档
看看我这个项目缺哪些文档，需要补的告诉我
```
> 会先判断项目类型、扫描现状，列出建议补齐的清单交你确认，再逐个生成——不会自作主张全建。

### 支持的文档类型

| 文档 | 说明 |
|------|------|
| `CLAUDE.md` / `AGENTS.md` | AI agent 项目上下文/指令文件（含 AGENTS.md 开放标准） |
| `SKILL.md` | Agent skill 技能包入口 |
| `README` | 项目首页介绍 |
| `CONTRIBUTING.md` | 贡献者指南 |
| `CHANGELOG` | 版本变更记录（Keep a Changelog 格式） |
| `PROGRESS.md` | 项目进度文档（分阶段待办，记录要做什么/做到哪了） |
| API 文档 | REST 接口 / 库 SDK 参考 / OpenAPI 规范 |
| 设计 / 架构文档 | 系统架构现状、技术方案 / RFC |
| ADR | 架构决策记录（单个技术决策的始末） |
| `SECURITY.md` | 安全漏洞上报策略 |
| 部署 / 运维文档 | 环境、配置项、上线步骤、回滚、排障 |
| FAQ / 故障排查 | 常见问题问答、报错排查指南 |
| 治理小文档 | Issue/PR 模板、CODE_OF_CONDUCT |
| 提交信息规范 | Commit Message 格式（Conventional Commits） |

> 各类文档分别管什么、边界在哪，见 [文档职责总览](references/doc-map.md)。

## 项目结构

```
scribe/
├── SKILL.md                      # Skill 主文件（L1 + L2）
├── README.md
├── CHANGELOG.md                  # 版本变更记录
├── references/                   # 按需加载的参考指南（L3）
│   ├── doc-map.md                # 文档职责总览（该写哪类、边界在哪）
│   ├── skill-spec.md             # 通用三层架构规范
│   ├── skill-agents.md           # 各 agent 差异对照表
│   ├── skill-pi.md               # pi 专属约定
│   ├── skill-claude.md           # Claude Code 专属约定
│   ├── agent-instructions.md     # CLAUDE.md / AGENTS.md 编写指南
│   ├── readme.md                 # README 编写指南
│   ├── contributing.md           # CONTRIBUTING.md 编写指南
│   ├── changelog.md              # CHANGELOG 编写指南
│   ├── progress.md               # 项目进度文档（PROGRESS.md）编写指南
│   ├── api-doc.md                # API 文档编写指南（REST / SDK / OpenAPI）
│   ├── design-doc.md             # 设计 / 架构文档编写指南
│   ├── adr.md                    # ADR（架构决策记录）编写指南
│   ├── security.md               # SECURITY.md 编写指南
│   ├── deployment.md             # 部署 / 运维文档编写指南
│   ├── faq.md                    # FAQ / 故障排查编写指南
│   ├── governance.md             # 治理小文档（Issue/PR 模板、行为准则）编写指南
│   ├── commit-message.md         # 提交信息规范（Conventional Commits）编写指南
│   ├── modify-doc.md             # 修改/规范/重构文档指南
│   ├── project-setup.md          # 项目级编排（初始化/补齐多文档）指南
│   └── general-principles.md     # 通用写作原则
├── examples/
│   └── trigger-examples.md
└── assets/
```

## 文档

- [项目进度 PROGRESS.md](PROGRESS.md) — 分阶段的开发进度与计划
- [架构设计 docs/ARCHITECTURE.md](docs/ARCHITECTURE.md) — L1/L2/L3 三层加载与路由机制
- [架构决策记录 docs/adr/](docs/adr/) — 关键技术决策的始末
- [常见问题 docs/FAQ.md](docs/FAQ.md) — 使用与贡献的高频疑问
- [贡献指南 CONTRIBUTING.md](CONTRIBUTING.md) · [提交规范 docs/COMMIT_CONVENTION.md](docs/COMMIT_CONVENTION.md) · [变更日志 CHANGELOG.md](CHANGELOG.md)

## 维护与联系

由 [@F-86](https://github.com/F-86) 维护。

- **使用问题 / Bug / 功能建议**:提 [Issue](https://github.com/F-86/scribe/issues)（见仓库内 Issue 模板）
- **贡献代码**:见 [CONTRIBUTING.md](CONTRIBUTING.md)
- **安全问题**:请勿走公开 issue，见 [SECURITY.md](SECURITY.md)（邮件 19909233758@163.com）

## 许可证

MIT

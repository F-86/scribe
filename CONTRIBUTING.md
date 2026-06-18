# 贡献指南

感谢你为 scribe 贡献。scribe 是一个纯 Markdown 的 AI agent 文档编写 Skill——没有代码、没有构建、没有依赖。贡献的核心是**新增或改进文档类型的编写指南**，并保证 skill 内部各文件的一致性。

## 准备工作

### 环境要求

- Git
- 一个支持 [Agent Skills](https://agentskills.io/specification) 标准的 agent（pi / Claude Code / Codex 之一），用于本地验证改动
- （可选）[markdownlint-cli](https://github.com/igorshubovych/markdownlint-cli)，用于格式检查

无需 Node、包管理器或任何构建工具。

### 克隆

```bash
git clone https://github.com/F-86/scribe.git
cd scribe
```

### 本地加载验证

把仓库软链或克隆到 agent 的 skills 目录，重启 agent 即可调试（以 Claude Code 为例）：

```bash
ln -s "$(pwd)" ~/.claude/skills/scribe
```

其他 agent 的安装路径见 [README](README.md#安装)。

## 项目结构

```
SKILL.md                  — L1 + L2，skill 入口与主流程
references/                — L3 按需加载的分类指南（每种文档类型一个 + doc-map 职责总览）
examples/                  — 触发示例，仅供调试
docs/                      — scribe 自身的架构文档、ADR、FAQ
.github/                   — Issue / PR 模板
README.md / CHANGELOG.md / PROGRESS.md / SECURITY.md — 项目自身的对外文档
```

三层加载（L1/L2/L3）的设计：L1 是 frontmatter（触发判断），L2 是 SKILL.md 正文（主流程），L3 是 `references/*.md`（用到哪类文档才加载哪个）。改动时注意内容放对层级。

## 贡献类型

### 改进现有文档指南

直接修改 `references/<type>.md`。保持该文件的统一结构：**定位 → 结构模板 → 编写原则（DO/DON'T）→ 章节取舍 → 示例**。

### 新增一种文档类型（同步清单）

这是最容易出错的贡献——新增一个文档类型必须同步改动多处，缺一处都会导致流程断裂：

1. 新建 `references/<type>.md`（L3 指南本体，套用 `references/readme.md` 的结构）
2. `SKILL.md` 第 2 步「文档类型识别表」加一行（该表含信号 + L3 文件两列，一处搞定识别与加载）
3. `references/modify-doc.md` 步骤 A 的类型识别表加一行
4. `README.md` 支持类型表 + 项目结构树各加一行
5. `examples/trigger-examples.md` 加触发示例
6. `references/doc-map.md` 补该类型与相邻文档的边界（若有易混淆的邻居）

## 提交前验证

scribe 没有自动化测试，用以下方式自检（建议全部跑一遍）：

### 1. 同步完整性检查

新增/改名文档类型后，用关键词搜索确认它在所有该出现的地方都出现了。例如新增「API 文档」：

```bash
grep -rn "api-doc\|API 文档" SKILL.md references/modify-doc.md README.md examples/trigger-examples.md
```

逐项核对上面「同步清单」，确认无遗漏。

### 2. 引用路径检查

确认 SKILL.md / README 中引用的 `references/*.md` 文件都真实存在：

```bash
grep -rhoE "references/[a-z-]+\.md" SKILL.md README.md | sort -u | while read f; do
  [ -e "$f" ] && echo "OK  $f" || echo "缺失 $f"
done
```

`docs/` 内文档之间也有交叉链接（如 ARCHITECTURE → ADR），一并校验：

```bash
grep -rhoE "adr/[0-9]{4}-[a-z-]+\.md" docs/ | sort -u | while read f; do
  [ -e "docs/$f" ] && echo "OK  docs/$f" || echo "缺失 docs/$f"
done
```

### 3. 真实对话验证

本地加载 skill，从 `examples/trigger-examples.md` 里挑对应的触发句子跑一遍，确认 agent 能：识别出正确的文档类型 → 加载对应 L3 → 产出符合该指南的文档。这是替代单元测试的核心手段。

### 4. 格式检查（可选）

```bash
markdownlint '**/*.md'
```

## 提交规范

使用 [Conventional Commits](https://www.conventionalcommits.org/)，**中文描述**，涉及版本号变更时在描述末尾标注版本（如 `(v0.7.0)`）：

```
feat: 新增 API 文档撰写能力 (v0.5.0)
fix: 修复 L3 加载表的路径错误
```

完整的类型说明、规则与示例见 [docs/COMMIT_CONVENTION.md](docs/COMMIT_CONVENTION.md)。

## 变更记录

每次功能性改动都要在 [`CHANGELOG.md`](CHANGELOG.md) 记录，遵循 [Keep a Changelog](https://keepachangelog.com/zh-CN/1.1.0/) 格式与 [SemVer](https://semver.org/lang/zh-CN/)：

- 新增能力 → `Added`
- 调整已有行为 → `Changed`
- 修复问题 → `Fixed`

未发布的改动累积在最新版本段内。

## Pull Request 流程

1. Fork 本仓库
2. 创建功能分支（`feat/xxx` 或 `fix/xxx`）
3. 完成改动，跑一遍上方「提交前验证」
4. 更新 `CHANGELOG.md`
5. 提交 PR，说明**改了什么**和**为什么改**；新增文档类型时在描述里勾选「同步清单」
6. 等待 review

PR 标题遵循 Conventional Commits 格式。

**PR 描述示例**（新增一种文档类型）：

```markdown
### 改了什么
新增「API 文档」类型支持，覆盖 REST 接口 / 库 SDK 参考 / OpenAPI 规范三类。

### 为什么改
此前 API 文档被列在「不适用场景」，但手写接口文档是高频需求，应由 skill 覆盖。

### 同步清单
- [x] 新建 references/api-doc.md
- [x] SKILL.md 文档类型识别表
- [x] SKILL.md L3 加载表
- [x] references/modify-doc.md 类型识别表
- [x] README 类型表 + 结构树
- [x] trigger-examples 触发示例
- [x] doc-map.md 边界（与 README/SDK 文档区分）
```

## Issue 规范

- **问题反馈**：说明触发场景（你对 agent 说了什么）、期望产出、实际产出
- **新文档类型提议**：描述这类文档的用途、典型触发语、以及它和现有类型的区别

**Issue 示例**（问题反馈）：

```markdown
**触发场景**：我对 agent 说「帮我写个 AGENTS.md」
**期望产出**：加载 agent-instructions.md，产出含选择建议的指令文件
**实际产出**：agent 没识别出文档类型，按通用流程问了一堆背景
```

## 协作约定

改动文档时遵循 skill 自身的核心规则：

- **最小改动**：只改有问题的部分，不顺手重写无关内容
- **不虚构信息**：拿不准的内容用 `<!-- TODO -->` 标注，不编造
- **每改必有据**：改动指南规范时，说明依据（社区标准 / 实际踩坑）

## 许可证

贡献的内容将采用与项目相同的 MIT 许可证。

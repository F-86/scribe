# pi Skill 编写约定

> 仅在**确认用户使用 pi** 时加载此文件。通用规范见 `skill-spec.md`，多 agent 差异见 `skill-agents.md`。

pi 遵循 [Agent Skills 标准](https://agentskills.io/specification)，沿用在 `skill-spec.md` 中定义的 L1/L2/L3 三层架构，此处只补充 pi 特有的约定。

## L1 front matter 推荐字段

```yaml
---
name: skill-name
description: >-
  功能描述，≤ 3 行，末行为关键词列表。
license: MIT                        # 可选
compatibility: "需要 xxx"           # 可选，有系统依赖时填写
metadata:                           # 可选，建议至少记录 version
  version: "1.0.0"
  author: your-name
allowed-tools: read bash write      # 可选，实验性
disable-model-invocation: true      # 可选，纯手动 Skill 时开启
---
```

> **注意**：pi 不强制 `name` 与文件夹名一致，但本规范强制要求一致。

## L2 必须包含的内容

除通用规范的执行流程和工具调用外，pi 环境下 L2 **必须**包含：

### 1. 环境准备（如有依赖）

```markdown
## 环境准备（仅首次执行）

```bash
cd /path/to/skill && pip install -r requirements.txt
```
```

### 2. 不适用场景

```markdown
## 不适用场景

- 场景 A → 使用 `other-skill` Skill
- 场景 B → 直接用 {工具名} 即可
```

### 3. L3 加载指针（写明工具调用）

不只是写"参照 xxx.md"，而是写清楚用什么工具加载：

```markdown
## 特殊情况

若检测到 {条件}，使用 `read` 工具加载 `references/{file}.md`
```

## 命令调用

```bash
/skill:name                        # 触发
/skill:name arg1 arg2              # 带参数
```

## 文件夹结构

```
{skill-name}/
├── SKILL.md                  # 必须（L1 + L2）
├── references/               # 条件性知识
├── examples/                 # 触发示例
├── scripts/                  # 可执行脚本
└── assets/                   # 模板文件
```

## 快速检查清单

- [ ] `name` 与文件夹名一致，格式合法（`a-z` `0-9` `-`）
- [ ] `description` ≤ 3 行 & ≤ 1024 字符，末行有关键词
- [ ] 有系统依赖时填写 `compatibility`
- [ ] `metadata.version` 已填写
- [ ] L2 包含环境准备步骤（如有依赖）
- [ ] L2 包含不适用场景
- [ ] L2 包含 L3 加载指针（写明工具调用）
- [ ] 所有路径引用相对于 Skill 根目录
- [ ] L3 只放条件性内容

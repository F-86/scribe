# pi Skill 补充约定

> 本文档是 [Skill 设计规范](skill-spec.md) 在 pi 中的具体落地约定。
> 沿用 L1 / L2 / L3 三层架构，此处只补充 pi 特有的字段、规则和实践。

---

## L1 front matter 完整字段

pi 遵循 [Agent Skills 标准](https://agentskills.io/specification)，除 `name` 和 `description` 外支持以下可选字段：

```yaml
---
name: skill-name
description: >-
  功能描述，说清楚此 Skill 的用途。
  关键词：关键词1、关键词2、关键词3
license: MIT                        # 可选，许可证名称或指向 bundled 文件
compatibility: "需要 Python 3.10+, jq, curl"  # 可选，环境要求（≤ 500 字符）
metadata:                           # 可选，自定义键值映射
  version: "1.2.0"
  author: jane
allowed-tools: read bash write      # 可选，预批准的工具列表（实验性）
disable-model-invocation: true      # 可选，true 时不在系统提示中列出，只能 /skill:name 手动触发
---
```

### 各字段说明

| 字段 | 必填 | 约束 | 建议 |
|------|------|------|------|
| `name` | ✅ | 1-64 字符，仅 `a-z` `0-9` `-`，不能以 `-` 开头/结尾，不能连续 `--` | 与文件夹名一致（本规范强制要求） |
| `description` | ✅ | ≤ 1024 字符 | 保持 ≤ 3 行，末行为关键词列表 |
| `license` | 否 | 字符串 | 第三方发布时建议填写 |
| `compatibility` | 否 | ≤ 500 字符 | 有系统依赖时必须填写 |
| `metadata` | 否 | 任意 YAML 键值对 | 建议至少记录 `version` |
| `allowed-tools` | 否 | 空格分隔的工具名 | 实验性功能，慎用 |
| `disable-model-invocation` | 否 | `true` / `false` | 仅调试或纯手动 Skill 时开启 |

### name 格式规范

```
✅ data-lookup
✅ pdf-tools
✅ meeting-summary
❌ DataLookup          # 不能有大写
❌ -pdf                # 不能以 - 开头
❌ pdf--tools          # 不能有连续 --
```

> **注意**：pi 本身不强制 `name` 与文件夹名一致（为了兼容共享技能目录场景），但本规范强制要求一致。不一致时 pi 不会报错，本规范视为不合规。

---

## description 最佳实践

```yaml
# ✅ 好：一句话定位 + 关键词，≤ 3 行
description: >-
  数据资产发现：模糊搜索表名、查询数据源连接信息、查看表结构。
  关键词：找表、表结构、数据源、字段、连接信息、库表、元数据

# ❌ 差：包含触发条件、执行步骤、边界条件（这些应放 L2）
description: >-
  用于数据资产发现。当用户提出以下需求时触发：
  - 模糊搜索表名（"dwd_order 是哪张表"）
  - 查询数据源信息（"数据源 ID 36 是什么"）
  注意：字段血缘请使用 column-explain。

# ❌ 差：过于模糊
description: 数据相关功能。
```

---

## L2 必须包含的内容

除通用规范要求的执行流程、工具调用方式、输出格式外，pi 环境下 L2 必须包含：

### 1. 环境准备（如有依赖）

```markdown
## 环境准备（仅首次执行）

```bash
cd /path/to/skill && pip install -r requirements.txt
```

或

```bash
cd /path/to/skill && npm install
```
```

### 2. 不适用场景

明确告诉 Agent 什么时候**不应该**使用此 Skill，避免误触发：

```markdown
## 不适用场景

- 字段血缘分析 → 使用 `column-lineage` Skill
- 单表简单查询 → 直接用 SQL 工具即可
- 数据写入操作 → 使用 `data-writer` Skill
```

### 3. L3 加载指针（明确工具调用）

不只是写"参照 xxx.md"，而是写清楚**用什么工具加载**：

```markdown
## 特殊情况

### DUMP 依赖处理
若检测到 DUMP 父任务，使用 `read` 工具加载：
`references/dump-dependency.md`

### 执行失败回滚
若迁移失败，使用 `read` 工具加载：
`references/rollback.md`
```

---

## 路径引用约定

Skill 内所有文件引用必须使用**相对于 Skill 根目录**的路径：

```
my-skill/                  # ← Skill 根目录
├── SKILL.md
├── references/
│   └── api.md
└── scripts/
    └── run.sh
```

```markdown
<!-- SKILL.md 内部引用 -->
参照 [API 参考](references/api.md) 获取完整端点列表。

<!-- 执行脚本 -->
```bash
./scripts/run.sh --input data.json
```
```

Agent 的 cwd 是项目根目录而非 Skill 目录，但 pi 会根据 SKILL.md 的位置自动解析相对路径。

---

## 文件夹结构（pi 约定）

```
{skill-name}/
├── SKILL.md                  # 必须（L1 + L2）
├── references/               # 条件性知识
│   ├── {topic}.md
│   └── ...
├── examples/                 # 触发示例（调试/测试用）
│   └── trigger-examples.md
├── scripts/                  # 可执行脚本
│   └── ...
└── assets/                   # 模板文件
    └── ...
```

---

## pi 特有的 Skill 用法

### 命令调用

Skills 自动注册为 `/skill:name` 命令：

```bash
/skill:brave-search                    # 加载并执行
/skill:pdf-tools extract --pages 1-5   # 带参数
```

参数会以 `User: <args>` 追加到 Skill 内容末尾。

### 禁用自动发现

在 `settings.json` 中全局关闭：

```json
{
  "skills": false
}
```

或使用 `--no-skills` CLI 参数。显式指定的 `--skill` 路径不受影响。

---

## pi 验证规则

pi 启动时对 Skills 做以下验证：

| 问题 | 行为 |
|------|------|
| 缺少 `description` | ❌ 不加载 |
| `name` 超 64 字符或格式非法 | ⚠️ 警告，仍加载 |
| `name` 以 `-` 开头/结尾或有连续 `--` | ⚠️ 警告，仍加载 |
| `description` 超 1024 字符 | ⚠️ 警告，仍加载 |
| 未知 front matter 字段 | 忽略，不影响加载 |
| `name` 冲突（多个 Skill 同名） | ⚠️ 警告，保留先发现的 |

---

## 版本管理建议

```yaml
metadata:
  version: "1.2.0"
  changelog: "references/CHANGELOG.md"
```

重大变更时在 SKILL.md 底部记录：

```markdown
## 变更记录

### v1.2.0 (2026-06-15)
- 新增 DUMP 依赖处理流程（L2）
- 将合规检查拆分到 L3 `references/compliance.md`
```

---

## 快速检查清单（pi 完整版）

- [ ] `name` 与文件夹名一致，格式合法（`a-z` `0-9` `-`）
- [ ] `description` ≤ 3 行 & ≤ 1024 字符，末行有关键词
- [ ] 有系统依赖时填写 `compatibility`
- [ ] `metadata.version` 已填写
- [ ] L2 包含环境准备步骤（如有依赖）
- [ ] L2 包含不适用场景
- [ ] L2 包含 L3 加载指针（写明用 `read` 工具）
- [ ] 所有路径引用相对于 Skill 根目录
- [ ] L3 只放条件性内容

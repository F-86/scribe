# SKILL.md 编写指南

## 定位

SKILL.md 是 AI agent 的"技能包"入口文件，定义了一个可被 agent 按需加载的专项能力模块。它遵循三层架构（L1/L2/L3），使 agent 能高效判断"是否触发"和"如何执行"。

## 完整规范引用

编写 SKILL.md 时，**必须**参照完整的 Skill 设计规范：

- **通用规范**：`references/skill-spec.md` — 三层架构的核心定义
- **pi 补充约定**：`references/pi-skill-spec.md` — pi 环境的额外字段和约定

> 加载方式：使用 `read` 工具加载上述文件。

## 结构模板（pi 版）

```markdown
---
name: skill-name
description: >-
  简要描述此 Skill 的用途，一句话说清核心功能。
  关键词：关键词1、关键词2、关键词3
license: MIT
compatibility: "需要 xxx"
metadata:
  version: "0.1.0"
  author: your-name
---

# {Skill 名称}

## 环境准备（仅首次执行）

```bash
...
```

## 执行流程

1. 步骤一
2. 步骤二
3. 步骤三

## 核心规则

- 规则一
- 规则二

## 输出格式

```markdown
## {标题}
...
```

## 不适用场景

- 场景 A → 请使用 `other-skill` Skill

## 特殊情况

- 若检测到 {条件}，使用 `read` 工具加载 `references/{file}.md`
```

## 三层架构快速参考

```
L1 — Front matter（YAML）
     AI 用 description 判断"是否触发"
     原则：≤ 3 行，只写核心定位 + 关键词

L2 — SKILL.md 主体（Markdown）
     执行此 Skill 时必然用到的所有内容
     原则：长度不限，完整准确，每次必用

L3 — references/ / examples/ / scripts/ / assets/
     只在特定情境下用到的条件性内容
     原则：按需加载
```

**L2 vs L3 唯一判断标准**：执行此 Skill 时，这段内容**每次都会用到**吗？

## 编写重点

### L1 — description 怎么写

```yaml
# ✅ 好
description: >-
  数据资产发现：模糊找表、查数据源连接信息、查表结构和字段。
  关键词：找表、表结构、数据源、字段、连接信息、库表、元数据

# ❌ 太重（包含了触发条件和边界）
description: >-
  用于数据资产发现。当用户提出以下需求时触发：
  - 模糊搜索表名
  - 查询数据源信息
  注意：字段血缘请使用 column-explain。
```

### L2 — 必须包含的元素

1. **执行流程**：步骤顺序、分支决策
2. **工具调用**：工具名、参数说明、调用示例
3. **核心规则与约束**
4. **输出格式模板**
5. **不适用场景**：明确说明什么时候不该用此 Skill
6. **L3 加载指针**：何时加载哪个 L3 文件（写明用 `read` 工具）

### L3 — 放什么

| 放 | 不放 |
|----|------|
| 特定场景的领域规范 | 每次必用的工具调用格式 |
| 错误排查/回滚手册 | 核心判断规则 |
| 触发示例（调试用） | 输出格式模板 |
| 可执行脚本、模板文件 | 必经流程的步骤 |

## 常见错误

| 错误 | 表现 | 修复 |
|------|------|------|
| L1 过重 | description 写了触发条件、执行步骤 | 只保留核心定位 + 关键词 |
| L2 过轻 | 主体只有几行指针，核心逻辑全在 references | 把每次必用的移回 SKILL.md |
| L2/L3 混淆 | "每次必用"的规则放进 references | 移回 SKILL.md 主体 |
| name 不匹配 | front matter 的 name 和文件夹名不同 | 以文件夹名为准 |
| 缺不适用场景 | Agent 容易在不该用的场景也触发 | 加独立的不适用场景章节 |
| L3 加载条件模糊 | "参照 xxx.md"没写何时触发 | 写清楚条件 + 用什么工具加载 |

## 完整示例

参见本 skill 的 `SKILL.md`（L1+L2 完整示例）及 `references/skill-spec.md`（完整规范）。

# ADR-0002: frontmatter 通用化,去除 agent 专属字段

> 状态:已接受 · 日期:2026-06-16 · 决策者:项目维护者

## 背景与问题

scribe 早期的 `SKILL.md` frontmatter 包含 `compatibility`、`metadata` 等字段,这些字段绑定特定 agent 的私有约定。随着目标从"给某个 agent 用"扩展到"兼容 pi / Claude Code / Codex 等任何支持 Agent Skills 标准的工具",这些专属字段成了跨工具兼容的障碍——某工具不认识的字段可能被忽略,也可能报错。

## 决策

frontmatter 只保留三个通用字段:`name`、`description`、`license`。移除所有 agent 专属字段。L2/L3 中涉及工具调用的描述也通用化,不假设特定 agent 的工具名称。

## 理由

- **最大化兼容**:只用各工具都认的字段,一份 skill 到处可用
- **降低维护成本**:不必为每个 agent 维护一套字段
- **专属约定下沉**:确有 agent 差异时,放到 `skill-pi.md` / `skill-claude.md` 等 L3 按需处理,而非顶在 frontmatter

## 备选方案

- **保留专属字段 + 多份 frontmatter**:为每个 agent 维护一份。维护成本高,且违背"单一 skill 跨工具"的目标,否决。
- **保留字段但标注可选**:仍可能在严格解析的工具上出问题,且 L1 越重越难维护,否决。

## 后果

- ✅ 同一份 scribe 可在 pi / Claude Code / Codex 直接使用
- ✅ L1 更轻,聚焦"触发判断"本职
- ⚠️ agent 特有能力无法在 L1 声明,只能在 L3 中按 agent 分支处理(见 `references/skill-pi.md`、`skill-claude.md`)

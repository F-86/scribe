# 常见问题

## 使用

### scribe 能写代码吗?
不能,也不该用它写代码。scribe 只负责文档。写业务逻辑直接写代码,不需要本 skill。

### 它和 TypeDoc / JSDoc / Sphinx 有什么区别?
那些工具从代码注释**全量自动生成** API 文档;scribe 负责**手写/整理**的文档(精选接口 + 叙述说明 + 真实示例)。两者互补:用 scribe 写概览页,链接到自动生成的全量参考。

### 支持哪些 AI 工具?
任何支持 Agent Skills 标准的工具,已验证 pi、Claude Code、Codex。frontmatter 只用通用字段以保证兼容(原因见 [ADR-0002](adr/0002-vendor-neutral-frontmatter.md))。

### 我要写的文档类型 scribe 没覆盖怎么办?
scribe 会回退到 `references/general-principles.md`(通用写作原则)。如果是高频需求,建议按 [CONTRIBUTING](../CONTRIBUTING.md) 新增一类 L3 规范。

## 写作行为

### 为什么 scribe 写 SKILL.md 时,不确定给哪个 agent 就不加专属字段?
这是刻意设计的安全默认值:不确定环境时,加了某 agent 的专属字段,到别的工具可能失效甚至报错。所以不确定时只输出通用内容(name + description + 通用 L2),宁可少写不可写错。

### 为什么生成的文档里有 `<!-- TODO -->`?
scribe 不虚构信息。项目名、接口字段、联系邮箱等它无法确认的内容,会标 TODO 让你补,而不是编一个看起来对、实际错的值。

### 为什么它改文档前先列"诊断清单"而不直接改?
修改模式遵循"先诊后治":先列出问题让你确认改哪些,避免它自作主张大改。详见 `references/modify-doc.md`。

## 贡献

### 新增一类文档,为什么要改那么多地方?
因为 L2 的路由依赖多张表。新增一类需同步 5 处(SKILL.md 的识别表 + 加载表、modify-doc.md、README、trigger-examples)。这是三层架构按需加载换来的代价(见 [ARCHITECTURE](ARCHITECTURE.md))。CONTRIBUTING 里有完整的"5 处同步"清单照着做即可。

### 怎么验证我的改动没问题?
scribe 是纯文档项目,没有自动化测试。验证靠四项人工检查:路由自检、边界自检、同步自检、成品检查。详见 [CONTRIBUTING](../CONTRIBUTING.md)。

# CLAUDE.md 编写指南

## 定位

CLAUDE.md 是放在项目根目录下、供 AI agent（Claude Code、Codex、pi 等）在会话中加载的项目上下文文件。它告诉 agent：这个项目是什么、技术栈、编码约定、常用命令，以及如何处理常见任务。

## 结构模板

```markdown
# {项目名称}

## 项目概述
<!-- 2-3 句话说清楚项目做什么、解决什么问题 -->

## 技术栈
- 语言/框架：...
- 数据库：...
- 关键依赖：...

## 目录结构
<!-- 只需列出关键目录，标注用途 -->
```
src/         — 源代码
tests/       — 测试
docs/        — 文档
```

## 常用命令
```bash
npm run dev        # 启动开发服务器
npm test           # 运行测试
npm run lint       # 代码检查
npm run build      # 构建
```

## 编码约定
<!-- 核心约定，不是全部规则 -->
- 使用 TypeScript 严格模式
- 函数命名：动词开头（fetchUser, saveOrder）
- 优先使用 async/await 而非原始 Promise
- ...

## 重要提示
<!-- agent 容易踩的坑、项目特有的注意事项 -->
- 数据库迁移必须通过 migration 工具，不要直接改表
- 配置文件在 config/ 目录，修改后需重启
- ...
```

## 编写原则

### DO ✅

- **简短**：agent 每次会话都会加载，每 KB 都是 token 成本。控制在 200 行以内
- **可执行**：写具体命令而非抽象描述。`npm run test:unit` 而非 "运行单元测试"
- **只写约定**：不要写代码示例，不要写文档——agent 会自己查
- **优先高频信息**：最常用的命令、最易踩的坑放前面
- **区分 agent 受众**：写给 AI 看，不是写给人类看

### DON'T ❌

- **不要写历史背景**：除非对当前开发有关键影响
- **不要列依赖版本号**：`package.json` 里已有
- **不要重复 README 内容**：README 给人类看，CLAUDE.md 给 AI 看，重点不同
- **不要写教程式的长段落**：用短列表、短命令，直接了当

## 典型大小

| 项目规模 | 建议长度 |
|---------|---------|
| 小型个人项目 | 30-60 行 |
| 中型团队项目 | 60-120 行 |
| 大型 monorepo | 120-200 行 |

超过 200 行时，考虑拆分为多个文件（如 `CLAUDE.md` + `.claude/rules/` 或 `rules/` 目录下的专题文件）。

## 常见章节取舍

- **项目概述** — 必须。2-3 句，agent 需要知道在什么项目里
- **技术栈** — 必须。agent 需要知道用什么语言/框架
- **目录结构** — 建议。只列关键目录（src/tests/docs/config），不列完整树
- **常用命令** — 必须。agent 最常做的事就是运行命令
- **编码约定** — 建议。选 5-10 条最重要的约定
- **测试说明** — 如有复杂测试流程，单列一节
- **部署/CI** — 如有非标准流程，简要说明
- **安全注意事项** — 仅在有硬性安全要求时添加
- **重要提示** — 建议。agent 容易踩的坑放这里

## 示例（精简版）

```markdown
# todo-app

Express + SQLite 待办事项 API，支持用户认证。

## 技术栈
- Node.js 20+, Express 4, SQLite (better-sqlite3)
- 测试：vitest
- 认证：JWT + bcrypt

## 目录
```
src/
  routes/      — 路由处理
  middleware/   — 认证、验证
  db/          — 数据库初始化 & 迁移
tests/         — 测试，与 src 结构对应
```

## 常用命令
```bash
npm run dev       # 开发模式（端口 3000，热重载）
npm test          # 全部测试
npm run test:unit # 仅单元测试
npm run migrate   # 数据库迁移（需先 npm run build）
```

## 编码约定
- 路由文件一个资源一个文件：`routes/todos.js`、`routes/users.js`
- 数据库查询写在 `db/queries/` 目录，不在路由里直接写 SQL
- 所有 API 返回 JSON，错误格式 `{ error: string }`
- HTTP 状态码遵循 REST 惯例

## 重要提示
- JWT secret 从环境变量 `JWT_SECRET` 读取，本地开发用 `.env` 文件
- SQLite 文件在 `data/app.db`，不要手动编辑
- 测试用内存数据库 (`:memory:`)，不影响开发数据
```

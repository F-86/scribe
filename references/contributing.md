# CONTRIBUTING.md 编写指南

## 定位

CONTRIBUTING.md 是贡献者入口，告诉外部开发者：如何参与、用什么工具、遵守什么规范。它减少新贡献者的摩擦。

> 与相邻文档的边界见 [`doc-map.md`](doc-map.md)：CONTRIBUTING 讲贡献**全流程**;架构原理归设计文档;提交信息的格式细则若已有独立的提交规范文档,这里只**链接过去**、不重复展开类型表与示例(单一信息源)。

## 结构模板

```markdown
# 贡献指南

## 准备工作

### 环境要求
- Node.js >= 18
- pnpm（全局安装 `npm i -g pnpm`）

### 克隆并安装
```bash
git clone https://github.com/owner/repo
cd repo
pnpm install
```

## 开发流程

### 分支策略
- `main` — 稳定版本，禁止直接提交
- `feat/xxx` — 新功能
- `fix/xxx` — Bug 修复

### 开发
```bash
pnpm dev              # 启动开发环境
pnpm test -- --watch  # 监听模式测试
```

### 提交前检查
```bash
pnpm lint             # 代码检查
pnpm test             # 运行全部测试
pnpm build            # 确认构建通过
```

## 提交规范

使用 [Conventional Commits](https://www.conventionalcommits.org/)：
```
feat: 添加用户注销功能
fix: 修复 token 过期未刷新的问题
docs: 更新 API 文档
refactor: 重构认证中间件
test: 添加注销功能的单元测试
```

## 代码规范

- 使用 TypeScript 严格模式
- 文件名：kebab-case（`user-auth.ts`）
- 函数命名：动词开头（`getUserById`, `createToken`）
- 超过 3 个参数使用对象参数
- 所有公开 API 必须有 JSDoc 注释

## Pull Request 流程

1. Fork 本仓库
2. 创建功能分支
3. 编写代码并自测
4. 提交 PR，描述变更内容和原因
5. 等待 CI 通过和 review

PR 标题遵循 Conventional Commits 格式。

**PR 描述示例**：
```markdown
### 变更内容
为登录接口添加 token 自动刷新。

### 变更原因
token 过期后用户被强制登出，体验差（见 #123）。

### 自测
- [x] pnpm test 全部通过
- [x] 手动验证 token 过期后能无感刷新
```

## Issue 规范

- Bug 报告：包含复现步骤、预期行为、实际行为、环境信息
- 功能请求：描述使用场景和期望的 API 形态

**Bug 报告示例**：
```markdown
**复现步骤**：1. 登录 2. 等待 token 过期 3. 点击任意操作
**预期行为**：自动刷新 token，操作正常完成
**实际行为**：跳回登录页
**环境**：v1.2.0 / Chrome 120 / macOS 14
```

## 许可证

贡献的代码将采用与项目相同的许可证。
```

## 编写原则

### DO ✅

- 每步命令必须可执行（用户复制粘贴就能跑）
- 明确分支命名和提交格式（减少 review 摩擦）
- 解释"为什么"有这条规则，不只是"是什么"
- 提供失败时的排查指引（如 "如果 pnpm install 失败，确认 Node 版本 ≥ 18"）

### DON'T ❌

- 不要重复 README 的项目介绍
- 不要只写"遵循社区惯例"——说出具体惯例
- 不要假设贡献者使用相同的 IDE/编辑器
- 不要写太长的代码规范——选 5-10 条最常违反的

## 章节取舍指南

| 章节 | 团队项目 | 个人项目 | 开源项目 |
|------|---------|---------|---------|
| 环境要求 | 必须 | 必须 | 必须 |
| 克隆并安装 | 必须 | 必须 | 必须 |
| 开发流程 | 必须 | 可选 | 必须 |
| 提交规范 | 必须 | 可选 | 必须 |
| 代码规范 | 必须 | 可选 | 建议 |
| PR 流程 | 必须 | 不需要 | 必须 |
| Issue 规范 | 建议 | 不需要 | 建议 |
| 行为准则 | 大团队 | 不要 | 大项目 |

## 针对不同项目类型的调整

### 前端项目
额外说明：UI 组件库使用、设计 token 位置、Storybook 开发流程

### CLI 工具
额外说明：如何全局安装开发版本（`npm link`）、测试命令的输出验证方式

### Monorepo
额外说明：workspace 结构、跨 package 开发、每个子包的单独开发命令

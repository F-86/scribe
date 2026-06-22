# README 编写指南

## 定位

README 是项目的第一印象，回答三个问题：这是什么？怎么用？怎么参与？

> 与相邻文档的边界见 [`doc-map.md`](doc-map.md)：README 讲"怎么用"，**为什么这么设计**归设计文档，**改了什么**归 CHANGELOG，架构细节不要塞进 README。

## 结构模板

```markdown
# {项目名称}

<!-- 一句话描述 + 徽章（CI、npm version、license 等） -->

## 特性
<!-- 3-5 个核心特性，用简短的列表 -->

## 快速开始

### 安装
```bash
npm install {package-name}
```

### 使用
```javascript
// 最小可运行示例
```

## 文档
<!-- 指向详细文档的链接 -->

## 贡献
<!-- 简要说明贡献流程，指向 CONTRIBUTING.md -->

## 维护与联系
<!-- 谁维护、出问题/求助找哪个入口：Issue、邮箱、安全上报指向 SECURITY.md -->

## 许可证
<!-- MIT / Apache-2.0 / ... -->
```

## 编写原则

### 受众分层

README 要为三类读者服务：

1. **路人（5 秒）**：标题 + 一句话描述 + 徽章，快速判断"这跟我的问题有关吗？"
2. **初级用户（5 分钟）**：快速开始 + 最小示例，能跑起来
3. **深度用户（30 分钟）**：文档链接、API 参考、高级用法

每部分内容要服务对应的受众层级。不要把深度用户的详细 API 文档塞在 README 里。

### DO ✅

- 先写"做了什么"再写"怎么用"
- 安装和快速开始的命令必须能直接运行
- 最小示例要真实可用（不依赖隐式上下文）
- 用徽章直观展示 CI、版本、下载量等状态
- 文档长时加目录（GitHub 会自动生成 TOC）

### DON'T ❌

- 不要写"强大的"、"快速的"等无意义形容词——用数据和特性说话
- 不要把 README 写成 API 参考手册——另建 docs/ 目录
- 不要假设读者已经了解项目背景
- 不要放过大的截图（影响 GitHub 加载速度）

## 章节取舍指南

| 章节 | 何时需要 | 何时可省 |
|------|---------|---------|
| 徽章 | 始终 | 无 |
| 特性 | 始终，替代空洞的简介 | 无 |
| 快速开始 | 始终 | 无 |
| 安装 | 可独立安装时 | 纯文档项目 |
| 使用 | 始终 | 无 |
| 配置 | 有配置文件或环境变量时 | 零配置项目 |
| API | 小型库（< 20 个方法） | 大型库 → 单独 docs/ |
| 文档链接 | 有独立文档站时 | 无 |
| 贡献 | 接受外部贡献时 | 个人项目可省，指向 CONTRIBUTING.md |
| 维护与联系 | 开源/对外项目 | 内部项目可省 |
| 常见问题 | 有重复出现的问题时 | 问题少或不成熟的项目 |
| 许可证 | 始终 | 无 |

## 最小可运行示例的写法

```javascript
// ✅ 好：真实可运行，包含 import + 关键 API
import { createTodo, listTodos } from 'todo-lib'

const todo = await createTodo({ title: 'Buy milk', done: false })
console.log(todo) // { id: 1, title: 'Buy milk', done: false, createdAt: ... }

const todos = await listTodos()
console.log(todos.length) // 1

// ❌ 差：伪代码或过于简化
todo = Todo.create()
todo.save()
# 没有 import，不知道从哪来的 Todo 对象
```

规则：
- 包含 import/require
- 输出有明确的预期结果（或用注释标注 `// => Result`）
- 不超过 15 行，一眼能看完
- 不依赖数据库连接等外部环境（用 mock 或注明前提）

## 示例（精简版）

```markdown
# todo-app

[![CI](https://github.com/user/todo-app/actions/workflows/ci.yml/badge.svg)](https://github.com/user/todo-app/actions)
[![npm](https://img.shields.io/npm/v/todo-app)](https://www.npmjs.com/package/todo-app)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

Express + SQLite 待办事项 API，支持 JWT 认证。

## 特性
- RESTful API（todos, users）
- JWT 认证 + bcrypt 密码哈希
- SQLite 零配置数据库
- 完整的测试覆盖

## 快速开始

### 安装
```bash
git clone https://github.com/user/todo-app
cd todo-app && npm install
cp .env.example .env    # 编辑 JWT_SECRET
```

### 使用
```bash
npm run dev             # 启动 API (http://localhost:3000)
curl localhost:3000/health  # 健康检查
```

## 文档
API 完整文档见 [docs/API.md](docs/API.md)

## 禁止行为

- **禁止**在 README 中列出安装步骤但没有说明依赖环境（如 Node 版本、数据库）
- **禁止**使用"简单"、"易用"、"强大"等无客观标准的形容词
- **禁止**在标题中使用全大写（如 "INTRODUCTION"、"INSTALLATION"）
- **禁止**把架构设计、API 细节、变更日志塞进 README

## 许可证
MIT
```

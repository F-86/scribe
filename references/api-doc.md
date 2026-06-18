# API 文档编写指南

## 定位

API 文档告诉调用方：有哪些接口、怎么调、传什么、返回什么、出错怎么办。读者是**集成方开发者**——他们不读源码，只照文档调用，文档不准就会调错。

API 文档分三类，写法差异很大，**动笔前先确认是哪一类**：

| 类型 | 信号 | 形态 |
|------|------|------|
| **REST/HTTP 接口文档** | "接口文档"、"REST API"、"endpoint"、"后端接口"、`docs/API.md` | 人读的 Markdown，按资源列 endpoint |
| **库/SDK API 参考** | "SDK 文档"、"库的 API"、"函数签名手册"、"方法说明" | 人读的 Markdown，按模块列函数/类 |
| **OpenAPI/Swagger 规范** | "OpenAPI"、"swagger"、`openapi.yaml`、"接口规范文件" | 机器可读的 YAML/JSON，供工具渲染 |

> **与自动生成工具的边界**：本指南覆盖**手写/整理**的 API 文档（精选公开接口 + 叙述性说明 + 真实示例）。若需求是「把代码里全部带注释的符号导出成文档」，那是自动生成场景，交给 TypeDoc / JSDoc / Sphinx / Doxygen，不用手写。

---

## 一、REST/HTTP 接口文档

### 结构模板

```markdown
# {服务名} API

Base URL: `https://api.example.com/v1`

## 认证
所有请求需在 Header 携带 Bearer Token：
`Authorization: Bearer <token>`

## 通用约定

### 状态码
| 码 | 含义 |
|----|------|
| 200 | 成功 |
| 201 | 创建成功 |
| 400 | 请求参数错误 |
| 401 | 未认证 / token 失效 |
| 404 | 资源不存在 |
| 429 | 触发限流 |

### 错误响应体
```json
{ "error": { "code": "INVALID_PARAM", "message": "title 不能为空" } }
```

### 分页
列表接口支持 `?page=1&limit=20`，响应含 `total` 字段。

## Todos

### 创建待办
`POST /todos`

请求体：
| 字段 | 类型 | 必填 | 说明 |
|------|------|------|------|
| title | string | 是 | 待办标题 |
| done | boolean | 否 | 默认 false |

示例：
```bash
curl -X POST https://api.example.com/v1/todos \
  -H "Authorization: Bearer <token>" \
  -H "Content-Type: application/json" \
  -d '{"title": "Buy milk"}'
```
响应 `201`：
```json
{ "id": 1, "title": "Buy milk", "done": false, "createdAt": "2026-06-18T10:00:00Z" }
```

### 获取待办列表
`GET /todos?page=1&limit=20`
...
```

### 编写原则

**DO ✅**
- 每个 endpoint 都给可直接运行的 `curl`（含真实 Header）+ 真实 JSON 响应
- 认证、状态码、错误体、分页等**通用约定提到顶部统一写一次**，不在每个接口重复
- 参数用表格列出：字段、类型、必填、说明——缺一不可
- 按资源分组（Todos、Users…），不按"GET/POST 动词"分组
- 区分请求体字段、查询参数、路径参数（`/todos/{id}` 里的 `id`）

**DON'T ❌**
- 不要只写"返回用户信息"——给出完整响应 JSON 的字段结构
- 不要漏掉错误情况——至少覆盖该接口特有的 4xx
- 不要用占位 token `xxx` 而不说明从哪获取
- 不要把内部实现细节（数据库表名、内部服务调用）写进对外文档

### 章节取舍

| 章节 | 何时需要 | 何时可省 |
|------|---------|---------|
| 认证 | 有鉴权时（多数） | 完全公开的 API |
| 通用约定（状态码/错误体） | 始终 | 无 |
| 分页 | 有列表接口时 | 无列表 |
| 限流 | 有速率限制时 | 内部低频 API |
| Webhook / 回调 | 有异步回调时 | 纯同步 API |
| 版本与弃用 | 对外长期维护的 API | 内部短期接口 |

---

## 二、库/SDK API 参考

### 结构模板

```markdown
# {库名} API 参考

## 安装
```bash
npm install todo-lib
```

## 导入
```javascript
import { createTodo, listTodos } from 'todo-lib'
```

## todos

### createTodo(options)
创建一条待办并返回完整对象。

```typescript
function createTodo(options: { title: string; done?: boolean }): Promise<Todo>
```

**参数**
| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| options.title | string | 是 | 待办标题 |
| options.done | boolean | 否 | 完成状态，默认 false |

**返回**：`Promise<Todo>` — 含自动生成的 `id` 和 `createdAt`

**抛出**：`ValidationError` — title 为空时

**示例**
```javascript
const todo = await createTodo({ title: 'Buy milk' })
console.log(todo) // { id: 1, title: 'Buy milk', done: false, createdAt: ... }
```
```

### 编写原则

**DO ✅**
- 每个条目固定五件套：**签名 + 一句话说明 + 参数表 + 返回值 + 示例**；有异常再加"抛出"
- 签名用带类型的代码块（TypeScript / Python 类型注解），一眼看清入参出参
- 示例真实可运行：含 import、有预期输出注释
- 按模块/功能分组，公开 API 在前、内部工具在后
- 标注版本信息（`@since 2.1.0`）和弃用（`@deprecated 改用 xxx`）

**DON'T ❌**
- 不要把私有/内部函数混进公开参考
- 不要只写类型不写语义——`id: number` 要说明"自增主键"
- 不要让示例依赖未展示的隐式上下文
- 不要为了凑全量而罗列每个 getter/setter——精选真正会被调用的 API，全量交给自动生成工具

### 手写 vs 自动生成

| 场景 | 选择 |
|------|------|
| 精选公开 API + 叙述性说明 + 上手示例 | **手写**（本指南） |
| 导出全部带注释符号、随代码同步更新 | TypeDoc / JSDoc / Sphinx |
| 两者结合：手写概览页 + 链接到自动生成的全量参考 | 推荐做法 |

---

## 三、OpenAPI/Swagger 规范

这是**机器可读**的接口契约文件（YAML 或 JSON），不是给人逐行读的——它由 Swagger UI / Redoc 渲染成可交互文档，也能驱动 SDK 生成和 mock。

### 最小可用骨架（OpenAPI 3.1）

```yaml
openapi: 3.1.0
info:
  title: Todo API
  version: 1.0.0
  description: 待办事项管理接口
servers:
  - url: https://api.example.com/v1
paths:
  /todos:
    post:
      summary: 创建待办
      security:
        - bearerAuth: []
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/TodoInput'
      responses:
        '201':
          description: 创建成功
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Todo'
        '400':
          description: 参数错误
components:
  securitySchemes:
    bearerAuth:
      type: http
      scheme: bearer
  schemas:
    TodoInput:
      type: object
      required: [title]
      properties:
        title: { type: string }
        done: { type: boolean, default: false }
    Todo:
      type: object
      properties:
        id: { type: integer }
        title: { type: string }
        done: { type: boolean }
        createdAt: { type: string, format: date-time }
```

### 编写原则

**DO ✅**
- 复用 `components/schemas`，用 `$ref` 引用，不重复定义同一个数据结构
- 认证统一定义在 `securitySchemes`，各 endpoint 用 `security` 引用
- 每个响应码都写 `description`；成功响应给出 `schema`
- 用工具校验：`npx @redocly/cli lint openapi.yaml` 或 swagger-cli
- 用 `examples` 提供真实示例值，渲染出来更直观

**DON'T ❌**
- 不要手写一份 YAML 又手写一份 Markdown 接口文档——选 OpenAPI 后用 Redoc 渲染，单一信息源
- 不要 inline 重复每个 schema——抽到 components
- 不要漏 `required` 数组——调用方靠它区分必填

### 渲染与校验

```bash
# 渲染为静态 HTML 文档
npx @redocly/cli build-docs openapi.yaml -o api.html

# 校验规范是否合法
npx @redocly/cli lint openapi.yaml
```

---

## 通用规则（三类共用）

- **不虚构接口**：不确定的字段、状态码、错误码用 `<!-- TODO: 确认 xxx -->` 标注，绝不编造
- **示例必须真实**：示例中的字段名、类型要与参数表一致，不能对不上
- **优先从已有素材提取**：有路由代码、类型定义、现有 OpenAPI 文件时，从中提取，不凭空写
- **保持单一信息源**：同一接口不要在多处用不同格式重复描述，避免维护时不同步

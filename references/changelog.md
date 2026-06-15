# CHANGELOG 编写指南

## 定位

CHANGELOG 是按时间倒序记录项目每个版本变更的文档。目标读者是升级用户：他们会对照着看「从我的版本到现在，改了什么？有什么 Breaking Change？」

## 格式：Keep a Changelog

遵循 [Keep a Changelog](https://keepachangelog.com/) 规范：

```markdown
# Changelog

所有对本项目的显著变更均将记录在此文件中。

格式基于 [Keep a Changelog](https://keepachangelog.com/zh-CN/1.1.0/)，
版本号遵循 [Semantic Versioning](https://semver.org/lang/zh-CN/)。

## [Unreleased]

### Added
- 新增功能 A

### Changed
- 修改了功能 B 的默认行为

### Deprecated
- 功能 C 将在 v3.0 移除

### Removed
- 移除了功能 D

### Fixed
- 修复了功能 E 的 Bug

### Security
- 修复了安全漏洞 F

## [1.2.0] - 2026-06-15

### Added
- 导出 PDF 功能，支持自定义模板
- CLI 新增 `--output` 参数

### Fixed
- 修复大文件处理内存溢出 (#123)

## [1.1.0] - 2026-05-20

### Added
- 用户认证模块

### Changed
- 最低 Node.js 版本要求提升至 18

## [1.0.0] - 2026-05-01

### Added
- 首个正式版本发布
```

## 变更类型

一个版本包含以下分类（按此顺序）：

| 类型 | 含义 | 示例 |
|------|------|------|
| `Added` | 新增功能 | "新增 CSV 导出" |
| `Changed` | 现有功能的变更 | "登录接口返回格式变更" |
| `Deprecated` | 即将移除的功能 | "`getUser` 已弃用，请用 `fetchUser`" |
| `Removed` | 已移除的功能 | "移除对 Node 14 的支持" |
| `Fixed` | Bug 修复 | "修复并发写入时的数据竞争" |
| `Security` | 安全修复 | "修复 SQL 注入漏洞" |

不涉及的分类直接省略，不要写「无」或空列表。

## 编写原则

### DO ✅

- 每条用一句话说清：改了什么 + 对用户的影响
- 关联 Issue/PR 编号（如 `(#123)`）
- Breaking Change 用 **粗体** 强调
- 日期使用 `YYYY-MM-DD` 格式
- 保持最新版本在最上面

### DON'T ❌

- 不要记录内部重构（不影响用户行为的代码清理）——用户不关心
- 不要写"修复了一些 Bug"——列出具体的
- 不要把 commit log 照搬过来——CHANGELOG 是给人看的摘要
- 不要忘记记录 Breaking Change

## 什么应该记录

```
✅ 记录：
- API 接口新增/修改/删除
- CLI 参数变更
- 配置文件格式变更
- 最低支持版本变更
- 性能显著提升
- 安全漏洞修复

❌ 不用记录：
- 代码重构（行为不变）
- 注释/文档更新（除非是重大文档重构）
- 测试补充
- 依赖小版本更新
- CI 配置变更
```

## 示例（条目级）

```markdown
# ✅ 好
### Added
- 新增 `--dry-run` 参数，预览操作但不实际执行 (#234)

### Changed
- **Breaking:** 配置文件从 JSON 迁移到 YAML 格式，旧配置需手动转换 (#300)

### Fixed
- 修复 Windows 下路径分隔符问题，`load()` 现在正确解析反斜杠 (#245)

# ❌ 差
### Changed
- 改了一些东西
- 更新了依赖

### Fixed
- 修 Bug
```

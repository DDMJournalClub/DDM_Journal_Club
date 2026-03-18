# Task Plan: 修复日程安排为空问题 ✅ **已完成**

## 问题描述
日程安排页面（schedule.html）显示为空，"即将进行/进行中"和"历史日程"都为空。

## 问题分析

### 第 1 轮：limit_posts 限制
- **`limit_posts: 10`** - Jekyll 配置只加载最近 10 个 posts
- **修复**：改为 `limit_posts: 100` ✅

### 第 2 轮：Liquid where_exp 语法错误
GitHub Actions 报错：
```
Liquid syntax error (line 23): Expected end_of_string but found id
```
- **根本原因**：Jekyll 3.10.0 的 `where_exp` 不支持复杂的 `or` 复合条件
- **修复**：改用 `for + if` 模式 ✅

### 第 3 轮：exclude 递归排除（主因）
GitHub Actions 日志：
```
EntryFilter: excluded /_posts/2022-08-09-mental-speed.md
...
```
- **根本原因**：`exclude: "*.md"` 递归排除了所有 posts
- **修复**：改为具体文件路径 `/README.md` 等 ✅

## 最终修复方案

### 修改的文件
| 文件 | 修改内容 |
|------|----------|
| `_config.yml` | `limit_posts: 10` → `100` |
| `_config.yml` | `exclude: "*.md"` → `/README.md` 等具体路径 |
| `schedule.html` | 改用 for + if 模式 |
| `index.html` | 改用 for + if 模式 |

### 已实施的修复
- [x] `_config.yml`: `limit_posts: 10` → `100`
- [x] `_config.yml`: 修复 exclude 配置
- [x] `schedule.html`: 改用 for + if 模式
- [x] `index.html`: 改用 for + if 模式

## 验证结果 ✅
- [x] GitHub Pages 构建成功
- [x] 日程显示正常
- [x] 即将进行/进行中正确显示
- [x] 历史日程正确显示并可点击跳转
- [x] 首页最新推送正常显示

## 状态
**✅ 已完成并验证通过**

## 关键教训
1. Jekyll `exclude` 模式是递归的 - `"*.md"` 会排除所有子目录的 .md 文件
2. `where_exp` 不支持复合条件 - 使用 `for + if` 更可靠
3. 查看 EntryFilter 日志 - 快速定位文件排除问题

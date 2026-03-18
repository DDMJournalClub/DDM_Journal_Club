# 进度日志

## 2026-03-18 - Jekyll 配置最终确认

### ✅ 问题根源
之前尝试了多种方案（自定义 GitHub Actions、复杂的 Jekyll 配置等），但都遇到了各种构建错误，主要问题是：
- 自定义 Actions 构建步骤过于复杂
- Jekyll 插件配置与 GitHub Pages 原生支持不匹配
- `relative_url` 过滤器与路径组合产生歧义 URI

### ✅ 最终解决方案（基于 commit 0e65994）

**1. 使用 GitHub Pages 官方 Jekyll 工作流**
- 放弃自定义复杂的 Actions 配置
- 改用 `actions/jekyll-build-pages@v1` - GitHub 官方预配置 Jekyll 构建
- 简化 `.github/workflows/jekyll.yml` 为标准模板

**2. _config.yml 关键修复**
```yaml
# 新增 Gemfile 依赖管理
plugins:
  - jekyll-feed
  - jekyll-seo-tag
  - jekyll-sitemap
  - jekyll-paginate

# 添加更多排除项，避免构建错误
exclude:
  - vendor/
  - .bundle/
  - "*.md"  # 排除根目录辅助 md 文件
  # ... 其他排除项
```

**3. 新增 Gemfile**
使用标准 Bundler 依赖管理，确保 GitHub Actions 能正确安装 Jekyll 和插件：
```ruby
source "https://rubygems.org"
gem "jekyll", "~> 3.9"
group :jekyll_plugins do
  gem "jekyll-theme-primer"
  gem "jekyll-feed"
  # ... 其他插件
end
```

**4. 保留的有效配置**
- Permalink 格式: `/:year-:month-:day-:slug/`（日期+主题缩写）
- HTML 页面显式 permalink: `/schedule/`, `/papers/`, `/resources/`
- 图片路径使用带前导 `/` 的绝对路径

---

## 历史会话记录

### 会话 1-2
- ✅ 创建了规划文件: `task_plan.md`, `findings.md`
- ✅ 确定技术方案: Jekyll + GitHub Pages
- ✅ 创建 `feature/static-site` 分支
- ✅ 创建基础模板和布局

### 会话 3-5
- ✅ 修复 baseurl 配置
- ✅ 修复页面链接
- ✅ 添加 permalink 配置
- ✅ CSS 样式生效
- ✅ README 重写 + 中英文双语切换

### 会话 6-7
- ✅ 迁移 23/26 个日程 md
- ✅ 添加 speaker_image 字段支持

### 会话 8-9
- ✅ 创建 `_layouts/post.html` 独立页面
- ✅ 实现图片浮动设计
- ✅ 迁移所有 speaker 图片 (20/20)
- ✅ 重命名图片使用日期+名字格式

---

## 项目状态总结

### 已完成 ✅
1. Jekyll 网站基础架构
2. 26 个日程文档全部迁移（含 R-EAM 2026 占位符）
3. 20 张主讲人图片迁移并重命名
4. 中英文双语 README
5. 现代化前端设计（首页、日程表格、文献页、资料页）
6. Post 详情页独立布局
7. GitHub Actions 自动部署（已修复）

### 待解决问题 ⏳
1. 首页最新推送卡片无法点击跳转
2. 链接使用日期+主题缩写（已完成配置，需验证）
3. 优化前端设计（响应式、动画等）

---

## 文件说明

**已纳入 Git 跟踪的文件**: 
- `README.md`, `README_CH.md` - 项目说明（双语）
- `index.html`, `schedule.html`, `papers.html`, `resources.html` - 主要页面
- `_config.yml`, `Gemfile` - Jekyll 配置
- `_layouts/`, `_includes/`, `_data/`, `_posts/` - Jekyll 核心目录
- `assets/` - 静态资源（图片、CSS、JS）
- `.github/workflows/jekyll.yml` - CI/CD 配置

**已排除在构建外的辅助文件**（`_config.yml` exclude 列表）：
- `AGENTS.md` - 开发规范
- `files_plan.md`, `findings.md`, `MIGRATION_PLAN.md`, `PLAN_ISSUES.md`, `task_plan.md` - 规划文件
- `log.md`, `prompt_log.md` - 日志记录
- `progress.md` - 本文件（进度记录）

---

**最后更新**: 2026-03-18

---

## 2026-03-18 下午 - 日程显示问题修复

### 问题描述
用户反馈日程安排页面显示为空，"即将进行 / 进行中" 和 "历史日程" 都为空。

### 第 1 轮修复
1. 检查 `schedule.html` Liquid 代码 - 发现 `where_exp` 查询逻辑问题
2. 检查 `_config.yml` - 发现 `limit_posts: 10` 限制
3. 修改查询逻辑为复杂的 `where_exp` 条件

**结果**：GitHub Actions 构建失败
```
Liquid syntax error (line 23): Expected end_of_string but found id
```

### 第 2 轮修复
**发现问题**：Jekyll 3.10.0 的 `where_exp` 不支持复合条件（`or` 语法）

**解决方案**：改为使用原生 for + if 模式

修改的文件：
- `_config.yml`: `limit_posts: 10` → `100`
- `schedule.html`: 改用 for + if 模式
- `index.html`: 改用 for + if 模式

### 第 3 轮修复（当前）
**发现问题**：`exclude: "*.md"` 递归排除了 `_posts/` 下的所有 posts！

查看 GitHub Actions 日志：
```
EntryFilter: excluded /_posts/2022-08-09-mental-speed.md
EntryFilter: excluded /_posts/2022-08-24-mnle.md
...
```

**根本原因**：
- `"*.md"` 本意排除根目录下的辅助 md 文件
- 但 Jekyll 的 exclude 模式是递归的，应用到所有子目录
- 导致所有 24 个 posts 被 EntryFilter 排除

**解决方案**：
- 将 `"*.md"` 改为具体文件的完整路径（以 `/` 开头）
- 或删除此项，只保留目录排除

修改的文件：
- `_config.yml`: 修复 exclude 配置

### 第 4 轮修复 ✅ **成功！**
**状态**：已验证通过

**修复确认**：
- GitHub Actions 构建成功 ✅
- 日程安排页面正常显示 posts ✅
- "即将进行/进行中" 显示 plan/in-progress 状态 posts ✅
- "历史日程" 显示 done/nil 状态 posts ✅
- 首页最新推送正常显示 ✅

**最终修改的文件**：
| 文件 | 修改内容 | 状态 |
|------|----------|------|
| `_config.yml` | `limit_posts: 10` → `100` | ✅ |
| `_config.yml` | 修复 `exclude: "*.md"` → 具体文件路径 | ✅ |
| `schedule.html` | 改用 for + if 模式（避免 where_exp 复合条件） | ✅ |
| `index.html` | 改用 for + if 模式 | ✅ |

### Status 元数据
| 值 | 含义 | 示例 |
|----|------|------|
| `plan` | 计划中 | 2026-03-26-rean.md |
| `in-progress` | 进行中 | 2026-03-18-lba-confidence.md |
| `done` | 已完成 | 大部分历史 posts |
| `nil` (默认) | 等同于 done | 向后兼容 |

### 关键教训
1. **Jekyll exclude 模式是递归的** - `"*.md"` 会排除所有子目录的 .md 文件
2. **where_exp 不支持复合条件** - Jekyll 3.10.0 中 `or` 语法会报错
3. **使用 for + if 模式** - 更灵活，兼容性更好
4. **检查 EntryFilter 日志** - 快速定位文件排除问题

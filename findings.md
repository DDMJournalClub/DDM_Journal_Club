# Findings: Jekyll 日程显示问题

## 根本原因

### 1. **exclude: "*.md" 递归排除了所有 posts** ⚠️ **这是主因！**
```yaml
exclude:
  - "*.md"  # 本意排除根目录辅助文件，但递归应用到所有子目录！
```
GitHub Actions 日志：
```
EntryFilter: excluded /_posts/2022-08-09-mental-speed.md
EntryFilter: excluded /_posts/2022-08-24-mnle.md
...
```
**所有 24 个 posts 都被 EntryFilter 排除了！**

### 2. limit_posts 限制
- `_config.yml` 中 `limit_posts: 10` 导致 Jekyll 只处理最近的 10 个 posts
- 24 个 posts 文件中很多被忽略

### 3. Liquid where_exp 逻辑问题
- `post.status != 'done'` 对 `nil` 值返回 `false`（非 true）
- 只有显式设置 `status: done` 的 posts 会被排除
- 大部分 posts 没有 `status` frontmatter，导致 `nil` 比較問題

### 4. Jekyll 3.10.0 where_exp 不支持复合条件
**GitHub Actions 报错**：
```
Liquid syntax error (line 23): Expected end_of_string but found id
```

- `where_exp` 不支持 `or` 连接的复合条件
- 必须使用简单的 for + if 模式

## 解决方案对比

| 方案 | 优点 | 缺点 |
|------|------|------|
| A. 修复 where_exp | 保持现有文件结构 | 语法不支持，失败 |
| B. **for + if 模式** ✅ | 兼容所有 Jekyll 版本 | 代码稍长 |
| C. 子文件夹分类 | 逻辑清晰 | 需要移动文件 |

**最终选择：方案 B**（for + if 模式）

## 正确写法

### 错误的 where_exp
```liquid
{% assign upcoming = site.posts | where_exp: "post", "post.status == 'plan' or post.status == 'in-progress'" %}
```

### 正确的 for + if
```liquid
{% for post in site.posts %}
  {% if post.status == 'plan' or post.status == 'in-progress' %}
    <!-- 显示 post -->
  {% endif %}
{% endfor %}
```

## 修改的文件

| 文件 | 修改内容 |
|------|----------|
| `_config.yml` | `limit_posts: 10` → `100` |
| `schedule.html` | 改为 for + if 模式 |
| `index.html` | 改为 for + if 模式 |

## Status 元数据设计
- `plan`: 计划中的项目（未来）
- `in-progress`: 正在进行中的项目（当天）
- `done`: 已完成的项目（历史）
- `nil`: 默认值，等同于 done（向后兼容）

## 错误教训
1. 不应在没有 Jekyll 环境的本地测试更改
2. 需要先理解 Liquid 的 nil 处理机制
3. limit_posts 会对 posts 集合产生限制
4. **Jekyll 3.10.0 的 where_exp 不支持复合条件**
5. **Jekyll exclude 模式是递归的** - `"*.md"` 会排除所有子目录的 .md 文件

## 解决方案总结

### 修复后的 _config.yml
```yaml
# Jekyll 配置
timezone: Asia/Shanghai
future: true
show_drafts: false
limit_posts: 100

# 排除文件（注意：使用 / 开头表示根目录，避免递归排除）
exclude:
  - templates/
  - Private & Shared/
  - .gitignore
  - node_modules/
  - "/README.md"
  - "/README_CH.md"
  - "/README_zh.md"
  - "/task_plan.md"
  - "/findings.md"
  - "/progress.md"

# 默认配置
defaults:
  - scope:
      path: ""
      type: posts
    values:
      layout: post
      comments: true
      status: done
```

### 修复后的 schedule.html 查询模式
```liquid
{% for post in site.posts limit: 100 %}
  {% if post.status == 'plan' or post.status == 'in-progress' or post.status == nil %}
    <!-- 显示 post -->
  {% endif %}
{% endfor %}
```

**状态**：✅ 已修复并验证通过

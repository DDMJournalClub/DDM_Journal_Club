# Files Plan - 管理最近进展与更新

## 📋 项目概览

**项目**: DDMJC 静态网站 (Jekyll + GitHub Pages)  
**最后更新**: 2026-03-18  
**当前状态**: 🔧 修复 GitHub Actions 构建错误

---

## 🚨 紧急修复状态

### 当前问题
| 问题 | 严重程度 | 状态 | 负责人 |
|------|----------|------|--------|
| Liquid Exception: Cannot assemble URI string with ambiguous path | 🔴 P0 | 🔄 修复中 | Sisyphus |

### 修复策略
**选择方案**: 日期+slug permalink 格式

**理由**:
1. ✅ 与实际 md 文件名一致
2. ✅ 避免同日多 post 冲突
3. ✅ URL 可读性强
4. ✅ SEO 友好
5. ✅ 避免中文 slugify 问题

---

## 🔍 需要修改的文件清单

### 🔴 P0 - 立即修复 (阻止构建)

| # | 文件 | 行号 | 当前问题 | 修复内容 | 预期结果 |
|---|------|------|----------|----------|----------|
| 1 | `_config.yml` | 21 | `permalink: /:year-:month-:day/` | 改为 `/:year-:month-:day-:slug/` | URL 格式匹配文件名 |
| 2 | `index.html` | 109 | `'assets/images/...'` | 改为 `'/assets/images/...'` | 消除歧义路径 |
| 3 | `index.html` | 117 | `'assets/images/...'` | 改为 `'/assets/images/...'` | 消除歧义路径 |
| 4 | `schedule.html` | 3 | 无 permalink | 添加 `permalink: /schedule/` | 防止中文 slug |
| 5 | `papers.html` | 3 | 无 permalink | 添加 `permalink: /papers/` | 防止中文 slug |
| 6 | `resources.html` | 3 | 无 permalink | 添加 `permalink: /resources/` | 防止中文 slug |
| 7 | `schedule.html` | 284 | `YYYY-MM-DD-报告标题简写.md` | 改为 `YYYY-MM-DD-topic-slug.md` | 文档一致性 |

### 🟡 P1 - 验证与测试

| # | 任务 | 工具/方法 | 成功标准 |
|---|------|-----------|----------|
| 8 | 本地 Jekyll 构建 | `jekyll build` | 无错误输出 |
| 9 | 检查 `_site` 结构 | `ls -R _site/` | URL 格式正确 |
| 10 | GitHub Actions 构建 | Push to main | 构建状态 Success |
| 11 | 在线验证 | 浏览器访问 | 所有页面可访问 |

---

## 📁 修改详情

### 1. `_config.yml` - Permalink 格式更新

**修改前:**
```yaml
# 第21行
permalink: /:year-:month-:day/
```

**修改后:**
```yaml
# 第21行
permalink: /:year-:month-:day-:slug/
```

**URL 对比**:
| Post 文件名 | 修改前 URL | 修改后 URL |
|-------------|------------|------------|
| `2026-03-18-lba-confidence.md` | `/2026/03/18/` | `/2026-03-18-lba-confidence/` |
| `2022-08-09-mental-speed.md` | `/2022/08/09/` | `/2022-08-09-mental-speed/` |

**注意事项**:
- 确保 `slug` 从文件名中提取（最后一个 `-` 后的部分）
- 所有现有 post 的 URL 会改变，需更新外部链接

---

### 2. `index.html` - 图片路径修复

**修改前 (第109行):**
```html
<img src="{{ 'assets/images/qrcode_ddmjournalclub.github.io.png' | relative_url }}" ...>
```

**修改后 (第109行):**
```html
<img src="{{ '/assets/images/qrcode_ddmjournalclub.github.io.png' | relative_url }}" ...>
```

**修改前 (第117行):**
```html
<img src="{{ 'assets/images/qrcode_bilibili.png' | relative_url }}" ...>
```

**修改后 (第117行):**
```html
<img src="{{ '/assets/images/qrcode_bilibili.png' | relative_url }}" ...>
```

**问题根源**:
- 缺少前导 `/` 导致 `relative_url` 过滤器无法正确组装 URI
- 与 `baseurl: "/DDM_Journal_Club"` 结合产生歧义路径

---

### 3. HTML 页面 - 添加 Permalink

**schedule.html - Front Matter (第1-4行):**
```yaml
---
layout: default
title: 日程安排
permalink: /schedule/
---
```

**papers.html - Front Matter (第1-4行):**
```yaml
---
layout: default
title: 文献资料
permalink: /papers/
---
```

**resources.html - Front Matter (第1-4行):**
```yaml
---
layout: default
title: 资料
permalink: /resources/
---
```

**index.html**:
- 可选择添加 `permalink: /` 或保持默认（Jekyll 自动识别首页）

**注意事项**:
- 必须以 `/` 开头
- 必须以 `/` 结尾（可选但推荐）
- 使用 ASCII 字符，避免中文

---

### 4. `schedule.html` - 示例文本更新

**修改前 (第284行):**
```html
<pre>YYYY-MM-DD-报告标题简写.md</pre>
```

**修改后 (第284行):**
```html
<pre>YYYY-MM-DD-topic-slug.md</pre>
<p style="color: var(--color-text-light); margin-top: var(--space-xs); font-size: 0.75rem;">
  示例: 2026-03-18-lba-confidence.md
</p>
```

**命名规范**:
- `YYYY-MM-DD`: 日期（必须与 front matter 中的 date 一致）
- `topic-slug`: 英文主题简写（如 `lba-confidence`, `mental-speed`）
- 使用连字符 `-` 分隔

---

## 🔧 修复执行计划

### Phase 1: 配置与路径修复
```
1. _config.yml (permalink)
   └── 保存后立即验证 YAML 语法

2. index.html (图片路径)
   └── 保存后检查其他图片路径

3. HTML 页面 (permalink)
   └── 批量修改，统一验证
```

### Phase 2: 文档与示例更新
```
4. schedule.html (示例文本)
   └── 更新注释说明

5. 检查其他 HTML 文件的示例文本
   └── papers.html, resources.html 等
```

### Phase 3: 验证测试
```
6. 本地构建测试
   └── jekyll build --destination _site

7. GitHub Actions 构建
   └── Push to main 分支

8. 在线验证
   └── 检查所有页面、链接、图片
```

---

## 📊 文件依赖关系

```
_config.yml (permalink)
    │
    ├──► 影响所有 _posts/*.md 的 URL 生成
    │
    └──► 影响所有页面中的 post.url 引用
         │
         ├──► index.html (最新 post 链接)
         ├──► schedule.html (日程列表链接)
         └──► _layouts/post.html (返回链接)

index.html (图片路径)
    │
    └──► 影响首页二维码显示

HTML 页面 (permalink)
    │
    ├──► 影响导航栏链接 (default.html)
    └──► 影响 _config.yml 中的 navigation
```

---

## 🎯 成功标准

### 构建成功
- [ ] `jekyll build` 无错误输出
- [ ] GitHub Actions 状态 Success
- [ ] `gh-pages` 分支成功更新

### URL 格式正确
- [ ] Post URL: `/2026-03-18-lba-confidence/`
- [ ] 日程页面: `/schedule/`
- [ ] 文献页面: `/papers/`
- [ ] 资料页面: `/resources/`

### 功能正常
- [ ] 首页动态读取最新 post
- [ ] 日程表格完整显示
- [ ] 所有导航链接有效
- [ ] 所有图片加载成功
- [ ] 二维码显示正常

---

## 📝 更新日志

### 2026-03-18
- 🔴 创建紧急修复计划
- ✅ 确定 permalink 策略: 日期+slug
- ✅ 完成文件修改清单
- ✅ 制定详细修复步骤
- ✅ 执行修复 (8个文件修改)
- ✅ Git 提交并推送 (commit: 23d2ce6)
- ✅ 更新文档状态 (commit: 2be6af8)
- ✅ 更新文件计划提交记录 (commit: f3755f9)
- ⏳ GitHub Actions 构建中 (需手动验证)

### 2026-03-18 (之前)
- ✅ 创建 `files_plan.md` 文件计划
- ✅ 修复 `_config.yml` permalink 配置 (第一次)
- ✅ 更新 `PLAN_ISSUES.md` 文档
- ✅ 分析 git 提交历史
- ✅ 创建文件状态矩阵

---

## 🔗 相关资源

- **GitHub Actions**: `.github/workflows/jekyll.yml`
- **Jekyll Permalinks**: https://jekyllrb.com/docs/permalinks/
- **Jekyll relative_url**: https://jekyllrb.com/docs/liquid/filters/
- **GitHub Pages 文档**: https://docs.github.com/en/pages

---

**文件计划版本**: v2.0  
**最后更新**: 2026-03-18  
**状态**: 已执行修复，等待 GitHub Actions 构建结果  
**维护者**: Sisyphus AI Agent

# DDMJC Permalink 与文件命名策略

## 策略概述

**采用方案**: 日期 + slug 格式  
**URL 格式**: `/:year-:month-:day-:slug/`  
**示例**: `/2026-03-18-lba-confidence/`

---

## 为什么选择日期+slug？

### 对比分析

| 方案 | 格式 | 优点 | 缺点 |
|------|------|------|------|
| **A. 仅日期** | `/:year-:month-:day/` | 简洁、URL 短 | 同日多 post 冲突 |
| **B. 日期+slug** ✅ | `/:year-:month-:day-:slug/` | 唯一、可读、与文件名一致 | URL 稍长 |
| **C. 仅 slug** | `/:title/` | 最简洁 | 中文 slug 问题 |
| **D. 分类+slug** | `/:categories/:title/` | 结构化 | 复杂度增加 |

### 选择理由

1. **与文件名一致**
   - 文件名: `2026-03-18-lba-confidence.md`
   - URL: `/2026-03-18-lba-confidence/`
   - 直观易懂，维护方便

2. **避免冲突**
   - 同一天可以有多个 post（如研讨会+讨论会）
   - slug 确保 URL 唯一性

3. **SEO 友好**
   - URL 包含英文关键词
   - 搜索引擎可识别

4. **避免中文问题**
   - slug 使用英文，避免 URI 编码问题
   - 与 `_config.yml` 中的 `permalink: /:year-:month-:day-:slug/` 配合

---

## 配置文件

### `_config.yml`

```yaml
# 全局 permalink 设置
permalink: /:year-:month-:day-:slug/

# 或保留子目录风格
permalink: /:year/:month/:day-:slug/
```

### 各页面显式 permalink

```yaml
# schedule.html
---
layout: default
title: 日程安排
permalink: /schedule/
---

# papers.html
---
layout: default
title: 文献资料
permalink: /papers/
---

# resources.html
---
layout: default
title: 资料
permalink: /resources/
---

# index.html (可选，Jekyll 默认识别)
---
layout: default
title: 首页
# permalink: /  # 可选
---
```

---

## 文件命名规范

### Post 文件命名

**格式**: `YYYY-MM-DD-topic-slug.md`

**示例**:
```
✅ 2026-03-18-lba-confidence.md
✅ 2022-08-09-mental-speed.md
✅ 2024-11-19-sleep-dep2.md

❌ 2026-03-18-lba confidence.md      # 包含空格
❌ 2026-03-18-理性证据累积.md        # 中文 slug
❌ 2026/03/18/lba-confidence.md      # 使用目录
```

### Slug 命名规则

1. **使用小写英文**
   - ✅ `lba-confidence`
   - ✅ `mental-speed`
   - ❌ `LBA-Confidence` (大写)
   - ❌ `理性证据累积` (中文)

2. **使用连字符分隔**
   - ✅ `closing-the-loop`
   - ❌ `closing_the_loop` (下划线)
   - ❌ `closing.the.loop` (点号)

3. **保持简洁**
   - ✅ `lba-confidence` (2-4 个词)
   - ❌ `linear-ballistic-accumulator-model-confidence` (过长)

4. **避免特殊字符**
   - ❌ `lba&confidence` (& 符号)
   - ❌ `lba:confidence` (冒号)
   - ❌ `lba(confidence)` (括号)

---

## URL 映射表

| Post 文件名 | 生成的 URL | 导航栏链接 |
|-------------|------------|------------|
| `2026-03-18-lba-confidence.md` | `/2026-03-18-lba-confidence/` | `/schedule/` |
| `2022-08-09-mental-speed.md` | `/2022-08-09-mental-speed/` | `/schedule/` |
| `2024-11-19-sleep-dep2.md` | `/2024-11-19-sleep-dep2/` | `/schedule/` |

### 页面 URL

| 页面 | URL | 说明 |
|------|-----|------|
| 首页 | `/` | Jekyll 默认识别 index.html |
| 日程 | `/schedule/` | 显式 permalink |
| 文献 | `/papers/` | 显式 permalink |
| 资料 | `/resources/` | 显式 permalink |

---

## 常见问题

### Q1: 为什么 HTML 页面需要显式 permalink？

**A**: 防止 Jekyll 从中文标题生成 slug。

如果 `schedule.html` 的 front matter 只有:
```yaml
---
layout: default
title: 日程安排
---
```

Jekyll 可能尝试生成 URL: `/日程安排/`，导致 URI 编码错误。

**解决方案**: 添加显式 ASCII permalink:
```yaml
permalink: /schedule/
```

---

### Q2: 可以自定义个别 post 的 permalink 吗？

**A**: 可以，在 post 的 front matter 中覆盖:

```yaml
---
title: "特殊报告"
permalink: /special-talk/
---
```

但推荐保持统一格式，除非特殊情况。

---

### Q3: 修改 permalink 后旧链接怎么办？

**A**: 有几种方案:

1. **保持旧链接可用** (推荐)
   - 使用 `jekyll-redirect-from` 插件
   - 或手动创建重定向页面

2. **添加重定向** (GitHub Pages 不支持 .htaccess)
   ```yaml
   ---
   title: "Old Post"
   permalink: /old-url/
   redirect_to: /new-url/
   ---
   ```

3. **接受链接失效** (如网站刚上线)
   - 更新所有外部引用

---

### Q4: 如何处理中文标题的 slug？

**A**: 推荐在 front matter 中指定英文 slug:

```yaml
---
title: "理性证据累积模型"
slug: "ream-rational-evidence"  # 手动指定
---
```

或依赖文件名中的英文部分（如 `2026-01-01-rean.md` 提取 `rean` 作为 slug）。

**注意**: `_config.yml` 的 `permalink: /:year-:month-:day-:slug/` 中的 `:slug` 默认从文件名提取（最后一个 `-` 后的部分）。

---

## 实施检查清单

### 配置文件
- [ ] `_config.yml` 包含 `permalink: /:year-:month-:day-:slug/`
- [ ] `baseurl` 设置正确（`"/DDM_Journal_Club"` 或 `""`）
- [ ] `url` 设置正确（不含尾部 `/`）

### HTML 页面
- [ ] `schedule.html` 包含 `permalink: /schedule/`
- [ ] `papers.html` 包含 `permalink: /papers/`
- [ ] `resources.html` 包含 `permalink: /resources/`
- [ ] `index.html` 可选择包含 `permalink: /`

### Post 文件
- [ ] 所有文件名遵循 `YYYY-MM-DD-slug.md` 格式
- [ ] 文件名使用英文 slug
- [ ] 无空格、特殊字符或中文

### 链接引用
- [ ] 所有 `relative_url` 过滤器使用带前导 `/` 的路径
- [ ] 示例: `'/assets/images/...'` 而非 `'assets/images/...'`

### 文档
- [ ] `schedule.html` 示例文本更新为 `YYYY-MM-DD-topic-slug.md`
- [ ] 添加具体示例（如 `2026-03-18-lba-confidence.md`）

---

## 验证步骤

### 1. 本地构建测试
```bash
jekyll build --destination _site
# 检查输出，确认无错误
```

### 2. 检查生成的 URL
```bash
ls _site/2026*/  # 查看 post 目录结构
# 应看到类似: 2026-03-18-lba-confidence/
```

### 3. 验证页面链接
```bash
cat _site/schedule/index.html | grep "2026-03-18"
# 应看到正确的链接: href="/DDM_Journal_Club/2026-03-18-lba-confidence/"
```

### 4. GitHub Actions 构建
- Push 到 `main` 分支
- 检查 Actions 状态是否为 Success
- 确认 `gh-pages` 分支更新

---

## 相关文档

- [task_plan.md](./task_plan.md) - 修复计划
- [files_plan.md](./files_plan.md) - 文件修改清单
- [AGENTS.md](./AGENTS.md) - 项目开发规范
- [PLAN_ISSUES.md](./PLAN_ISSUES.md) - 问题追踪

---

**策略版本**: v1.0  
**最后更新**: 2026-03-18  
**作者**: Sisyphus AI Agent

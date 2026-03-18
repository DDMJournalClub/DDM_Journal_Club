# DDMJC 静态网站项目计划

## 目标
创建 GitHub Pages 静态网站，实现：
1. 自动从 md 文件生成首页推送和日程表格
2. 现代化设计（不使用原有 Notion 风格）
3. 结构化导航：首页、日程表格、文献、资料
4. 中英文双语切换
5. README 显示 GitHub 关注/star 变化

## 当前状态: 🔧 修复 GitHub Actions 构建错误

---

## 🚨 紧急问题 (GitHub Actions 构建失败)

### 问题描述
**错误**: `Liquid Exception: Cannot assemble URI string with ambiguous path`

**发生时间**: GitHub Actions 构建阶段
**严重程度**: 🔴 P0 - 阻止网站部署

### 根本原因分析
1. `index.html` 图片路径缺少前导 `/`，导致 `relative_url` 生成歧义路径
2. HTML 页面使用中文标题但未指定 `permalink`，Jekyll 尝试 slugify 中文失败
3. `_config.yml` permalink 格式与文件命名策略不匹配

### 修复策略
采用 **日期+slug** permalink 格式，与实际 md 文件名一致

---

## 决策记录

| 决策项 | 选项 | 选择 | 理由 |
|--------|------|------|------|
| 静态网站生成器 | Jekyll | ✅ | GitHub Pages 原生支持 |
| 部署平台 | GitHub Pages | ✅ | 免费且集成好 |
| md 格式标准化 | 必须 | ✅ | 便于自动化提取 |
| 本地测试方案 | GitHub 直接测试 | ✅ | Windows 无需配置 Ruby |
| 语言支持 | 中英文双语 | ✅ | 用户需求 |
| **permalink 格式** | 日期+slug vs 仅日期 | ✅ **日期+slug** | 避免同日冲突、URL 可读、与文件名一致 |

---

## 修复阶段计划

### 🔴 P0 - URI 构建错误修复 (立即执行)

#### 1. 配置文件修复
**文件**: `_config.yml`
- [ ] 将 `permalink: /:year-:month-:day/` 改为 `/:year-:month-:day-:slug/`
- [ ] 验证配置语法正确

**预期效果**: Post URL 格式变为 `/2026-03-18-lba-confidence/`

#### 2. 图片路径修复
**文件**: `index.html`
- [ ] 第109行: `'assets/images/qrcode_ddmjournalclub.github.io.png'` → `'/assets/images/qrcode_ddmjournalclub.github.io.png'`
- [ ] 第117行: `'assets/images/qrcode_bilibili.png'` → `'/assets/images/qrcode_bilibili.png'`

**预期效果**: 消除 `relative_url` 歧义路径错误

#### 3. HTML 页面添加 permalink
**文件**: `schedule.html`
- [ ] 在 front matter 添加: `permalink: /schedule/`

**文件**: `index.html`
- [ ] 确认 front matter 包含 `permalink: /` 或完全省略（Jekyll 默认首页）

**文件**: `papers.html`
- [ ] 在 front matter 添加: `permalink: /papers/`

**文件**: `resources.html`
- [ ] 在 front matter 添加: `permalink: /resources/`

**预期效果**: 防止 Jekyll 从中文标题自动生成 slug

#### 4. 示例文本更新
**文件**: `schedule.html`
- [ ] 第284行: 将 `YYYY-MM-DD-报告标题简写.md` 改为 `YYYY-MM-DD-topic-slug.md`
- [ ] 添加示例: `<!-- 示例: 2026-03-18-lba-confidence.md -->`

**预期效果**: 文档与实际命名策略保持一致

### 🟡 P1 - 验证与测试

#### 5. 本地构建验证
- [ ] 运行 `jekyll build --destination _site`
- [ ] 检查 `_site/` 目录结构
- [ ] 确认所有页面 URL 格式正确

#### 6. GitHub Actions 验证
- [ ] 推送修复到 `main` 分支
- [ ] 监控 Actions 构建状态
- [ ] 确认部署到 `gh-pages` 分支成功

#### 7. 在线验证
- [ ] 访问 `https://ddmjournalclub.github.io/DDM_Journal_Club/`
- [ ] 测试所有导航链接
- [ ] 验证日程页面动态读取正常
- [ ] 验证 post 详情页可访问

### 🟢 P2 - 文档更新

#### 8. 更新 PLAN_ISSUES.md
- [ ] 添加 permalink 策略说明
- [ ] 更新文件命名规范
- [ ] 记录修复过程

#### 9. 更新 AGENTS.md
- [ ] 添加 permalink 配置规范
- [ ] 更新示例代码

#### 10. 创建 permalink 迁移指南
- [ ] 为未来的 contributors 提供规范

---

## 检查清单 (Checklist)

### 修复前检查
- [x] 确认当前 permalink 格式
- [x] 确认文件命名模式
- [x] 确认所有 HTML 页面 front matter

### 修复中检查
- [ ] 每个文件修改后立即保存
- [ ] 验证 YAML 语法 (使用在线 YAML 验证器)
- [ ] 检查 Liquid 语法 (无未闭合标签)

### 修复后检查
- [ ] Jekyll 构建成功
- [ ] Actions 构建通过
- [ ] 所有页面可访问
- [ ] 所有图片加载正常
- [ ] 所有导航链接有效

---

## 风险与回滚策略

### 风险点
| 风险 | 可能性 | 影响 | 缓解措施 |
|------|--------|------|----------|
| 新的 permalink 破坏外部链接 | 中 | 中 | 暂时保持旧 URL 可用或添加重定向 |
| 同日多个 post 冲突 | 低 | 高 | slug 确保唯一性 |
| 中文标题 slugify 失败 | 中 | 高 | 使用文件中的英文 slug |

### 回滚计划
如修复导致问题，可回滚到：
- `permalink: /:year-:month-:day/` (仅日期)
- 但需同时修复 HTML 页面 permalink 问题

---

## 相关资源

- **GitHub 仓库**: https://github.com/DDMJournalClub/DDM_Journal_Club
- **在线网站**: https://ddmjournalclub.github.io/DDM_Journal_Club/
- **GitHub Actions**: `.github/workflows/jekyll.yml`
- **Jekyll 文档**: https://jekyllrb.com/docs/permalinks/

---

## 更新日志

### 2026-03-18
- 🔴 新增 P0: 修复 GitHub Actions 构建错误
- ✅ 确定 permalink 策略: 日期+slug
- ✅ 完成问题根源分析
- 🔄 准备执行修复步骤

---

**文件版本**: v2.0  
**最后更新**: 2026-03-18  
**状态**: 准备执行修复

# 进度日志

## 2025-03-18

### 会话 1

**创建了规划文件**:
- ✅ `task_plan.md` - 项目阶段跟踪
- ✅ `findings.md` - 研究发现

**下一步**:
- 创建新分支 `feature/static-site`
- 询问用户确认技术方案和设计需求

---

### 会话 2

**用户确认**:
- 技术方案: Jekyll (推荐)
- md 格式: 必须标准化 front matter
- 首页内容: 全部显示

**已完成**:
- ✅ 创建新分支 `feature/static-site`
- ✅ 创建模板文件夹 `templates/`
- ✅ 创建 Jekyll 配置 `_config.yml`
- ✅ 创建基础布局 `_layouts/default.html`
- ✅ 创建 CSS 样式 `assets/css/style.css`
- ✅ 创建首页 `index.html`
- ✅ 创建日程页 `schedule.html`
- ✅ 创建文献页 `papers.html`
- ✅ 创建资料页 `resources.html`
- ✅ 创建 GitHub Actions 工作流
- ✅ 部署到 GitHub Pages
- ✅ CSS 样式生效
- ✅ 修复 baseurl 和链接问题

---

### 会话 3

**问题修复**:
- ✅ 修复 baseurl 配置
- ✅ 修复页面链接（使用 relative_url）
- ✅ 添加 permalink 配置

**新需求确认**:
- 重写 README，显示 GitHub star 变化
- 中英文双语切换
- 迁移剩余日程文档

---

### 会话 4

**P1 完成**:
- ✅ 新增 2 个日程 post (2025-01-16, 2025-01-23)
- ✅ schedule.html 动态读取 _posts/ 数据
- ✅ index.html 首页动态显示最新日程

---

### 会话 5

**问题修复**:
- ✅ 修复 baseurl 配置
- ✅ 修复页面链接
- ✅ 添加 permalink 配置
- ✅ CSS 样式生效
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
- ✅ 创建模板文件夹 `templates/`:
  - `schedule-entry-template.md` - 日程模板
  - `paper-entry-template.md` - 文献模板
  - `discussion-entry-template.md` - 讨论模板
- ✅ 创建 Jekyll 配置 `_config.yml`
- ✅ 创建基础布局 `_layouts/default.html`
- ✅ 创建 CSS 样式 `assets/css/style.css`
- ✅ 创建首页 `index.html` (使用 frontend-design 技能)
- ✅ 创建日程页 `schedule.html`
- ✅ 创建文献页 `papers.html`
- ✅ 创建资料页 `resources.html`
- ✅ 创建 GitHub Actions 工作流 `.github/workflows/jekyll.yml`

**文件结构**:
```
DDMJC_Journal_Club/
├── _config.yml           # Jekyll 配置
├── _layouts/
│   └── default.html      # 基础布局
├── _posts/               # 日程文章目录
├── assets/
│   └── css/
│       └── style.css    # 样式文件
├── templates/           # md 模板
├── index.html           # 首页
├── schedule.html        # 日程页
├── papers.html          # 文献页
├── resources.html       # 资料页
├── .github/
│   └── workflows/
│       └── jekyll.yml  # 自动部署工作流
├── task_plan.md         # 项目计划
├── findings.md          # 研究发现
└── progress.md          # 进度日志
```
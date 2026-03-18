# DDMJC 静态网站项目计划

## 目标
创建 GitHub Pages 静态网站，实现：
1. 自动从 md 文件生成首页推送和日程表格
2. 现代化设计（不使用原有 Notion 风格）
3. 结构化导航：首页、日程表格、文献、资料

## 当前状态: 规划中

---

## 阶段 1: 仓库重构

- [ ] 创建新分支 `feature/static-site`
- [ ] 清理并重组目录结构
- [ ] 创建模板文件夹 `templates/`

**状态**: pending

---

## 阶段 2: md 文件模板标准化

- [ ] 创建 `templates/schedule-entry-template.md` 作为新建日程 md 的标准模板
- [ ] 定义 YAML front matter 字段规范
- [ ] 现有 md 迁移方案

**状态**: pending

---

## 阶段 3: 静态网站技术选型与搭建

- [ ] 选择静态网站生成器 (Jekyll 推荐)
- [ ] 创建 GitHub Actions 工作流
- [ ] 配置 GitHub Pages

**状态**: pending

---

## 阶段 4: 前端设计与开发

使用 `visual-engineering` + `frontend-design` 技能

- [ ] 导航栏设计
- [ ] 首页：最新推送 + 最近更新 + 组委会信息
- [ ] 日程表格页
- [ ] 文献页
- [ ] 资料页

**状态**: pending

---

## 阶段 5: 自动化功能开发

- [ ] 脚本：从 md 文件提取数据生成 JSON/HTML
- [ ] 自动化构建流程

**状态**: pending

---

## 阶段 6: 测试与部署

- [ ] 本地测试
- [ ] 部署到 GitHub Pages
- [ ] 验证自动更新功能

**状态**: pending

---

## 决策记录

| 决策项 | 选项 | 选择 | 理由 |
|--------|------|------|------|
| 静态网站生成器 | Jekyll / Hugo / Next.js | 待定 | 需要确认 |
| 部署平台 | GitHub Pages | ✅ | 免费且集成好 |
| md 格式标准化 | 必须/可选 | 待定 | 需要确认模板 |

---

## 待确认问题

1. ⏳ md 文件 front matter 规范
2. ⏳ 网站页面具体内容
3. ⏳ 设计风格偏好（已确认：不用原有 Notion 风格）
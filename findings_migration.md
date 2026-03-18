# Findings: Notion 备份内容分析

## 数据来源
`Private & Shared/DDM Club/` - Notion 网站备份文件夹

## 内容分类

### 1. DDMoJC 日程安排 ✅ **已完成移植**
- **数量**: 24 个 posts
- **位置**: `_posts/` 目录
- **状态**: 已移植，包含 frontmatter、图片、文档
- **模板**: 标准 schedule post 模板

### 2. 文献分享 ⏳ **待移植**
- **数量**: 67 个 md 文件
- **位置**: `Private & Shared/DDM Club/文献分享/`
- **内容**: 学术论文摘要、引用信息

#### 样本分析
```markdown
# Boehm, U., Annis, J., Frank, M.J., Hawkins, G.E., ...

## Citation
Boehm, U., Annis, J., Frank, M.J., Hawkins, G.E., ... (2018). ...

## Links
- PDF: [链接]
- DOI: [链接]

## Notes
研究笔记...
```

**特点**:
- 以论文标题为文件名
- 包含引用信息、链接、笔记
- 需要添加日期 frontmatter 才能作为 post

### 3. 热门讨论 ⏳ **待移植**
- **数量**: 26 个 md 文件 + 多个子文件夹
- **位置**: `Private & Shared/DDM Club/热门讨论/`
- **内容**: 技术问题讨论、安装教程、概念解释

#### 样本分析
```markdown
# Docker 安装 HDDM

## 问题描述
如何安装 dockerHDDM

## 解决方案
步骤 1: ...
步骤 2: ...

## 讨论
用户评论...
```

**特点**:
- 主题式讨论
- 有些包含子讨论（子文件夹）
- 技术教程性质
- 需要整理成结构化的 post

## 移植挑战

### 1. 日期问题
文献分享和热门讨论没有明确的日期，需要：
- 使用文件创建日期？
- 使用一个默认日期？
- 不显示日期？

### 2. URL 结构
如果作为 posts，URL 会是：
- `/2024/01/15/paper-title/`
- 这可能不是理想的 URL

### 3. 分类展示
需要在网站上有专门的页面展示：
- 文献分享列表
- 热门讨论列表

## 建议方案

### 方案 1: 作为 Posts（推荐）
- 使用 `category` 区分类型
- 使用现有 `_layouts/post.html`
- 在 `papers.html` 和 `discussions.html` 中按 category 筛选

### 方案 2: 作为 Collections
- 创建 `_papers/` 和 `_discussions/` collections
- 自定义 URL: `/papers/title/`
- 需要修改 `_config.yml`

### 方案 3: 作为 Data Files
- 放入 `_data/` 目录
- 使用 YAML/JSON 格式
- 在页面中遍历展示
- 不适合长内容

## 待决策
1. 使用哪种移植方案？
2. 如何处理日期问题？
3. 是否需要批量转换脚本？

# Task Plan: Notion 内容移植到 Jekyll 网站

## 项目背景
从 Notion 网站备份的 `Private & Shared/DDM Club/` 文件夹中，已将部分数据移植到 Jekyll 网站。目前 **DDMoJC 日程安排** 已完成移植，但 **文献分享** 和 **热门讨论** 的内容尚未移植。

## 移植状态总览

| 分类 | 状态 | 数量 | 备注 |
|------|------|------|------|
| DDMoJC 日程安排 | ✅ **已完成** | 24 个 posts | 已移植到 `_posts/`，包含图片和文档 |
| 文献分享 | ⏳ **待移植** | 67 个 md 文件 | 位于 `Private & Shared/DDM Club/文献分享/` |
| 热门讨论 | ⏳ **待移植** | 26 个 md 文件 | 位于 `Private & Shared/DDM Club/热门讨论/` |

## 目标
完成 **文献分享** 和 **热门讨论** 内容的移植，使其在 Jekyll 网站上正确显示。

## 移植策略对比

### 方案 A: 作为 Posts 移植（推荐）
将内容移植到 `_posts/` 目录，使用 Jekyll 的 post 机制

**优点：**
- 使用现有的 post 布局和样式
- 自动时间排序
- 支持标签、分类
- 与现有日程安排保持一致

**缺点：**
- 需要添加日期 frontmatter
- 文件命名需要符合 `YYYY-MM-DD-slug.md` 格式

### 方案 B: 作为独立 Pages
创建 `papers.html` 和 `discussions.html` 页面，内容以列表形式展示

**优点：**
- 不需要日期
- 可以自定义布局

**缺点：**
- 需要手动维护列表
- 搜索和筛选功能需要额外开发

### 方案 C: 作为 Collections
使用 Jekyll Collections 功能

**优点：**
- 灵活的组织方式
- 可以自定义 URL 结构

**缺点：**
- 需要修改 `_config.yml`
- 学习成本较高

## 最终决策
**采用方案 A**：将文献分享和热门讨论作为 posts 移植，但使用不同的 `categories` 或 `tags` 来区分内容类型。

## 移植阶段

### Phase 1: 文献分享移植（67 个文件）
- [ ] 分析文献分享内容结构
- [ ] 创建文献分享的 frontmatter 模板
- [ ] 批量转换和移植（可能需要脚本）
- [ ] 验证显示效果

### Phase 2: 热门讨论移植（26 个文件）
- [ ] 分析热门讨论内容结构
- [ ] 创建讨论的 frontmatter 模板
- [ ] 批量转换和移植
- [ ] 验证显示效果

### Phase 3: 页面优化
- [ ] 更新 `papers.html` 展示文献分享
- [ ] 更新 `resources.html` 或创建 `discussions.html` 展示热门讨论
- [ ] 添加分类筛选功能
- [ ] 添加标签云

## 技术细节

### 文件位置
```
Private & Shared/
└── DDM Club/
    ├── DDMoJC 日程安排/          ✅ 已完成（24 posts）
    ├── 文献分享/                  ⏳ 待移植（67 个 md 文件）
    │   ├── Alan N, T... .md
    │   ├── Boehm, U... .md
    │   └── ...
    └── 热门讨论/                  ⏳ 待移植（26 个 md 文件）
        ├── bayesian p 与 hypothesis testing.md
        ├── DDM 与 confidence 的讨论.md
        └── ...
```

### 当前 _posts/ 结构
```
_posts/
├── 2022-08-09-mental-speed.md       # 已有日程 post
├── ...
└── 2026-03-26-rean.md               # 最新日程 post
```

### 建议的新结构
```
_posts/
├── 2022-08-09-mental-speed.md       # category: schedule
├── ...
├── 2024-01-15-paper-evans-2020.md   # category: paper
└── 2024-02-20-discussion-hddm.md    # category: discussion
```

## 下一步行动
等待用户确认移植方案后，开始 Phase 1：文献分享内容分析。

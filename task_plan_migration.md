# DDMJC Notion 到 Jekyll 移植计划

## 项目背景

DDMoJC (Diffusion Decision Model Journal Club) 的 Notion 网站备份已部分移植到 Jekyll 静态网站。

### 当前完成情况

| 类别 | 状态 | 数量 | 说明 |
|------|------|------|------|
| **DDMoJC 日程安排** | ✅ 已完成 | 24 posts | 已完整移植，包括图片和 frontmatter |
| **文献分享** | ❌ 待移植 | 70 files | 需要移植为 paper 类型 posts |
| **热门讨论** | ❌ 待移植 | 39 files | 需要移植为 discussion 类型 posts |

**总计**: 133 个内容项，已完成 24/133 (18%)

---

## 移植范围

### 1. 文献分享 (70个文件)

**位置**: `Private & Shared/DDM Club/文献分享/`

**数据结构**:
- 每篇文献一个 .md 文件
- 文件命名格式: `Author (Year) Title [hash].md`
- 部分包含关联文件夹 (存放PDF、图片等)
- CSV索引文件: `文献分享 1a3781b6768a454cba8cb8a640a5cf99.csv` (23条记录)

**CSV 字段分析**:
| 字段 | 说明 | Jekyll 映射 |
|------|------|-------------|
| Name | 完整 APA 格式引用 | title |
| Tags | 标签列表 (逗号分隔) | tags |
| created time | 创建时间 | date |
| edited time | 编辑时间 | (可选) |
| pdf | PDF 附件链接 | pdf |
| 推荐等级 | ⭐⭐ 等 | rating |
| 日程安排 | 关联的日程链接 | related_schedule |
| 是否认领 | Yes/No | claimed |
| 汇报进度 | In progress/Done | progress |

**内容特点**:
- 标题包含完整文献引用信息
- 正文包含文献摘要、研究方法、结果、结论
- 部分包含微信公众号链接
- 可能包含图片和 PDF 附件

### 2. 热门讨论 (39个文件)

**位置**: `Private & Shared/DDM Club/热门讨论/`

**数据结构**:
- 每个讨论主题一个 .md 文件
- 文件命名格式: `讨论标题 [hash].md`
- 部分包含关联文件夹
- CSV索引文件: `热门讨论 f29984211cf749b4af0fa4d9af1f1415.csv` (27条记录)

**CSV 字段分析**:
| 字段 | 说明 | Jekyll 映射 |
|------|------|-------------|
| Name | 讨论标题 | title |
| Created | 创建时间 | date |
| Last edited by | 最后编辑者 | (可选) |
| Last edited time | 最后编辑时间 | (可选) |
| Owner | 所有者 | author |
| Tags | 标签 | tags |

**内容主题分类** (基于文件名):

| 类别 | 数量 | 示例 |
|------|------|------|
| HDDM 安装问题 | ~8 | Docker 安装、WSL 安装、依赖问题 |
| 模型拟合问题 | ~6 | 收敛问题、参数设置、错误处理 |
| 方法论讨论 | ~5 | Bayesian vs Frequentist、模型比较 |
| 数据处理 | ~4 | Coding schemes、Trial-wise analysis |
| 理论讨论 | ~3 | DDM 与 confidence、长 RT tasks |

---

## 目标格式

### 文献分享 Frontmatter

```yaml
---
title: "完整文献标题"
short_title: "简短标题"
category: paper
date: "YYYY-MM-DD"
tags:
  - "DDM"
  - "confidence"
authors: "Author1, Author2, ..."
year: 2022
venue: "Journal Name"
doi: "https://doi.org/..."
pdf: "/assets/pdfs/paper.pdf"
rating: "⭐⭐"  # 可选
claimed: true  # 是否被认领
progress: "Done"  # In progress / Done
---
```

### 热门讨论 Frontmatter

```yaml
---
title: "讨论主题"
short_title: "简短主题"
category: discussion
date: "YYYY-MM-DD"
tags:
  - "tutorial"
  - "installation"
author: "创建者姓名"
---
```

---

## 移植步骤

### Phase 1: 文献分享移植 ⭐ 高优先级

#### 1.1 数据提取与解析
- [ ] 读取 CSV 索引文件获取元数据
- [ ] 建立文件与 CSV 记录的对应关系
- [ ] 解析文献引用信息 (作者、年份、标题、期刊、DOI)
- [ ] 提取并规范化 tags

#### 1.2 内容转换
- [ ] 设计文献分享的 frontmatter 模板
- [ ] 转换正文内容为标准 Markdown
- [ ] 识别并提取图片引用
- [ ] 识别并提取 PDF 附件链接

#### 1.3 资源迁移
- [ ] 扫描所有关联文件夹中的图片
- [ ] 重命名图片 (使用日期+slug格式)
- [ ] 复制图片到 `assets/images/papers/`
- [ ] 扫描并复制 PDF 到 `assets/pdfs/`

#### 1.4 文件生成
- [ ] 生成符合 Jekyll 格式的 .md 文件
- [ ] 文件名格式: `YYYY-MM-DD-paper-slug.md`
- [ ] 替换图片路径为 Jekyll 格式
- [ ] 保存到 `_posts/` 目录

**预计耗时**: 4-6 小时

### Phase 2: 热门讨论移植 ⭐ 中优先级

#### 2.1 数据提取
- [ ] 读取 CSV 索引文件获取元数据
- [ ] 提取 tags 和创建者信息
- [ ] 从 CSV 提取创建日期

#### 2.2 内容转换
- [ ] 设计讨论帖子的 frontmatter 模板
- [ ] 保留问答结构
- [ ] 格式化代码块 (确保语言标识正确)

#### 2.3 资源迁移
- [ ] 扫描关联文件夹中的图片
- [ ] 复制图片到 `assets/images/discussions/`

#### 2.4 文件生成
- [ ] 生成符合 Jekyll 格式的 .md 文件
- [ ] 文件名格式: `YYYY-MM-DD-discussion-slug.md`
- [ ] 保存到 `_posts/` 目录

**预计耗时**: 2-3 小时

### Phase 3: 验证与优化 ⭐ 中优先级

#### 3.1 构建验证
- [ ] 本地 Jekyll 构建测试 (如可用)
- [ ] GitHub Actions 构建验证
- [ ] 检查 frontmatter 语法错误
- [ ] 验证所有图片链接

#### 3.2 内容检查
- [ ] 抽样检查转换质量 (10%)
- [ ] 验证特殊字符处理 (中文、公式)
- [ ] 检查代码块格式

#### 3.3 索引页面更新
- [ ] 创建/更新 `papers.html` 页面
- [ ] 创建/更新 `discussions.html` 页面
- [ ] 添加分类导航
- [ ] 更新首页展示逻辑

**预计耗时**: 1-2 小时

---

## 技术细节

### 文件名处理

#### 文献分享
```
输入: Luo, J , Yang, M , & Wang, L (2022) Learned irrele [hash].md
输出: 2022-08-25-learned-irrelevant-srr.md

处理流程:
1. 从 CSV 或文件名提取年份
2. 从 CSV 提取创建日期，或使用默认日期
3. 从标题提取关键词生成 slug
4. 中文标题: 保留关键拼音或手动指定
```

#### 热门讨论
```
输入: 如何安装HDDM？ [hash].md
输出: 2023-06-12-how-to-install-hddm.md

处理流程:
1. 从 CSV 提取创建日期
2. 标题转拼音生成 slug
```

### 图片路径转换

Notion 格式:
```markdown
![描述](Luo,%20J%20,%20Yang,%20M%20,%20&%20Wang,%20L%20(2022)%20Learned%20irrele/Untitled.png)
```

Jekyll 格式:
```markdown
![描述](/assets/images/papers/YYYY-MM-DD-slug/image.png)
```

### 日期处理策略

| 情况 | 策略 |
|------|------|
| CSV 有 created time | 使用 CSV 日期 |
| CSV 无记录 | 使用文件名中的年份 + 默认月份 |
| 无年份信息 | 使用文件修改日期作为回退 |

### 标签规范化

- 统一使用小写
- 使用连字符连接多词标签
- 映射常见标签:
  - `"DDM"` → `"ddm"`
  - `"confidence"` → `"confidence"`
  - `"fMRI"` → `"fmri"`

---

## 风险评估

| 风险 | 影响 | 可能性 | 缓解措施 |
|------|------|--------|----------|
| 文件命名冲突 | 中 | 中 | 使用 hash 确保唯一性 |
| 图片路径损坏 | 高 | 中 | 批量替换并验证所有链接 |
| 中文编码问题 | 中 | 低 | 统一使用 UTF-8 |
| 日期信息缺失 | 低 | 中 | 使用多级回退策略 |
| CSV 与文件不匹配 | 中 | 高 | 建立映射表，标记未索引文件 |
| PDF 附件缺失 | 低 | 中 | 标记为 "PDF 待添加" |

---

## 成功标准

- [ ] 所有 70 篇文献分享成功移植到 `_posts/`
- [ ] 所有 39 个热门讨论成功移植到 `_posts/`
- [ ] Jekyll 构建无错误
- [ ] 所有图片正常显示
- [ ] papers.html 页面正确显示文献列表
- [ ] discussions.html 页面正确显示讨论列表
- [ ] 首页可以展示最新文献和讨论

---

## 时间预估

| 阶段 | 预估时间 |
|------|----------|
| Phase 1: 文献分享移植 | 4-6 小时 |
| Phase 2: 热门讨论移植 | 2-3 小时 |
| Phase 3: 验证与优化 | 1-2 小时 |
| **总计** | **7-11 小时** |

---

## 参考文件

- 现有日程安排样本: `_posts/2025-04-08-closing-the-loop.md`
- 文献分享模板: `templates/paper-entry-template.md`
- 讨论模板: `templates/discussion-entry-template.md`
- AGENTS.md: 项目技术规范

---

**计划创建日期**: 2026-03-19
**最后更新**: 2026-03-19

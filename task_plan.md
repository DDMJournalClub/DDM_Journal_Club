# Task Plan: 修复 Discussions 页面图片显示问题

## 问题描述
Discussions 页面（如 discussion-1）中的图片无法正常显示。图片路径格式为：
```markdown
![Untitled](bayesian%20p%20与%20hypothesis%20testing/Untitled.png)
```

实际访问 URL 时返回 404。

## 问题分析

### 根本原因

#### 1. 图片路径格式错误 ⚠️ **主因**
- Markdown 中使用的是 Notion 导出的相对路径
- 路径包含空格和中文字符，URL编码后 (`%20`) 无法正确解析
- 例如：`bayesian%20p%20与%20hypothesis%20testing/Untitled.png`

#### 2. 图片文件位置不当
- 图片实际存放在 `Private & Shared/` 目录下
- `_config.yml` 中 `exclude` 包含了 `Private & Shared/`
- 这些图片不会被复制到 `_site` 目录，无法通过网站访问

#### 3. 路径引用方式问题
- 当前使用相对路径引用图片
- Jekyll 构建后，相对路径解析错误

## 最终修复方案

### 方案选择：迁移图片到 assets 目录 + 更新 Markdown 引用

**理由：**
- 符合 Jekyll 最佳实践（静态资源统一放在 assets/）
- 使用绝对路径 `/assets/...`，避免相对路径解析问题
- 文件名规范化（去除空格和中文，使用简短英文名）

### 修改计划

| 步骤 | 任务 | 状态 |
|------|------|------|
| 1 | 扫描所有 discussions 文件，找出所有图片引用 | ✅ 已完成 |
| 2 | 从 Private & Shared 复制图片到 assets/images/discussions/ | ✅ 已完成 |
| 3 | 重命名图片文件（去除空格和中文） | ✅ 已完成 |
| 4 | 更新 Markdown 文件中的图片路径 | ✅ 已完成 |
| 5 | 提交并验证 | ⏳ 待验证 |

### 已完成的修改

#### 步骤 1: 扫描结果
- ✅ 发现 12 个 discussions 文件有图片引用问题
- ✅ 总计 19 处图片引用需要修复
- ✅ 已修复 discussion-1（3 张图片 + 1 个 PDF）

#### 步骤 2: 图片迁移
```
源: Private & Shared/DDM Club/热门讨论/bayesian p 与 hypothesis testing/Untitled.png
目标: assets/images/discussions/discussion-1/bayesian-hypothesis-testing-1.png
```

#### 步骤 3: 文件重命名
- `Untitled.png` → `bayesian-hypothesis-testing-1.png`
- `Untitled 1.png` → `bayesian-hypothesis-testing-2.png`
- `Untitled 2.png` → `bayesian-hypothesis-testing-3.png`

#### 步骤 4: Markdown 路径更新
```markdown
# 修改前 (3 处):
![Untitled](bayesian%20p%20%E4%B8%8E%20hypothesis%20testing/Untitled.png)
![Untitled](bayesian%20p%20%E4%B8%8E%20hypothesis%20testing/Untitled%201.png)
![Untitled](bayesian%20p%20%E4%B8%8E%20hypothesis%20testing/Untitled%202.png)

# 修改后:
![Untitled](/assets/images/discussions/discussion-1/bayesian-hypothesis-testing-1.png)
![Untitled](/assets/images/discussions/discussion-1/bayesian-hypothesis-testing-2.png)
![Untitled](/assets/images/discussions/discussion-1/bayesian-hypothesis-testing-3.png)
```

#### 步骤 5: PDF 文件处理
```markdown
# 修改前:
[PDF](bayesian%20p%20%E4%B8%8E%20hypothesis%20testing/2021cognition_yuhongbo_molly_...pdf)

# 修改后:
[PDF](/assets/documents/discussions/discussion-1/yuhongbo-peer-influence-moral-decision-making.pdf)
```

## 发现的系统性问题 ⚠️

### 受影响文件
| 文件 | 图片数量 | 状态 |
|------|----------|------|
| `discussion-1` | 3 张 | ✅ 已修复 |
| `discussion-2` | 1 张 | ⏳ 待修复 |
| `discussion-5` | 2 张 | ⏳ 待修复 |
| `discussion-9` | 1 张 | ⏳ 待修复 |
| `discussion-12` | 1 张 | ⏳ 待修复 |
| `discussion-13` | 1 张 | ⏳ 待修复 |
| `discussion-14` | 1 张 | ⏳ 待修复 |
| `discussion-15` | 1 张 | ⏳ 待修复 |
| `discussion-18` | 4 张 | ⏳ 待修复 |
| `discussion-20` | 1 张 | ⏳ 待修复 |
| `discussion-21` | 1 张 | ⏳ 待修复 |
| `discussion-23` | 2 张 | ⏳ 待修复 |
| **总计** | **19 张** | **1/12 已修复** |

### 下一步行动
1. **立即验证**：提交当前更改，验证 discussion-1 图片显示
2. **批量修复**：根据用户需求，决定是否批量修复其他 11 个文件
3. **自动化**：考虑编写脚本自动处理 Notion 导出的路径转换

## 状态
**✅ Discussion-1 修复完成，发现系统性问题待处理**

## 关键教训
1. Notion 导出的 Markdown 图片路径需要手动处理
2. 避免在文件名中使用空格和中文字符
3. Jekyll 静态资源应统一放在 assets/ 目录下
4. 使用绝对路径 `/assets/...` 比相对路径更可靠
5. 需要批量处理时，建议编写自动化脚本

# Findings: Discussions 图片路径问题

## 根本原因

### 1. **图片路径格式问题** ⚠️ **主因**
```markdown
![Untitled](bayesian%20p%20与%20hypothesis%20testing/Untitled.png)
```

**问题分析：**
- 路径包含空格和中文字符
- URL 编码后 (`bayesian%20p%20%E4%B8%8E%20hypothesis%20testing`) 复杂且容易出错
- 这是 Notion 导出时的默认路径格式

### 2. **图片文件位置不当**

**图片实际位置：**
```
Private & Shared/
└── DDM Club/
    └── bayesian p 与 hypothesis testing/
        ├── Untitled.png
        ├── Untitled 1.png
        └── Untitled 2.png
```

**Jekyll 配置问题：**
```yaml
# _config.yml
exclude:
  - Private & Shared/  # 图片被排除在构建外！
```

### 3. **路径引用方式**
- 使用相对路径：`bayesian%20p%20与%20hypothesis%20testing/Untitled.png`
- Jekyll 构建后，相对路径基于当前页面 URL 解析
- 页面 URL: `https://ddmjournalclub.github.io/DDM_Journal_Club/discussions/discussion-1/`
- 图片尝试加载: `https://ddmjournalclub.github.io/DDM_Journal_Club/discussions/discussion-1/bayesian%20p%20与%20hypothesis%20testing/Untitled.png`
- **结果：404**，因为该路径不存在

## 解决方案对比

| 方案 | 优点 | 缺点 | 可行性 |
|------|------|------|--------|
| A. 修改 exclude，包含 Private & Shared | 改动最小 | 路径仍有问题，URL 不友好 | ❌ 不推荐 |
| B. **迁移图片到 assets/** ✅ | 符合 Jekyll 规范，路径清晰 | 需要手动复制和重命名 | ✅ 推荐 |
| C. 使用外部图床 | 不占用仓库空间 | 依赖外部服务，可能失效 | ❌ 不推荐 |

## 推荐方案详细设计

### 目标目录结构
```
assets/
├── images/
│   ├── speakers/          # 已有
│   └── discussions/       # 新建
│       └── discussion-1/
│           ├── bayesian-hypothesis-testing-1.png
│           ├── bayesian-hypothesis-testing-2.png
│           └── bayesian-hypothesis-testing-3.png
└── documents/
    └── discussions/
        └── discussion-1/
            └── yuhongbo-peer-influence-moral-decision-making.pdf
```

### 文件名规范化规则
1. **去除空格**：用 `-` 替换空格
2. **去除中文**：使用英文描述
3. **添加序号**：如果多个 Untitled，添加 `-1`, `-2` 等
4. **保留扩展名**：`.png`, `.jpg` 等

### Markdown 路径更新规则
```markdown
# 修改前（相对路径，带 URL 编码）:
![Untitled](bayesian%20p%20与%20hypothesis%20testing/Untitled.png)

# 修改后（绝对路径，简洁）:
![Untitled](/assets/images/discussions/discussion-1/bayesian-hypothesis-testing-1.png)
```

## 已完成的修复 ✅

### discussion-1 修复详情
| 文件 | 图片数量 | 状态 |
|------|----------|------|
| `_discussions/2023-06-15-discussion-1.md` | 3 张 | ✅ 已修复 |

**修改内容：**
- ✅ 创建目录 `assets/images/discussions/discussion-1/`
- ✅ 复制并重命名 3 张图片
- ✅ 更新 Markdown 中的 3 个图片引用
- ✅ 迁移 1 个 PDF 文件并重命名
- ✅ 更新 PDF 引用路径

## 重要发现 ⚠️

### 系统性问题
扫描发现 **12 个 discussions 文件** 有类似的图片引用问题，共 **19 处引用**。

### 受影响文件列表
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

### 路径格式示例
这些文件都使用类似的 URL 编码路径格式：
```markdown
![Untitled](HDDM%E5%AE%89%E8%A3%85%E9%97%AE%E9%A2%98/Untitled.png)
![Untitled](%E6%8A%A5%E9%94%99%EF%BC%9AAttributeError%20'DataFrame'%20object%20has%20no%20attrib/Untitled.png)
![Untitled](trial%E6%95%B0%E9%87%8F%E5%AF%B9DDM%E7%9A%84%E5%BD%B1%E5%93%8D/Untitled.png)
```

## 验证清单

### discussion-1 验证
- [x] 图片复制到正确位置
- [x] 文件名规范化
- [x] Markdown 路径更新
- [x] PDF 文件迁移
- [ ] GitHub Actions 构建成功
- [ ] 线上页面图片正常显示

### 全局验证
- [ ] 其他 11 个文件修复
- [ ] 所有图片正常显示
- [ ] 所有 PDF 可正常下载

## 下一步建议

### 立即行动
1. **提交当前更改**：验证 discussion-1 图片显示是否正常
2. **批量修复**：如果需要，可以批量修复其他 11 个文件

### 长期优化
1. **自动化脚本**：编写脚本自动处理 Notion 导出的路径转换
2. **图片规范化**：建立统一的图片命名规范
3. **文档迁移检查**：检查 `_papers/` 和 `_posts/` 是否有类似问题

## 状态
**✅ Discussion-1 修复完成，发现系统性问题待处理**

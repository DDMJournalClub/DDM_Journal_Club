# 移植修订计划

## 问题总结

用户反馈的问题：
1. 文献和讨论不应该放在 `_posts/`，应该分开存储
2. PDF 和 PPT 资源不需要，应该删除
3. 需要在首页添加 Awesome-Evidence-Accumulation-Models 的介绍

## 新目录结构

```
_posts/          # 只保留日程安排 (24篇)
_papers/         # 文献分享 (67篇)
_discussions/    # 热门讨论 (26篇)
assets/          # 删除 pdfs/ 目录
```

## 实施步骤

### Phase 1: 删除资源文件
- [ ] 删除 `assets/pdfs/` 目录
- [ ] 删除文献文件夹中的 PDF 文件
- [ ] 从文献 frontmatter 中移除 pdf 字段

### Phase 2: 创建新目录并迁移
- [ ] 创建 `_papers/` 目录
- [ ] 将67篇文献从 `_posts/` 移动到 `_papers/`
- [ ] 创建 `_discussions/` 目录
- [ ] 将26篇讨论从 `_posts/` 移动到 `_discussions/`

### Phase 3: 更新配置
- [ ] 在 `_config.yml` 中添加新目录配置
- [ ] 配置 `_papers/` 和 `_discussions/` 的默认 frontmatter
- [ ] 确保 permalink 设置正确

### Phase 4: 更新页面
- [ ] 更新 `papers.html` 以使用 `site.papers`
- [ ] 更新 `resources.html` 以使用 `site.discussions`
- [ ] 更新 `index.html` 添加 Awesome-Repo 介绍

### Phase 5: 清理和验证
- [ ] 删除旧的迁移脚本中的资源复制逻辑
- [ ] 验证 Jekyll 构建
- [ ] 提交更改

## 注意事项

1. Jekyll Collections 可以用来处理 `_papers/` 和 `_discussions/`
2. 或者直接在页面中使用 glob 模式读取文件
3. 需要确保 permalink 仍然有效

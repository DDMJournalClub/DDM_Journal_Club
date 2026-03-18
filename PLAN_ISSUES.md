# 问题修复计划

## 问题 1: 首页最新推送卡片无法点击跳转

**现状**: index.html 中的最新推送卡片只展示了信息，但没有链接

**解决方案**:
1. 修改 `index.html` 中的最新推送卡片
2. 添加 `<a>` 包裹整个卡片或添加 "查看详情" 按钮
3. 链接指向: `{{ latest_post.url | relative_url }}`

**修改文件**:
- `index.html`

---

## 问题 2: 链接使用日期+主题缩写

**现状**: 当前链接可能是 Jekyll 默认的 slug 格式

**解决方案**:
1. 在 `_config.yml` 中配置 `permalink` 格式为自定义
2. 格式: `/:year-:month-:day-:slug/` 或 `/:year/:title/`
3. 或在每个 post 的 front matter 中指定 `permalink`

**推荐格式**: `permalink: /:year-:month-:day/`

**修改文件**:
- `_config.yml`
- 可能需要更新所有 post 的 front matter

---

## 问题 3: 图片命名使用日期+名字

**现状**: 当前图片名称混乱，如 `psycomodels.png`, `closing_loop.png`

**解决方案**:
1. 制定命名规则: `{日期}_{主讲人英文名首字母}.png`
   - 例如: `2025-07-07_NVD.png` (Noah van Dongen)
   - 例如: `2025-04-08_LW.png` (卢炜文)

2. 重命名现有图片

3. 更新所有 post 的 `speaker_image` 字段

**图片命名映射表**:

| 日期 | 主讲人 | 新文件名 |
|------|--------|----------|
| 2025-07-07 | Noah van Dongen | 2025-07-07_NVD.png |
| 2025-04-17 | Zhanbin | 2025-04-17_Z.png |
| 2025-04-08 | 卢炜文 | 2025-04-08_LW.png |
| 2025-01-23 | 罗天林 | 2025-01-23_TL.png |
| 2025-01-16 | 吴雨菲 | 2025-01-16_YW.png |
| 2024-10-10 | Jiaqi Huang | 2024-10-10_JH.png |
| 2024-06-12 | Xiujuan Wen | 2024-06-12_XW.png |
| 2024-05-20 | Pierre Le Denma | 2024-05-20_PL.png |
| 2024-03-29 | Jiaorong Luo | 2024-03-29_JL.png |
| 2023-11-13 | 张浩宇 | 2023-11-13_HZ.png |
| ... | ... | ... |

**修改文件**:
- `assets/images/speakers/*.png` (重命名)
- 所有 `_posts/*.md` (更新 speaker_image 字段)

---

## 问题 4: 迁移所有 speaker 图片并修改 md

**现状**: 只迁移了 3 张图片 (psycomodels, closing_loop, quantum_sampler)

**解决方案**:
1. 从源文件夹 `Private & Shared/DDM Club/DDMoJC 日程安排/*/` 复制所有图片
2. 重命名为日期+名字格式
3. 更新对应 post 的 front matter

**执行步骤**:

### Step 1: 列出所有源图片
```bash
ls "Private & Shared/DDM Club/DDMoJC 日程安排"/*/*.png
```

### Step 2: 复制并重命名
为每个有图片的 post:
1. 找到对应的源图片
2. 复制到 `assets/images/speakers/`
3. 重命名为标准格式
4. 更新 md 文件的 `speaker_image` 字段

### Step 3: 验证
- 检查所有图片是否存在
- 检查所有 md 是否有正确的 speaker_image

---

## 执行顺序

```
1. [index.html] - 修复首页卡片点击跳转
2. [_config.yml] - 修改 permalink 格式
3. [重命名图片] - 使用日期+名字格式
4. [迁移图片] - 复制所有源图片
5. [更新 md] - 修改所有 post 的 speaker_image
6. [验证] - 测试所有链接和图片
```

---

## 注意事项

1. **不需要 git push** - 等待用户确认后再推送
2. **保留源文件** - 不要删除 Private & Shared 中的原始文件
3. **备份** - 重要操作前先检查文件状态
---
name: update-post-status-and-workflow
overview: 将 22 篇历史帖子标记为 done，修复 schedule.html 中 nil 状态帖子的显示 bug，并提供新帖子创建和进度管理的标准化流程。
todos:
  - id: add-status-done-nil
    content: "为 21 篇无 status 字段的历史帖子添加 status: done（在 recording 行后插入）"
    status: completed
  - id: update-lba-status
    content: 将 2026-03-18-lba-confidence.md 的 status 从 in-progress 改为 done
    status: completed
  - id: fix-schedule-bug
    content: 修复 schedule.html 第33行：移除 or post.status == nil，消除双重显示 bug
    status: completed
---

## 用户需求

1. 将 22 篇已完成的旧帖子统一更新为 `status: done`
2. 修复 `schedule.html` 中无 status 帖子同时出现在两个区域的 bug
3. 了解新帖子的创建流程和进度管理

## 核心功能

- **批量状态修正**：21 篇历史帖子缺少 `status` 字段，需在 YAML frontmatter 中添加 `status: done`；1 篇 `lba-confidence` 从 `in-progress` 改为 `done`
- **修复双重显示 bug**：`schedule.html` 第 33 行 `post.status == nil` 条件导致帖子同时出现在"即将进行"和"历史日程"中，移除该冗余条件
- **新帖子创建指引**：梳理基于 `templates/schedule-entry-template.md` 的帖子创建流程和 `plan → in-progress → done` 的状态管理方式

## 技术方案

### 修改范围

本次修改仅涉及 YAML frontmatter 属性插入和 Liquid 模板条件修正，均为纯文本编辑，不涉及逻辑重构或新增文件。

### 实现细节

#### 1. Frontmatter 插入规则

所有无 status 的历史帖子，在 `recording:` 行正下方插入一行 `status: done`。参考已有 status 的帖子（如 `2025-04-17-social-influence.md`）的字段顺序：

```
recording: true
status: done          # ← 插入位置
speaker_image: "..."
```

#### 2. schedule.html 修复

- **第 33 行**：`{% if post.status == 'plan' or post.status == 'in-progress' or post.status == nil %}` → 移除 `or post.status == nil`
- **第 87 行**保持不变（历史日程区域保留 `or post.status == nil` 作为防御性编码，防止未来无 status 的帖子被遗漏）

```
<!-- 修复前（第33行） -->
{% if post.status == 'plan' or post.status == 'in-progress' or post.status == nil %}

<!-- 修复后 -->
{% if post.status == 'plan' or post.status == 'in-progress' %}
```

#### 3. 帖子状态生命周期

```
创建新帖 → status: plan（计划中，信息可为TBD）
确认详情 → status: in-progress（进行中，补充speaker/host/Zoom）
报告结束 → status: done（已完成，添加录屏链接和slides）
```

### 性能与可靠性

- 修改 22 个文件的 YAML frontmatter，不影响 Jekyll 构建性能
- 修复后 schedule 页面不再重复渲染同一帖子，渲染数据量减少 21 条
- 向后兼容：已正确标记的帖子不受影响

### 目录结构

```
DDMJC_Journal_Club/
├── _posts/
│   ├── 2022-08-09-mental-speed.md          # [MODIFY] 添加 status: done
│   ├── 2022-08-24-mnle.md                  # [MODIFY] 添加 status: done
│   ├── 2022-09-06-conformity-sddm.md       # [MODIFY] 添加 status: done
│   ├── 2022-09-24-learned-irrelevant.md     # [MODIFY] 添加 status: done
│   ├── 2022-10-08-prior-information.md     # [MODIFY] 添加 status: done
│   ├── 2022-10-18-non-decision-time.md     # [MODIFY] 添加 status: done
│   ├── 2022-10-30-ddm-stories.md           # [MODIFY] 添加 status: done
│   ├── 2022-11-08-evidence-accumulation.md # [MODIFY] 添加 status: done
│   ├── 2023-07-14-eeg-ddm.md               # [MODIFY] 添加 status: done
│   ├── 2023-09-07-model-comparison.md      # [MODIFY] 添加 status: done
│   ├── 2023-11-02-rational-process.md      # [MODIFY] 添加 status: done
│   ├── 2023-11-13-ddm-emotion.md           # [MODIFY] 添加 status: done
│   ├── 2024-03-29-sleep-deprivation.md     # [MODIFY] 添加 status: done
│   ├── 2024-05-20-prior-belief-confidence.md # [MODIFY] 添加 status: done
│   ├── 2024-06-12-fmri-ddm.md              # [MODIFY] 添加 status: done
│   ├── 2024-10-10-quantum-sampler.md       # [MODIFY] 添加 status: done
│   ├── 2024-11-19-sleep-dep2.md            # [MODIFY] 添加 status: done
│   ├── 2025-01-16-bayesian-nn.md           # [MODIFY] 添加 status: done
│   ├── 2025-01-23-inherent-limitation.md   # [MODIFY] 添加 status: done
│   ├── 2025-04-08-closing-the-loop.md      # [MODIFY] 添加 status: done
│   ├── 2025-07-07-psychomodels.md          # [MODIFY] 添加 status: done
│   └── 2026-03-18-lba-confidence.md        # [MODIFY] status: in-progress → done
├── schedule.html                            # [MODIFY] 移除第33行 nil 条件
└── templates/
    └── schedule-entry-template.md           # [REFERENCE] 新帖子模板
```
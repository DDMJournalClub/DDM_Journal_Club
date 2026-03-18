# AGENTS.md - DDMJC Jekyll Site

Guidelines for AI agents working on the DDMJC (Diffusion Decision Model Journal Club) Jekyll website.

## Project Overview

This is a **Jekyll static site** deployed on GitHub Pages for the DDMJC academic journal club. It hosts:
- **Schedule posts**: 24 meeting records with speaker info, abstracts, and recordings
- **Papers**: Academic paper discussions (to be migrated from Notion)
- **Resources**: Discussion threads and tutorials (to be migrated from Notion)

## Build Commands

### Local Development (if Jekyll installed)
```bash
# Install dependencies
bundle install

# Build site
bundle exec jekyll build

# Serve locally with live reload
bundle exec jekyll serve --livereload

# Build for production
bundle exec jekyll build --environment=production
```

### GitHub Pages Deployment
- **Automatic**: Push to `main` branch triggers GitHub Actions workflow (`.github/workflows/jekyll.yml`)
- **Manual**: Use "Run workflow" in GitHub Actions tab
- **Build status**: Check https://github.com/DDMJournalClub/DDM_Journal_Club/actions

### Verify Build
```bash
# Check _site folder after build
ls -la _site/

# Validate HTML (optional)
htmlproofer _site/ --disable-external
```

## Code Style Guidelines

### File Organization
```
_posts/           # Jekyll posts (YYYY-MM-DD-slug.md)
_layouts/         # HTML layouts (default.html, post.html)
_includes/        # Reusable HTML snippets
assets/           # Static assets (images, css, js)
templates/        # Post templates for content creation
Private & Shared/ # Notion backup (excluded from build)
```

### Frontmatter Standards

#### Schedule Post Template
```yaml
---
title: "完整报告标题"
short_title: "简短标题(20字以内)"
speaker: "报告人姓名"
institution: "报告人单位"
host: "主持人姓名"
date: "YYYY-MM-DD"
time: "20:00-21:00"
timezone: "北京时间 [GMT+8]"
zoom_id: "123 4567 8901"
language: "中文"  # 中文 / English
tags:
  - "DDM"
  - "tutorial"
recording: true  # true = 已有录屏，false = 暂无
status: done    # plan / in-progress / done
speaker_image: "/assets/images/speakers/YYYY-MM-DD_name.png"
links:
  paper: "https://doi.org/..."
  bilibili: "https://bilibili.com/..."
---
```

### Naming Conventions

- **Post filenames**: `YYYY-MM-DD-topic-slug.md` (lowercase, hyphenated)
- **Images**: `/assets/images/speakers/YYYY-MM-DD_initials.png`
- **Layout files**: `lowercase-with-hyphens.html`
- **Variables in templates**: `lowercase_with_underscores`

### Liquid Template Guidelines

#### DO's
```liquid
<!-- Use relative_url for links -->
<a href="{{ '/schedule/' | relative_url }}">日程</a>

<!-- Use for + if pattern (where_exp has limitations) -->
{% for post in site.posts %}
  {% if post.status == 'plan' %}
    {{ post.title }}
  {% endif %}
{% endfor %}

<!-- Access frontmatter variables -->
{{ page.title }}
{{ post.speaker }}
```

#### DON'Ts
```liquid
<!-- DON'T use complex where_exp conditions (Jekyll 3.10 limitation) -->
{% assign posts = site.posts | where_exp: "post", "status == 'a' or status == 'b'" %}

<!-- DON'T use recursive glob patterns in exclude -->
exclude:
  - "*.md"  # This excludes ALL .md files recursively!
```

### Jekyll Configuration Rules

#### Exclude Patterns (CRITICAL)
- **Use absolute paths** for files: `"/README.md"` (NOT `"*.md"`)
- **Directory excludes** are recursive by default: `templates/`
- **Never use** `"*.md"` - it excludes `_posts/*.md` too!

```yaml
# CORRECT
exclude:
  - templates/
  - "/README.md"
  - "/task_plan.md"

# WRONG - excludes _posts/*.md too!
exclude:
  - "*.md"
```

### Error Handling

#### Common Issues & Solutions

1. **Posts not showing**: Check `limit_posts` in `_config.yml` (default: 100)
2. **EntryFilter excluded**: Check `exclude` patterns in `_config.yml`
3. **Liquid syntax error**: Don't use complex `where_exp` with `or` conditions
4. **Image not loading**: Use leading slash `/assets/images/...`

### Content Migration Guidelines

When migrating from Notion (`Private & Shared/DDM Club/`):

1. **Schedule entries** → `_posts/` (already done: 24 posts)
2. **Papers** → `_posts/` with `category: paper` (67 files pending)
3. **Discussions** → `_posts/` with `category: discussion` (26 files pending)

#### Migration Checklist
- [ ] Add proper frontmatter with date
- [ ] Convert Notion links to local paths
- [ ] Move images to `/assets/images/`
- [ ] Update status: `plan` / `in-progress` / `done`
- [ ] Test build locally if possible

## Testing

### Pre-deployment Checklist
1. Verify `_config.yml` syntax is valid YAML
2. Check no `"*.md"` patterns in `exclude`
3. Confirm all post filenames match `YYYY-MM-DD-*.md`
4. Verify frontmatter in all posts
5. Build passes in GitHub Actions

### Post-deployment Verification
1. Check https://ddmjournalclub.github.io/DDM_Journal_Club/ loads
2. Verify schedule page shows posts
3. Test navigation links
4. Check images load correctly

## Key Technical Constraints

- **Jekyll version**: 3.10.0 (GitHub Pages)
- **where_exp limitation**: No complex `or` conditions
- **exclude behavior**: Recursive by default
- **Post limit**: `limit_posts: 100` (configure in `_config.yml`)
- **URL structure**: `/:year-:month-:day-:slug/` (permalink setting)

## References

- Jekyll docs: https://jekyllrb.com/docs/
- GitHub Pages: https://docs.github.com/en/pages
- Liquid docs: https://shopify.github.io/liquid/

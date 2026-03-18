# AGENTS.md — DDMJC Journal Club

**Project**: Jekyll static site for DDMJC (Diffusion Decision Model Journal Club)
**URL**: https://ddmjournalclub.github.io/DDM_Journal_Club/
**Framework**: Jekyll 4.x with Kramdown Markdown, GitHub Pages deployment
**Languages**: HTML, CSS, JavaScript (inline), Markdown, YAML

---

## Build / Deploy Commands

### GitHub Actions (Automatic)

- **Trigger**: Push to `main` or `feature/static-site` branches
- **Workflow**: `.github/workflows/jekyll.yml`
- **Output**: Deploys to `gh-pages` branch
- **Ruby version**: 3.1

```bash
# No manual test command — CI runs jekyll build to validate
```
---

## Directory Structure

```
_includes/     # Reusable HTML partials (header, footer, nav)
_layouts/      # Page templates (default.html, post.html)
_posts/        # Blog/talk entries — FILENAME FORMAT: YYYY-MM-DD-slug.md
_data/         # YAML data files (currently empty)
assets/        # Static assets
  css/         # Stylesheets (style.css)
  js/          # JavaScript (if any)
  papers/      # PDF papers for download
_pages/        # Extra Jekyll pages (if any)
templates/     # Content templates for contributors
.github/workflows/jekyll.yml  # CI/CD pipeline
```

---

## Content Conventions

### Post/File Naming

- **Posts**: `YYYY-MM-DD-slug.md` (e.g., `2026-03-18-lba-confidence.md`)
- **Pages**: `{name}.html` (e.g., `schedule.html`, `papers.html`)

### Front Matter (YAML Header)

All posts and pages MUST include front matter:

```yaml
---
title: "Talk Title"
short_title: "Short"
speaker: "Name"
institution: "University"
host: "Host Name"
date: "2026-03-18"
time: "14:00-15:00"
timezone: "北京时间 [GMT+8]"
zoom_id: "123 4567 8901"
language: "English"  # or "中文"
tags:
  - "ddm"
  - "review"
recording: true  # or false
links:
  paper: "https://..."
  code: "https://..."
---
```

### Markdown Guidelines

- Use **Kramdown** with GFM (GitHub Flavored Markdown)
- Use `---` horizontal rules to separate sections
- Use `#` for main headings, `##` for subsections
- Use `**bold**` and `*italic*` for emphasis
- Use `-` for bullet lists, `1.` for numbered lists
- Use `[text](url)` for links with `target="_blank" rel="noopener"` where appropriate
- Checkboxes: `- [x]` for done, `- [ ]` for pending
- Use `*引用*` or `> blockquote` sparingly

### Bilingual Content

- This site supports Chinese/English toggle
- Use `data-i18n` attributes in HTML for translatable text
- Use `zh-only` / `en-only` CSS classes to hide language-specific content
- Prefer Chinese for Chinese audience content, English for international

### Jekyll Liquid Templating

```liquid
# Variable interpolation
{{ page.title }}
{{ site.title }}
{{ site.time | date: '%Y' }}

# Conditionals
{% if page.recording %}
  <!-- show recording link -->
{% endif %}

# Loops
{% for item in site.navigation %}
  <a href="{{ item.url }}">{{ item.title }}</a>
{% endfor %}

# Filters
{{ content | relative_url }}
{{ page.date | date: "%Y-%m-%d" }}
```

---

## Code Style

### HTML

- Use **2-space indentation** (matching existing files)
- Use `<!-- 中文 comments -->` for comments
- Use semantic HTML5 elements (`<header>`, `<main>`, `<footer>`, `<nav>`)
- Always include `rel="noopener"` on external links
- Use `{{ '...' | relative_url }}` for internal links

### CSS

- **CSS variables** for theming (see `assets/css/style.css`)
- Use BEM-like naming: `.site-header`, `.site-nav`, `.footer-section`
- 2-space indentation
- Group properties: variables → reset → layout → typography → components → utilities
- Use hex colors with lowercase: `#2c5282`

### JavaScript

- Currently inline only (in `_layouts/default.html`)
- Use vanilla JS, no frameworks
- Use `localStorage` for preferences
- Use `const` / `let`, never `var`

### YAML (Config & Data)

- Use 2-space indentation (no tabs)
- Use kebab-case for keys: `zoom_id`, `baseurl`
- Quote strings with special characters: `title: "DDMJC - Diffusion..."`
- Use arrays for lists: `tags: ["ddm", "review"]`

---

### Python

If you use Python, you need use the env from conda, which is py312 as the main python env. The second env option is pymc5_3.11. 

## Naming Conventions

| Type | Convention | Example |
|------|------------|---------|
| Post filenames | `YYYY-MM-DD-slug.md` | `2026-03-18-lba-confidence.md` |
| HTML files | lowercase with hyphens | `schedule.html`, `papers.html` |
| CSS classes | kebab-case | `.site-header`, `.footer-content` |
| JavaScript variables | camelCase | `savedLang`, `newLang` |
| YAML keys | snake_case | `zoom_id`, `baseurl` |
| Git branches | kebab-case | `feature/static-site`, `fix/post-layout` |

---

## What NOT to Do

- **Do not** modify `_config.yml` plugins section (commented for GitHub Pages compatibility)
- **Do not** commit to `Private & Shared/` or `for_administrators/` (gitignored)
- **Do not** use `as any`, `@ts-ignore`, or type suppression
- **Do not** commit `*.lnk` files (gitignored)
- **Do not** add large binary files — use external links for PDFs
- **Do not** hardcode URLs — use `{{ site.url }}{{ site.baseurl }}`
- **Do not** use emoji in file names or headings in HTML (emoji OK in Markdown posts)

---

## Common Tasks

### Adding a New Talk Post

1. Create `_posts/YYYY-MM-DD-descriptive-slug.md`
2. Use `templates/schedule-entry-template.md` as reference
3. Include all required front matter fields
4. Write content in Markdown with `---` section dividers
5. Run `jekyll build` to verify

### Adding a New Page

1. Create `{name}.html` in root OR `_pages/{name}.md`
2. Add to `navigation` array in `_config.yml`
3. Include front matter with `layout: default`

### Modifying Site-wide Settings

- Edit `_config.yml` — most site config lives here
- Changes require `jekyll build` to take effect

---

## References

- [Jekyll Docs](https://jekyllrb.com/docs/)
- [Kramdown Syntax](https://kramdown.gettalong.org/syntax.html)
- [GitHub Pages + Jekyll](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll)
- Existing posts in `_posts/` for content examples

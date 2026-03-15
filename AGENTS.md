# AGENTS.md — Ideas in Software

Static blog built with **Hugo**, deployed to **GitHub Pages** via GitHub Actions.
Recreates [graeme-lockley.github.io](https://graeme-lockley.github.io) with a modern theme.

---

## Quick Reference

| Action | Command |
|---|---|
| Install Hugo (macOS) | `brew install hugo` |
| Dev server (with drafts) | `hugo server -D` |
| Dev server (published only) | `hugo server` |
| Production build | `hugo --minify` |
| New post | `hugo new content/posts/YYYYMMDD-slug/index.md` |
| Validate config | `hugo config` |
| List all content | `hugo list all` |
| List drafts | `hugo list drafts` |

Hugo version: **extended** edition required (for SCSS/image processing).
Minimum version: 0.128.0+

---

## Project Structure

```
ideas-in-software/
  hugo.toml                  # Site configuration (NOT config.toml)
  content/
    posts/                   # Blog posts — one directory per post
      YYYYMMDD-slug/         # e.g. 20160320-software-principles/
        index.md             # Post content (Hugo "page bundle")
        images/              # Post-specific images (optional)
    _index.md                # Homepage content
  layouts/                   # Custom template overrides
    _default/
    partials/
    shortcodes/              # Custom shortcodes
  static/                    # Static assets copied verbatim to output
    images/
    favicon.ico
  assets/                    # Processed assets (SCSS, JS via Hugo Pipes)
  themes/                    # Git submodule(s) for theme
  .github/
    workflows/
      deploy.yml             # GitHub Actions: build + deploy to Pages
  public/                    # Build output (gitignored)
```

---

## Content Conventions

### Post Frontmatter (required fields)

```yaml
---
title: "Post Title Here"
date: 2016-03-20
description: "One-sentence summary for SEO and list pages."
tags: ["java", "design"]
draft: false
---
```

- **date**: Determines URL and sort order. Format: `YYYY-MM-DD`.
- **draft: true**: Post is excluded from production builds. Visible with `hugo server -D`.
- **description**: Required. Used in meta tags and post list excerpts.
- **tags**: Optional but encouraged. Lowercase, hyphenated.

### Post URL Scheme

Posts render at `/posts/YYYYMMDD-slug/` matching the old site's URL pattern.
Configured in `hugo.toml` via `[permalinks]`.

### Writing Style

- Markdown with Hugo shortcodes where needed.
- Use page bundles (`posts/slug/index.md`) so images live alongside content.
- Code blocks: use fenced blocks with language identifier (```java, ```python, etc.).
- No HTML in markdown unless absolutely necessary — prefer shortcodes.

---

## Build & Deployment

### Local Development

```bash
hugo server -D --navigateToChanged
```

Opens at `http://localhost:1313/`. Live-reloads on save. `-D` includes drafts.

### Production Build

```bash
hugo --minify
```

Output goes to `public/`. This is what GitHub Actions runs.

### GitHub Actions Pipeline

The workflow in `.github/workflows/deploy.yml`:
1. Checks out repo (with submodules for theme)
2. Installs Hugo extended
3. Runs `hugo --minify`
4. Uploads `public/` via `actions/upload-pages-artifact@v3`
5. Deploys via `actions/deploy-pages@v4`

Key: the repo must have **Pages source set to "GitHub Actions"** in Settings > Pages.

### There Are No Tests

This is a static content site. Validation is:
1. `hugo --minify` exits 0 (build succeeds)
2. `hugo server -D` renders correctly in browser
3. No broken links (can use `htmltest` or `linkchecker` if added later)

---

## Hugo Configuration (hugo.toml)

Use `hugo.toml` (TOML format). Do NOT use `config.toml`, `config.yaml`, or `config/` directory
unless there's a specific multi-environment need.

Key settings:
```toml
baseURL = "https://<username>.github.io/ideas-in-software/"
languageCode = "en-us"
title = "Ideas in Software"
theme = "<theme-name>"

[permalinks]
  posts = "/posts/:filename/"

[params]
  description = "Thoughts on software design, principles, and craft"
  author = "Graeme Lockley"

[markup.goldmark.renderer]
  unsafe = false    # Do NOT enable unsafe HTML rendering
```

---

## Theme

Theme is installed as a **git submodule** under `themes/`.

```bash
git submodule add https://github.com/<theme-repo>.git themes/<theme-name>
git submodule update --init --recursive
```

When cloning the repo or in CI, always use `--recurse-submodules`:
```bash
git clone --recurse-submodules <repo-url>
```

Override theme templates by placing files in `layouts/` with matching paths.
Never edit files inside `themes/` directly.

---

## Code Style & Conventions

### Templates (Go/HTML in layouts/)

- Use `{{ }}` delimiters with spaces: `{{ .Title }}` not `{{.Title}}`.
- Partials for reusable fragments: `{{ partial "header.html" . }}`.
- Always pass context (`.`) to partials explicitly.
- Use `with` for nil-safe access: `{{ with .Params.description }}...{{ end }}`.
- Prefer `range` over index-based loops.
- Name partials and shortcodes in lowercase kebab-case: `post-header.html`.

### SCSS/CSS (in assets/)

- Use Hugo Pipes for SCSS processing, not external build tools.
- Variables in a `_variables.scss` partial.
- Mobile-first responsive design.
- Prefer relative units (rem, em) over px for typography.

### Shortcodes (in layouts/shortcodes/)

- One file per shortcode, named `shortcode-name.html`.
- Document parameters in a comment at the top of the file.
- Use named parameters: `{{< my-shortcode param="value" >}}`.

### File Naming

- Post directories: `YYYYMMDD-slug/` (e.g., `20160320-software-principles/`).
- Templates: lowercase, hyphenated (e.g., `single.html`, `post-meta.html`).
- Assets: lowercase, hyphenated.
- No spaces in any filenames.

### Git Conventions

- `public/` is gitignored (build output).
- `resources/` may be gitignored or committed (Hugo's asset cache).
- Theme is a git submodule — never commit theme files directly.
- Commit messages: imperative mood, concise (`Add post on value objects`, `Fix nav link styling`).

---

## Common Tasks for Agents

### Adding a New Post

```bash
hugo new content/posts/YYYYMMDD-slug/index.md
```
Then edit the generated file. Fill in all required frontmatter fields.
Set `draft: false` when ready to publish.

### Modifying the Theme

1. Identify the theme template to override (look in `themes/<name>/layouts/`).
2. Copy it to the same relative path under `layouts/`.
3. Edit the copy — Hugo's lookup order prefers project-level layouts.

### Adding a Shortcode

1. Create `layouts/shortcodes/<name>.html`.
2. Use in content as `{{</* name param="value" */>}}`.

### Debugging Build Failures

- Run `hugo --verbose` for detailed output.
- Check `hugo.toml` syntax with `hugo config`.
- Template errors show file and line — fix in project `layouts/`, not in theme.
- Missing content fields: check frontmatter against archetype in `archetypes/`.

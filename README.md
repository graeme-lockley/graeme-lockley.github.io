# Ideas in Software

A static blog on software design, principles, and craft — built with Hugo and deployed to GitHub Pages.

## Writing

New posts are created in `content/posts/` as page bundles:

```bash
hugo new content/posts/YYYYMMDD-slug/index.md
```

## Development

```bash
# Run local server with drafts
hugo server -D

# Production build
hugo --minify
```

## Deploy

Pushing to `main` automatically deploys via GitHub Actions. The site is available at:

- Homepage: https://graeme-lockley.github.io
- Posts: https://graeme-lockley.github.io/{slug}/

## Credits

Custom design - hand-crafted CSS and layouts in `assets/css/` and `layouts/`.

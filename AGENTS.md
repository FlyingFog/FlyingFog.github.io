# FlyingFog Hugo Site

## Overview

- Hugo site using the vendored `hugo-theme-ladder` theme.
- English content is the default; Chinese translations use the `.zh.md` suffix.
- Do not edit generated `public/` or `resources/` output.

## Common commands

```bash
hugo server
hugo --panicOnWarning --destination /tmp/flyingfog-hugo-check
```

## Content and navigation

- Homepage: `content/_index.md` and `content/_index.zh.md`.
- Publications: `content/publications.md` and `content/publications.zh.md`.
- Projects: `content/projects.md` and `content/projects.zh.md`.
- Keep English and Chinese navigation entries in `config.yml` in sync. Menu URLs are language-aware through the theme.

## Styling

- Home content and navigation share a `100rem` maximum width in `themes/hugo-theme-ladder/assets/scss/`.
- Site font variable: `themes/hugo-theme-ladder/assets/scss/common/_variables.scss`.
- The external webfont stylesheet is loaded by `themes/hugo-theme-ladder/layouts/partials/head.html`; change both locations when replacing the font.

## Configuration

- Use `services.googleAnalytics.id`, not the legacy top-level `googleAnalytics` key.
- Use `pagination.pagerSize`, not the legacy top-level `paginate` key.

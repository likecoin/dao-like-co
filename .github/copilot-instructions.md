# Copilot Instructions for dao-like-co

## Project Overview

This is the official website for [LikeCoin DAO](https://dao.like.co/), a Hugo-based static site that introduces LikeCoin, its relationship with [3ook.com](https://3ook.com) (a decentralized bookstore), and links to community resources. It is deployed automatically to GitHub Pages via GitHub Actions on every push to `main`.

## Tech Stack

- **Hugo** (managed via `hugo-bin` npm package, not a system install)
- **TailwindCSS v4** with PostCSS
- **FontAwesome 6** (loaded from CDN in `layouts/partials/head.html`)
- **Node/NPM** for tooling

## Commands

```bash
npm install          # Install dependencies (includes Hugo binary)
npm run dev          # Start dev server with drafts: hugo server -D
npm run build        # Production build: hugo --minify
npm run clean        # Remove public/ and resources/
npm run css:build    # Standalone CSS build via Tailwind CLI
```

> **Note:** Always use `npm run build` or `npm run dev` — never call `hugo` directly, as Hugo is provided by `hugo-bin` and lives in `node_modules/.bin/hugo`.

## Repository Structure

```
hugo.toml                  # Main Hugo config: baseURL, languages, all [params.*]
content/
  _index.md                # Homepage (English)
  _index.zh.md             # Homepage (Chinese)
  declaration/
    index.md               # Declaration page (English)
    index.zh.md            # Declaration page (Chinese)
layouts/
  index.html               # Homepage template
  _default/
    baseof.html            # Base layout (wraps all pages)
    list.html              # List page template
    single.html            # Single page template
  partials/
    head.html              # <head> with SEO, OG tags, CSS, JS
    navbar.html            # Navigation bar + theme/language toggles
    footer.html            # Site footer
    css.html               # (unused/legacy)
assets/css/
  main.css                 # TailwindCSS source with @theme variables
i18n/
  en.yaml                  # English UI strings
  zh.yaml                  # Chinese UI strings
static/assets/             # Shared static images (banner, gif, favicon)
.github/workflows/
  deploy.yml               # CI/CD: build + deploy to GitHub Pages
```

## Content & Frontmatter Conventions

- **Format:** All frontmatter uses **YAML** (not TOML), delimited by `---`
- **Date format:** `YYYY-MM-DD`
- **Required fields:** `title`, `date`, `image`, `description` (max 160 chars)
- **Optional fields:** `modified`, `summary`, `tags`, `categories`
- `image` is a root-relative path (e.g., `/assets/likecoin-banner.png`)

### Homepage-specific frontmatter (`_index.md` / `_index.zh.md`)
```yaml
title: LikeCoin - Powering 3ook.com
date: 2026-01-20
description: ...
image: /assets/likecoin-banner.png
punchline: ...
3ook_com:
  title: 3ook.com
  bio: ...
```
The Markdown body of `_index.md` renders as the "What is LikeCoin" section.

## i18n System

- **Default language:** English (`en`), served at `/`
- **Second language:** Chinese Traditional (`zh`), served at `/zh/`
- Both languages share the same `contentDir = 'content'` — files are differentiated by filename suffix: `index.md` (English) vs `index.zh.md` (Chinese)
- UI strings go in `i18n/en.yaml` and `i18n/zh.yaml`; reference in templates with `{{ T "key" }}`
- Language switcher uses Hugo's `.IsTranslated` / `.Translations`
- **No fallback:** if a page has no Chinese translation, it simply won't appear in the `/zh/` tree
- hreflang alternate links are only output when both language versions exist

## Site-Wide Configuration (`hugo.toml`)

All shared URLs and external links are stored in `hugo.toml` under `[params]` sub-sections — **never hardcoded in templates**:

| Section | Keys |
|---|---|
| `[params]` | `whitepaper`, `description`, `images` |
| `[params.like]` | `uniswap`, `coingecko`, `coinmarketcap` |
| `[params.get_involved]` | `github`, `governance`, `docs` |
| `[params.social_media]` | `youtube`, `x`, `reddit`, `github`, `substack`, `threads`, `instagram`, `facebook`, `mail`, `website` |
| `[params.chain]` | `token_contract`, `deposit_contract`, `staking_contract`, `staking_nft_contract`, `debook_contract`, `multisig_treasury`, `tech_subdao_multisig` |

Access in templates as `.Site.Params.like.uniswap`, `.Site.Params.social_media.x`, etc.

## Styling Conventions

CSS is in `assets/css/main.css` using TailwindCSS v4's `@theme` directive. Color and font variables:

```css
--color-primary: #28646e
--color-primary-light: #3a8a97
--color-primary-dark: #1a4a52
--color-secondary: #50e3c2
--color-accent: #f7931a
--color-brand-text: #4a4a4a
--color-brand-muted: #9b9b9b
--color-brand-bg: #f7f7f7
--color-brand-bg-alt: #d7ecec
```

Dark mode is toggled via the `dark` class on `<html>` (stored in `localStorage`). Dark mode variant uses `@variant dark (&:where(.dark, .dark *))`.

Utility classes defined in `main.css`:
- `.btn`, `.btn-primary`, `.btn-secondary`, `.btn-accent`, `.btn-white`, `.btn-outline`
- `.card` — card component style
- `.section` — section padding
- `.container-custom` — max-width container
- `.prose` — rich text styling

## SEO & Head

- Page `<title>`: `{{ .Title }}` for homepage; `{{ .Title }} | LikeCoin` for all other pages
- `meta description` from frontmatter `description`
- OpenGraph/Twitter Card via Hugo built-in partials (`opengraph.html`, `twitter_cards.html`)
- `canonical` set to each language's own permalink
- `hreflang` alternate links only output when both translations exist
- Sitemap and `robots.txt` auto-generated by Hugo
- RSS feeds per language; links in footer
- Google Analytics tag: `G-G9WM2V5EW6`

## Image Guidelines

- Hero images: **no lazy loading** (`loading` attribute omitted)
- List/gallery images: use `loading="lazy"`
- `alt` text: use `image_alt` frontmatter if present, otherwise fall back to `title`
- Shared static images live in `static/assets/`

## Deployment

- **Trigger:** push to `main` branch (or manual `workflow_dispatch`)
- **Pipeline:** `npm ci` → `npm run build` → upload `public/` as GitHub Pages artifact → deploy
- Hugo is invoked with `--minify` in production
- CSS fingerprinting is enabled in production (`minify | fingerprint | resources.PostProcess`)
- `enableGitInfo = true` so Hugo can read last-modified dates from Git history

## Known Patterns & Gotchas

1. **Hugo is not globally installed** — always use `npm run dev` / `npm run build`, or `node_modules/.bin/hugo` directly.
2. **TailwindCSS v4** — uses `@import "tailwindcss"` and `@theme {}` (not the v3 `tailwind.config.js` approach). Config is in `postcss.config.js`.
3. **`3ook_com` frontmatter key** — the homepage section key contains a dot/number prefix (`3ook_com`); accessed in templates as `index .Params "3ook_com"`.
4. **Content language files** — Chinese files use `.zh.md` suffix in the same directory, not a separate `zh/` folder.
5. **Hugo build stats** — `hugo_stats.json` is generated at build time and mounted back into assets for TailwindCSS class scanning (see `[build.cachebusters]` in `hugo.toml`).
6. **FontAwesome** — loaded from CDN (not npm). Use `fa-solid`, `fa-brands` class prefixes.
7. **`unsafe = true`** in `[markup.goldmark.renderer]` — raw HTML is allowed in Markdown content.

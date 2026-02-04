# dao.like.co

The official website for [LikeCoin DAO](https://dao.like.co/), the decentralized protocol powering [3ook.com](https://3ook.com) - a decentralized bookstore.

## Features

- **Multilingual Support**: English and Traditional Chinese (繁體中文)
- **Responsive Design**: Mobile-first design with TailwindCSS
- **Dark Mode**: Built-in theme switching
- **SEO Optimized**: Meta tags, Open Graph, Twitter Cards, sitemap, and RSS
- **Static Site**: Built with Hugo for fast performance and easy deployment

## Tech Stack

- [Hugo](https://gohugo.io/) - Static site generator (managed via [hugo-bin](https://www.npmjs.com/package/hugo-bin))
- [TailwindCSS v4](https://tailwindcss.com/) - Utility-first CSS framework
- [PostCSS](https://postcss.org/) - CSS processing
- [FontAwesome](https://fontawesome.com/) - Icons

## Prerequisites

- Node.js (v20 or higher)
- npm

## Development

### Install Dependencies

```bash
npm install
```

### Run Development Server

```bash
npm run dev
```

The site will be available at http://localhost:1313/

### Build for Production

```bash
npm run build
```

The built site will be in the `public/` directory.

### Clean Build Files

```bash
npm run clean
```

## Project Structure

```
dao-like-co/
├── archetypes/          # Content templates
├── assets/
│   └── css/             # CSS source files (TailwindCSS)
├── content/             # Markdown content
│   ├── _index.md        # English homepage
│   ├── _index.zh.md     # Chinese homepage
│   └── declaration/     # Declaration page (leaf bundle)
│       ├── index.md
│       └── index.zh.md
├── i18n/                # Translation strings
│   ├── en.yaml
│   └── zh.yaml
├── layouts/             # HTML templates
│   ├── index.html       # Homepage template
│   ├── _default/        # Default templates (baseof, list, single)
│   └── partials/        # Reusable components (head, navbar, footer, css)
├── static/              # Static files (images, favicon)
│   ├── assets/
│   └── favicon.ico
├── .github/workflows/   # GitHub Actions deployment
├── hugo.toml            # Hugo configuration
├── postcss.config.js    # PostCSS configuration
└── package.json         # Node dependencies
```

## Content Management

### Homepage Content

Edit `content/_index.md` (English) or `content/_index.zh.md` (Chinese).

Page-specific data lives in frontmatter:

```yaml
---
title: LikeCoin - Powering 3ook.com
date: 2026-01-20
description: Page description for SEO
image: /assets/likecoin-banner.png
punchline: LikeCoin is the DeBook protocol powering 3ook.com, a decentralized bookstore.
3ook_com:
  title: 3ook.com
  bio: Description of 3ook.com...
---

Markdown content for the "What is LikeCoin" section.
```

Site-wide data (shared across homepage sections, footer, etc.) lives in `hugo.toml` under `[params]`:

- `[params.like]` - Uniswap, CoinGecko, CoinMarketCap links
- `[params.get_involved]` - GitHub, Governance, Docs links
- `[params.social_media]` - YouTube, X, Reddit, Substack, etc.
- `[params.chain]` - Contract addresses and related links

### Declaration Page

Edit `content/declaration/index.md` (English) or `content/declaration/index.zh.md` (Chinese).

### Adding New Pages

Create a new directory under `content/` with:
- `index.md` for English
- `index.zh.md` for Chinese

Both files should have proper frontmatter:

```yaml
---
title: Page Title
date: 2026-01-20
description: Page description for SEO
image: /path/to/image.png
---

Your markdown content here...
```

### Translations (i18n)

UI strings are managed via Hugo's i18n system in `i18n/en.yaml` and `i18n/zh.yaml`. Templates use `{{ T "key" }}` to render the correct translation. To add or update a UI string:

1. Add the key to both `i18n/en.yaml` and `i18n/zh.yaml`
2. Use `{{ T "key_name" }}` in templates

## Customization

### Colors

Edit `assets/css/main.css` to change the color theme:

```css
@theme {
  --color-primary: #28646e;
  --color-secondary: #50e3c2;
  --color-accent: #f7931a;
}
```

### Fonts

Modify the font families in `assets/css/main.css` or use custom fonts by adding them to the `static/` directory.

## Deployment

The site deploys automatically to GitHub Pages via GitHub Actions on push to `main`. See `.github/workflows/deploy.yml`.

To deploy manually:

1. Build the site: `npm run build`
2. The `public/` directory contains the static site
3. Deploy the contents of `public/` to your hosting service

## License

GPL-3.0

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

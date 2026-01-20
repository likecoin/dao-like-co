# dao.like.co

The official website for [LikeCoin DAO](https://dao.like.co/), the decentralized protocol powering [3ook.com](https://3ook.com) - a decentralized bookstore.

## Features

- **Multilingual Support**: English and Traditional Chinese (繁體中文)
- **Responsive Design**: Mobile-first design with TailwindCSS
- **Dark Mode**: Built-in theme switching
- **SEO Optimized**: Meta tags, Open Graph, Twitter Cards, sitemap, and RSS
- **Static Site**: Built with Hugo for fast performance and easy deployment

## Tech Stack

- [Hugo](https://gohugo.io/) - Static site generator
- [TailwindCSS v4](https://tailwindcss.com/) - Utility-first CSS framework
- [PostCSS](https://postcss.org/) - CSS processing

## Prerequisites

- Node.js (v18 or higher)
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
│   └── css/             # CSS source files
├── content/             # Markdown content
│   ├── _index.md        # English homepage
│   ├── _index.zh.md     # Chinese homepage
│   └── declaration/     # Declaration pages
├── layouts/             # HTML templates
│   ├── _default/        # Default templates
│   └── partials/        # Reusable components
├── static/              # Static files (images, etc.)
├── hugo.toml            # Hugo configuration
├── postcss.config.js    # PostCSS configuration
└── package.json         # Node dependencies
```

## Content Management

### Homepage Content

Edit `content/_index.md` (English) or `content/_index.zh.md` (Chinese).

The frontmatter contains all the data for the homepage sections:

```yaml
---
title: LikeCoin - Powering 3ook.com
punchline: Your punchline here
whitepaper: https://...
3ook_com:
  title: Your AI Reading Companion
  bio: Description...
get_involved:
  github: https://github.com/likecoin
  governance: https://...
social_media:
  youtube: https://...
  x: https://...
chain:
  token_contract: https://...
---
```

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

All Markdown content is automatically styled with beautiful typography. See [MARKDOWN.md](MARKDOWN.md) for details on markdown styling.

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

### GitHub Pages

1. Build the site: `npm run build`
2. The `public/` directory contains the static site
3. Deploy the contents of `public/` to your hosting service

### Automated Deployment

The site can be deployed automatically using GitHub Actions. Create `.github/workflows/deploy.yml`:

```yaml
name: Deploy Hugo site

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 20
      - run: npm ci
      - run: npm run build
      - uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./public
```

## License

GPL-3.0

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

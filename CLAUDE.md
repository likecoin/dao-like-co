# dao.like.co 網站設計

主要目標：介紹 LikeCoin, 與 3ook.com 關係，與各個重要連結
可維運性：Markdown 編輯 + GitHub Actions 自動部署。網站內容可直接以 Markdown + YAML 編輯
圖片儲存：共用圖片放在 `static/assets/`

## 網站架構

### Navbar

Left sided

* LikeCoin logo

Right sided

* 3ook.com logo
* language switcher

### 首頁

* Hero image
    * title
    * punch line
* What is LikeCoin
    * 簡短介紹 + 圖
    * Declaration
    * Whitepaper -> link to 3ook.com
* 3ook.com 介紹
    * description + 圖
    * Book.com link
* Get Involved
    * Community Call
    * GitHub
    * Governance (snapshot.io)
* Follow us (social media links)
    * YouTube
    * X
    * Reddit
    * GitHub
* Chain info
    * Contract address
    * CoinMarketCap / CoinGecko
    * Uniswap

### Footer

* 3ook.com
* social media
* GitHub
* Chain Info
    * Contract address
    * CoinMarketCap / CoinGecko
    * Uniswap

### Declaration

/declaration, single page render from markdown.


## 技術棧

- NPM
- Hugo (managed by NPM, hugo-bin)
- TailwindCSS
- FontAwesome

### i18n

- 預設語言: en-US
- 第二語言: zh，路徑為 /zh/
- 所有頁面採用 Leaf Bundle，將不同語言置於同一資料夾下的 index.md 與 index.zh.md
- URL 使用 bundle title，兩語系共用同樣名稱
- 使用 hugo languages 設定，defaultContentLanguageInSubdir=false
- 缺翻譯時就不在列表中顯示，不需要 fallback
- 每頁需輸出 rel="alternate" hreflang="en-us" / zh-Hant 的互指。
  - 只在兩語系皆存在該內容時，輸出對應 hreflang alternate；缺語系則省略該 hreflang。
- canonical 以各語系自身 URL 為主，避免 SEO 重複內容。
- 內容欄位各語系獨立（title/description/location）
- UI 字串使用 Hugo i18n 系統：`i18n/en.yaml` 和 `i18n/zh.yaml`，模板中使用 `{{ T "key" }}`

## Content model

- date format (date/modified): YYYY-MM-DD
- featured: boolean
- image: relative path in current dir
- images: list of strings (relative path in current dir)

Common properties:
- title: required
- date: required
- modified: optional, fallback to date
- image: required
- description: required
  - explain the content, used by SEO / OpenGraph
  - max to 160 characters
- summary: optional, fallback to auto summary
  - used by list pages
- tags: optional
- categories: optional

Use YAML for all frontmatter (for compatibility in Obsidian)

Content files:

- `_index.md`: Home page /
- `_index.zh.md`: Home page Chinese version /zh/
- `declaration/index.md`: Declaration page /declaration/
- `declaration/index.zh.md`: Declaration Chinese version /zh/declaration/

### Homepage frontmatter

Page-specific data in frontmatter:

```yaml
title: LikeCoin - Powering 3ook.com
date: 2026-01-20
description: Page description for SEO
image: /assets/likecoin-banner.png
punchline: LikeCoin is the DeBook protocol powering 3ook.com, a decentralized bookstore.
3ook_com:
  title: 3ook.com
  bio: Description of 3ook.com...
```

### Site-wide params in hugo.toml

Shared data used across homepage sections, footer, and navbar lives in `hugo.toml` under `[params]`:

```toml
[params]
whitepaper = "https://3ook.com/store/..."

[params.like]
uniswap = "https://app.uniswap.org/..."
coingecko = "https://www.coingecko.com/..."
coinmarketcap = "https://coinmarketcap.com/..."

[params.get_involved]
github = "https://github.com/likecoin"
governance = "https://snapshot.box/#/s:likerland.eth"
docs = "https://docs.3ook.com"

[params.social_media]
youtube = "https://youtube.com/likecoin"
x = "https://x.com/likecoin"
reddit = "https://reddit.com/r/likecoin"
github = "https://github.com/likecoin"
substack = "https://likecoin.substack.com"
threads = "https://threads.com/@3ookcom"
instagram = "https://instagram.com/3ookcom"
facebook = "https://facebook.com/likecoin"
mail = "mailto:cs@3ook.com"
website = "https://like.co"

[params.chain]
token_contract = "https://basescan.org/token/..."
deposit_contract = "https://basescan.org/token/..."
staking_contract = "https://basescan.org/address/..."
staking_nft_contract = "https://opensea.io/collection/..."
debook_contract = "https://basescan.org/address/..."
multisig_treasury = "https://app.safe.global/..."
tech_subdao_multisig = "https://app.safe.global/..."
```

## Layouts

- index.html
- default/list.html
- default/single.html

### Partials

- head.html
- navbar.html
- footer.html
- css.html

### Themes

Use variables for
- colors
  - primary
  - secondary
  - accent
- fonts
  - heading
  - body

Support light/dark mode switching

## SEO / Social / Sitemap / RSS

- 每頁的 title 組合規則: `{{ .Title }} | LikeCoin`
- meta description：來自 summary
- Open Graph / Twitter card：og:image 使用 frontmatter 的 image
- 生成 sitemap、robots.txt
- RSS：提供全站 RSS，依語言區分，連結置於 footer

## 圖片規格

- lazy loading 規則
  - 首屏 hero 不 lazy
  - list 卡片 lazy
  - gallery grid lazy
- alt 文案來源
  - image_alt 若有用它
  - 否則 fallback title

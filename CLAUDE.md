# dao.like.co 網站設計

主要目標：介紹 LikeCoin, 與 3ook.com 關係，與各個重要連結
可維運性：Markdown 編輯 + GitHub Actions 自動部署。網站內容可直接以 Markdown + YAML 編輯
圖片儲存：所有圖片直接放入 page bundle

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
- `declaration.md`: Declaration page /declaration/
- `declaration.zh.md`: Declaration Chinese version /zh/declaration/

Properties for home page:

```yaml
title: LikeCoin - Powering 3ook.com
punchline: LikeCoin is the DeBook protocol powering 3ook.com, a decentralized bookstore.
whitepaper: https://3ook.com/store/0x3ddc416c403449bc2a9ae873bff0b687993cd662
3ook_com: 
  title: Your AI Reading Companion
  bio: 3ook.com is an AI-powered book platform that transforms how you experience books - books can be both read and heard, narrated by AI in different voice actor or author's voice.
like:
  uniswap: https://app.uniswap.org/explore/tokens/base/0x1ee5dd1794c28f559f94d2cc642bae62dc3be5cf
  coingecko: https://www.coingecko.com/en/coins/likecoin
  coinmarketcap: https://coinmarketcap.com/currencies/likecoin/
get_involved:
  github: https://github.com/likecoin
  governance: "https://snapshot.box/#/s:likerland.eth"
  docs: https://docs.3ook.com 
social_media:
  youtube: https://youtube.com/likecoin
  x: https://x.com/likecoin
  reddit: https://reddit.com/r/likecoin
  github: https://github.com/likecoin
  substack: https://likecoion.substack.com
  threads: https://threads.com/@3ookcom
  instagram: https://instagram.com/3ookcom
  facebook: https://facebook.com/likecoin
  mail: mailto:cs@3ook.com 
  website: https://like.co
chain:
  token_contract: https://basescan.org/token/0x1EE5DD1794C28F559f94d2cc642BaE62dC3be5cf
  deposit_contract: https://basescan.org/token/0xE55C2b91E688BE70e5BbcEdE3792d723b4766e2B
  staking_contract: https://basescan.org/address/0x4506ac2dd1e9a470d92a3d1656e1a99c676e1c8e
  staking_nft_contract: https://opensea.io/collection/likestakeposition
  debook_contract: https://basescan.org/address/0xfb5cbb1973a092E6C77af02EA1E74B14870AbeC5
  multisig_treasury: https://app.safe.global/home?safe=base:0x3aFaEA57802E0b8380d6461a62efbd5346752e83
  tech_subdao_multisig: https://app.safe.global/home?safe=base:0x07f7355F73af2Ea47CDf0DE97853776bd833D67b
```

## Layouts

- index.html
- default/list.html
- default/single.html

### Partials

- header.html
- footer.html
- navbar.html
- grid-card.html
- list-card.html

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

- 卡片圖片裁切策略
  - posts list 4:3：crop/cover，裁切焦點預設 center，可用 image_anchor 覆蓋（optional）
- 輸出尺寸（至少規定 2-3 段）
  - card: 480 / 960
  - hero: 960 / 1440 / 1920
- lazy loading 規則
  - 首屏 hero 不 lazy
  - list 卡片 lazy
  - gallery grid lazy
- alt 文案來源
  - image_alt 若有用它
  - 否則 fallback title

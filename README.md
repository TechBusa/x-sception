# X-SEPTION catalogue

The website of X-SEPTION, luxury menswear since 1991, at Nelson Mandela Square (Sandton) and Eastgate (Bedfordview). It is a single-page catalogue on GitHub Pages: a filterable product grid (photo, brand, name, category) where every enquiry goes to WhatsApp. Pieces come from Instagram posts.

Live site: https://x-seption.co.za/

## What is in this repository

| Path | What it is |
|---|---|
| `index.html` | The whole site: layout, styles and script in one file |
| `assets/hero/` | Store photos for the slideshow and the store cards (`-m` = phone copy) |
| `assets/products/` | Product photos: `ID-N.webp` (large) and `ID-N-s.webp` (grid) |
| `assets/icons/` | X star icons for phone home screens and Google Search (the browser tab icon is built into `index.html`) |
| `catalogue.json` | The list of Instagram posts shown on the site, written by the daily sync |
| `scripts/sync-instagram.mjs` and `.github/workflows/sync-instagram.yml` | Optional daily Instagram sync |
| `llms.txt` | Plain summary of the boutique for AI search tools |
| `google0358e8a493fefd4f.html` | Google Search Console verification. Do not delete |
| `SETUP.md` | Full setup and upkeep guide |

## Adding a piece by hand

1. Save each photo as WebP in `assets/products/`, named `ID-1.webp`, `ID-2.webp` and so on (about 1600 px on the long side), plus a small copy `ID-1-s.webp` (about 720 px) for the grid. A missing small copy just means the big one is used.
2. Open `index.html` > Edit, search for `var PRODUCTS` and add one line:

```
{ id: 'jg-penny-loafer-navy', name: 'Penny loafer, navy', brand: 'Jo Ghost', group: 'shoes', line: 'formal', imgs: ['assets/products/jg-penny-loafer-navy-1.webp', 'assets/products/jg-penny-loafer-navy-2.webp'], shape: ['square', 'wide'] },
```

- Groups: `tees`, `shirts`, `knitwear`, `sets`, `jackets`, `jeans`, `pants`, `shoes`, `boots`, `sneakers`, `underwear`, `socks`.
- Line: `formal`, `casual` or `any`. Status (optional): `new`, `low`, `sold`.
- `shape` per photo: `wide` footwear photos are shown whole in the grid, everything else fills the card.
- `brand` must match a name in the Brands section, so tapping that brand's logo finds the piece. Leave it `''` if unknown.
- Delete the line when the piece is gone.

### Prices and sales

```
{ id: 'agv-bear-tee-white', name: 'Bear back-print tee, white', brand: 'Avenue George V Paris', group: 'tees', line: 'casual', price: 1600, was: 3200, sale: 'Black Friday', imgs: ['assets/products/agv-bear-tee-white-1.webp'] },
```

`price` alone shows a price. With `was` and `sale` the card shows the old price struck through, a `-50%` badge, and the piece joins the sale filter (named after `sale`). The Black Friday slideshow, ticker and offer text are in `CONFIG.hero` and `CONFIG.offer` in `index.html`. When Black Friday ends, remove `was` and `sale` from those lines and replace the three `trio` slides.

Links can open pre-filtered: `?cat=sale`, `?cat=boots`, `?line=formal`, `?brand=Jo%20Ghost`, or one piece with `?piece=jg-penny-loafer-navy`.

Curated. Not Crowded.

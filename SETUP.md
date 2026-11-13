# X-SEPTION catalogue: setup and upkeep

## 1. Files and where they go
Repo: `TechBusa/x-sception`, branch `main` (GitHub Pages, custom domain x-seption.co.za via the `CNAME` file).

| File | Path in the repo |
|---|---|
| index.html | `index.html` (replaces the current page) |
| catalogue.json | `catalogue.json` |
| sync-instagram.mjs | `scripts/sync-instagram.mjs` |
| sync-instagram.yml | kept in `scripts/` until the sync is set up, then moved to `.github/workflows/` (section 4) |
| apple-touch-icon.png, icon-192.png | `assets/icons/` |
| llms.txt, README.md | repo root (replace the old ones) |

On a phone: open the repo in the browser, tap the 3 dots (⋯) > Add file > Create new file, type the full path (folders included), paste the content, then Commit changes.

## 2. How the edit is organised
Twelve groups, shown as filter chips and as photo tiles under Shop by category. Every product is a card with its own photos.

| Section | Groups |
|---|---|
| Tops & jackets | `tops`, `jackets` |
| Jeans & pants (Bottoms) | `jeans`, `pants`, `underwear` (Bruno Banani underwear and swimwear) |
| Shoes & sneakers | `shoes`, `sneakers`, `socks` (XPOOOS) |


### Houses on the floor
| House | At X-SEPTION | Line |
|---|---|---|
| A Fish Named Fred | Shirts and coordinated looks | Formal and casual |
| ALBERTO | Pants and denim | Formal and casual |
| Avenue George V Paris | Tops | Casual |
| Bruno Banani | Underwear and swimwear | Shows under both |
| bugatti | Shirts, chinos and trousers | Formal and casual |
| Cesare Paciotti | Shoes | Formal and casual |
| Jo Ghost | Formal shoes and casual pairs | Formal and casual |
| Milestone | Outerwear jackets | Semi-formal to casual (shows under both) |
| Psycho Bunny | Polos, shirts and caps | Casual |
| R2 Amsterdam | Shirts | Formal and casual |
| Rock Revival | Denim | Casual |
| Venturo | Minimalist, tailored cuts | Casual |
| XPOOOS | Premium fashionable socks | Shows under both |

All posts sit in one grid. Each group that has posts becomes a filter chip (with a count) above the grid; empty groups are hidden. The Formal / Casual switch filters on top of that. With nothing to show, the grid shows one line with WhatsApp and Instagram buttons.

## 3. Add a piece by hand (works today, no setup)
See **Adding a piece by hand** and **Prices and sales** in README.md.

## 4. Automatic sync from Instagram (optional, about 30 minutes once)
Needs admin access to X-SEPTION's Meta Business Suite (the integration flag in the Brand Kit report).

1. Instagram account set to Professional and linked to the X-SEPTION Facebook Page in Meta Business Suite.
2. developers.facebook.com > Create app > Business. Add the Instagram product (API setup with Facebook login).
3. Business Settings > Users > System users > Add (Admin). Assign the app, the Page and the Instagram account. Generate a token with `instagram_basic`, `pages_show_list`, `pages_read_engagement`, `business_management`. Set expiry to Never.
4. Find the Instagram account ID in Graph API Explorer: `me/accounts?fields=instagram_business_account`.
5. Move `scripts/sync-instagram.yml` to `.github/workflows/sync-instagram.yml` (GitHub: open the file > Edit > change the path in the name box > Commit).
6. GitHub repo > Settings > Secrets and variables > Actions > New repository secret: `IG_ACCESS_TOKEN` and `IG_USER_ID`.
7. Actions tab > Sync Instagram edit > Run workflow. After that it runs every day at 06:17 SAST and commits only when the list changed.
8. Each listed post's photo is saved once into `assets/products/<post code>.jpg` and used for its catalogue card. Video posts use their cover frame; carousels use the first photo.

The token stays in GitHub secrets. The website never calls the Instagram API or loads Instagram's embed; it reads `catalogue.json` and the saved photos.

### Caption tags for the sync
First caption line is the piece name (keep it short, e.g. "Tapered leather ankle boot"). Name the brand in the caption.

| Purpose | Tags |
|---|---|
| Group (required) | `#xsTees` `#xsShirts` `#xsKnitwear` `#xsSets` `#xsJackets` `#xsJeans` `#xsPants` `#xsShoes` `#xsBoots` `#xsSneakers` `#xsSocks` `#xsUnderwear` or `#xsSwimwear` |
| Formal or casual (optional) | `#xsFormal` `#xsCasual` |
| Wardrobe zone (optional) | `#xsBeach` `#xsBoardwalk` `#xsSmartCasual` `#xsBoardroom` `#xsAfterHours` |
| Status | `#xsNew` `#xsLow` `#xsSoldOut` `#xsArchive` |
| Keep past 45 days | `#xsKeep` |

Example caption:
```
Jo Ghost tapered leather ankle boot
Limited sizes at Eastgate.
#xsShoes #xsAfterHours #xsNew
```

Posts without a group tag (store news, events) are never listed. Sold out: add `#xsSoldOut`. Gone for good: add `#xsArchive` or delete the post. Posts older than 45 days drop off unless tagged `#xsKeep`, so the page never shows stale stock. A line in `DROPS` overrides the synced version of the same post.

## 5. Links to use in GBP and Instagram
- Formal edit: `https://x-seption.co.za/?line=formal#shoes`
- Casual edit: `https://x-seption.co.za/?line=casual#tops`
- One post: `https://x-seption.co.za/?piece=POST_CODE` (opens the Ask sheet for that post)

## 6. Before go-live
- Keep `google0358e8a493fefd4f.html` in the repo root: it is the Google Search Console verification.
- The X star tab icon is built into `index.html`. The two files in `assets/icons/` add the icon for phone home screens and for Google Search results.
- After the new `index.html` is live, the old files `assets/logoxt.png` and `assets/xegstorebg1.webp` to `xegstorebg4.webp` are no longer used and can be deleted (about 1.6 MB).
- Nelson Mandela Square is Shop 39, Retail Lower Level (confirmed 8 Oct 2026). Use the same wording on Google Business Profile and directories.
- When the site moves to x-seption.com, update the canonical, `og:url` and JSON-LD URLs in the head.

## 7. Page layout (Nov 2026)
Top to bottom:
1. **Slideshow**: three Black Friday slides of three photos each (the last cell of slide 3 is the 50% off card), then the Nelson Mandela Square and Eastgate store photos. Each photo opens its piece. It auto-advances every 6 seconds, pauses on hover, and can be swiped. Edit it in `CONFIG.hero`.
2. **Offer ticker**: the Black Friday line scrolling under the slideshow (`CONFIG.offer`).
3. **Shop by category**: one photo tile per group (`CONFIG.tiles`).
4. **Catalogue**: filter chips (Black Friday first), Formal / Casual switch, 24 pieces then Show more. Cards swap to the second photo on hover.
5. **Lookbook**: a full-width mosaic of the outdoor shots (`CONFIG.lookbook`).
6. **Brands** (tap to filter), **Visit us** (store photos, directions, WhatsApp), footer.

Daytime (light) is the default look; the moon button switches to night and the choice is remembered on that phone.

Product names, groups and brands were read from the 13 Nov 2026 photo shoot. The shop should check them, especially the pieces with no brand (floral shirts, line-art shirt, blazer, printed polos and tees, knit polos): fill `brand` in where known.

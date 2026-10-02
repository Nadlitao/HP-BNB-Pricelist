# HP Commercial Notebook Pricelist (Iontech) — static website

Open `index.html` in any modern browser, or upload this whole folder to any static web host
(Netlify, GitHub Pages, cPanel `public_html`, S3, etc.). No server, database or login is needed.

## Folder layout

| Path | What it is |
|---|---|
| `index.html` | Page structure (header, hero, pricelist, promotions, inquiry form, footer) |
| `assets/css/styles.css` | All styling (colors are tokens at the top of the file) |
| `assets/js/app.js` | Search, filters, sorting, grid/pricelist views, compare, product details, inquiry form |
| `data/products.js` | **Products, DP, SRP and On Hand** — generated from the Excel price guide |
| `data/site.js` | **Contact details, categories and promotions** — edit by hand |
| `assets/img/products/` | Product photos, named by HP part number: `<PART>_1-Front.webp`, `_2-FrontRight`, `_3-FrontLeft` (or `_3-TentMode`), `_4-RearLeft`, plus `-sm` thumbnails |
| `assets/img/promos/` | Promotion images |
| `assets/img/brand/` | HP and Iontech logos (header, hero, contact card, footer, browser tab icon) |
| `tools/update_from_excel.py` | Rebuilds `data/products.js` from the workbook |

## Updating prices and stock (recommended)

1. Update the **Stock Summary** sheet of the price guide (Part Number, Model, Dealer Price, SRP, On hand Stocks).
2. Run, from this folder:
   ```
   pip install openpyxl
   python tools/update_from_excel.py "C:\path\to\Iontech BNB Price Guide - September Updated 2026.xlsx"
   ```
3. Re-upload `data/products.js`.

The script prints any part number that has no photo yet and any duplicate part numbers (only the first row is kept).

You can also edit `data/products.js` directly — each product is one block with `dp`, `srp` and `onHand`.

**Stock status is automatic:** `onHand > 0` → **IN STOCK** (with the unit count); `onHand = 0` → **ORDER BASIS**.

## Adding a new product photo

Put one to four images into `assets/img/products/` named with the part number, e.g. `ABC12PT_1-Front.webp` …
(see existing files for the size: 1200×900 large, 480×360 `-sm`), then re-run the update script; it lists
whatever views exist for each part number. A product with a single photo shows it without the thumbnail strip.

## Changing contact details or promotions

Edit `data/site.js`. Each promotion has a title, image, period, mechanics, eligible-model text and
requirements. Products show a promotion when its `id` is listed in the product's `promos` array.

## Inquiry form

The form has no backend: **Request Quotation** opens the visitor's email app with the inquiry addressed to the
sales email in `data/site.js`, and also shows a **Copy inquiry** button as a fallback. To receive submissions
without email apps, point the form to a form service (e.g. Formspree) in `assets/js/app.js` → `onSubmit`.

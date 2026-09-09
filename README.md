# VARDHMAN EXIM Website — Editing Guide

This is a static website (HTML + CSS + JavaScript only — no build tools,
no server, no database). It works as-is on GitHub Pages.

## Files in this folder

```
index.html              <- the entire website (structure, styles, logic)
images/products/        <- all 15 product photos (used for the collection
                            grid, the gallery section, AND the quick-view
                            popup — one photo per design, reused in all
                            three places)
README.md               <- this file
```

---

## A. Which file do I edit to change products?

**`index.html`** only. Open it in any text editor (Notepad, VS Code, or
even GitHub's own web editor). Scroll to the bottom, just above
`</body>`, to the `<script>` block. Near the top of that script you'll
see:

```js
var PRODUCTS = [
  { code: "VE-KS-001", img: "images/products/ve-ks-001.jpg" },
  { code: "VE-KS-002", img: "images/products/ve-ks-002.jpg" },
  ...
];
```

This one array controls every product card, the gallery grid, and the
quick-view popup. You never need to touch the HTML for products — the
page builds those sections automatically from this list.

## B. Where exactly do I add a new product?

Add a new line inside the `PRODUCTS` array, e.g. to add design 016:

```js
  { code: "VE-KS-016", img: "images/products/ve-ks-016.jpg" },
```

Put it anywhere in the list (order = display order). Save the file.
That's it — the new card, its gallery photo, its "Enquire on WhatsApp"
link (auto-built with the right design code), and its quick-view popup
are all generated automatically.

To remove a design, delete its line from the array.

## C. How do I replace a product image?

Two options:

1. **Same design, new photo:** just replace the file in
   `images/products/` with a new photo that has the **exact same
   filename** (e.g. overwrite `ve-ks-003.jpg`). Nothing else to change.
2. **New design / new filename:** add the photo file to
   `images/products/`, then add or update its line in the `PRODUCTS`
   array to point to that filename.

Recommended photo specs to match the existing set: portrait orientation,
roughly a 2:3 ratio (e.g. 800×1200px), JPG format.

## D. How do I change the WhatsApp number?

The number `918000366184` (WhatsApp format, no `+` or spaces) and
`+918000366184` (used in `tel:` call links) appear in a few places:
the JS `WA_NUMBER` variable near the top of the script (this drives
every product card and the quick-view popup), plus the header icon,
hero button, mobile bottom bar, wholesale CTA section, and contact
section, which are written directly in the HTML.

**Easiest method:** open `index.html` in a text editor and use
Find & Replace:
- Replace all `918000366184` → your new number (covers every
  `wa.me/...` link and every `tel:+...` link, since both contain this
  digit string)
- Also replace the human-readable version `+91 80003 66184` (appears
  once, in the Contact section) with your new number in that format

## E. How do I change company/contact information?

Also in `index.html` — search for the text you want to change (e.g.
`Surat, Gujarat, India`, `pj664847@gmail.com`, the About paragraphs, or
the footer text) and edit it directly. This site has no separate
"settings" file — all visible text lives in the HTML, so search-and-edit
is the way to update it.

## F. What do I upload to GitHub?

Upload **everything in this folder**, keeping the folder structure
intact:

```
index.html
images/products/  (all 15 .jpg files)
```

`index.html` must stay at the root of your GitHub Pages branch (or
inside `/docs` if that's what your repo is configured to serve), and
the `images` folder must sit next to it at the same level. Don't
rename the `images` folder or move `index.html` to a different folder
relative to it — the site refers to the images with relative paths like
`images/products/ve-ks-001.jpg`.

---

## What changed from the previous version (summary)

- **Mobile menu fixed** — the hamburger button previously had no code
  behind it and did nothing; it now opens/closes properly.
- **Product grid bug fixed** — 10 of the 15 products were previously
  sitting outside the grid container (a missing/misplaced closing
  tag), so they lost their 3-column layout. All 15 now display
  correctly.
- **Images moved out of the HTML** — all 30 photos were previously
  embedded as base64 text directly inside `index.html`, making it a
  9MB file. They're now real `.jpg` files in `images/products/`,
  which loads faster, lets browsers cache them, and lets you swap a
  photo by replacing a file instead of re-encoding it.
- **No more horizontal scrolling on small phones** — grid layouts with
  photos could force the page slightly wider than the screen at
  320–390px; this is fixed.
- **Cleaned up CSS** — removed duplicate/conflicting style blocks that
  had piled up from earlier edits, and dead unused styles.
- **Added SEO/social tags** — Open Graph and Twitter Card meta tags
  (using your existing hero photo), and structured data (JSON-LD)
  with your real business name, phone, email and location — nothing
  invented.
  - I did **not** add a canonical URL tag since I don't know your live
    domain. Once your site is live, you can add this line inside
    `<head>`:
    `<link rel="canonical" href="https://your-domain.com/"/>`
- Nothing else was invented — all copy, prices-on-request policy,
  specs, and business details are exactly what was in your original
  file.

## A note on "gallery" photos

The Collection Gallery section previously stored a *second* full copy
of every photo (base64, again) — but it turned out to be the exact
same 15 photos as the product grid, just duplicated. The rebuilt site
reuses the single copy in `images/products/` for both the product
cards and the gallery, so you only ever need to manage one photo per
design.

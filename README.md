# Your Apartment Catania — Digital Guest Book

A small, mobile-first website that gives guests of *Your Apartment Catania* an
app-like guidebook: a home screen with an icon menu, and a separate screen for
each section (check-in, Wi-Fi, house info, things to do, …).

Re-skinned from the sibling Malta guidebook — same architecture, Catania
content and brand tokens.

**Style**
- **Titles** → *Playfair Display* (serif)
- **Accents / signature** → *Caveat* (script — e.g. "Enjoy your stay!")
- **Body & labels** → *Poppins* (sans-serif)
- Paper-white background, near-black ink, Etna-red accents, dark brand footer.

---

## Run / preview locally

Plain static site — no build step. From this folder:

```bash
python3 -m http.server 8000
# then open http://localhost:8000
```

## Deploy

GitHub Pages from `main` / root. Push and enable Pages in the repo settings.

Only `index.html` + `assets/` are needed in production. `.claude/` (local
preview tooling) and `assets/img/from-host/` (raw originals) are gitignored.

---

## Structure

```
index.html              All 15 screens + the inline SVG icon set + a tiny hash router
assets/css/style.css    All styling and the design tokens (colors/fonts) at the top
assets/js/app.js        Screen navigation, sticky top bar, copy-password button,
                        section nav (Things to do), Wi-Fi QR generation
assets/js/qrcode.min.js Vendored QR library (davidshimjs/qrcodejs, MIT)
assets/img/             Optimised photos
```

Navigation uses URL hashes (`#wifi`, `#emergency`, …), so the browser **Back**
button works and any screen can be linked directly.

## Re-skinning (brand tokens)

The first ~70 lines of `assets/css/style.css` are the public brand API —
typography, palette, radii. Edit those + the Google Fonts URL in
`index.html`'s `<head>` to re-skin the site; component CSS stays untouched.

## Editing content

Text lives in `index.html`, grouped by screen in clearly-commented `<section>`
blocks. Common edits:

| What | Where |
|------|-------|
| Wi-Fi name / password | `id="wifi"` in `index.html` (displayed value **and** `data-copy="…"`) **and** the QR text in `assets/js/app.js` (`text: 'WIFI:T:WPA;S:…;P:…;;'`). Update all three so the QR stays in sync. |
| Host phone / WhatsApp | search `wa.me/` and `tel:` (currently placeholder `+390000000000` — replace with Stefano's real number) |
| Address | search `Via Caronda` |
| Any section text | find the matching `<!-- SECTION -->` comment |

## Photos

Apartment, host and logo photos in `assets/img/` come from Stefano. POI photos
(Etna, Taormina, Aci Trezza, …) are Wikimedia Commons placeholders — swap them
for original photos as soon as they're available.

## Add to Home Screen

The page includes the meta tags so guests can "Add to Home Screen" on iOS or
Android and open it like a native app.

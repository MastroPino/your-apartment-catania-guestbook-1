# Your Apartment Catania — Guest Book (kickoff brief)

Self-contained brief for a fresh Claude Code session. Read top to
bottom, do the "First moves" section before anything else.

---

## TL;DR

Replicate the **Malta** guidebook in this folder for a Catania (Sicily)
short-stay apartment. Same architecture, same screen inventory — only
the brand tokens, the content, the photos and the place data change.

- **This folder** — `/Users/andreacalabro/Sites/Personal/your-apartment-catania-guestbook`
- **Sibling reference** — `/Users/andreacalabro/Sites/Personal/your-apartment-malta-guestbook`
  (the Malta site; production at https://mastropino.github.io/your-apartment-malta-guestbook/)

---

## First moves (important — do these before any other edit)

This folder is **a clone of the Malta repo**. `git remote -v` still
points at `MastroPino/your-apartment-malta-guestbook`. If you commit
and push without re-pointing, you'll push Catania content over Malta.

Two paths — ask the user which:

1. **Fresh repo** (recommended) — create a new GitHub repo
   `your-apartment-catania-guestbook` under the same account, then:
   ```sh
   git remote set-url origin https://github.com/MastroPino/your-apartment-catania-guestbook.git
   git push -u origin main
   ```
   GitHub Pages: Settings → Pages → Deploy from a branch → `main` / `/ (root)`.
2. **Detach history** — same as above but start with
   `git checkout --orphan main && git commit -m "Initial commit (forked from Malta)"`
   if a clean history is desired.

Until origin is re-pointed, **don't push**.

---

## What stays vs. what changes

| Stays (don't reinvent) | Changes (rework) |
|---|---|
| `index.html` skeleton + 15-screen structure | Site copy (English), addresses, names, place data |
| Hash router, sticky topbar, scroll-spy, copy-to-clipboard, Wi-Fi QR rendering, universal "message the host" CTA — all in `assets/js/app.js` | Wi-Fi SSID + password in `app.js`'s `renderWifiQr()` |
| Component CSS (cards, accordion, hero, collage, etc.) | The **PUBLIC BRAND API** block at the top of `assets/css/style.css` (fonts, brand palette, radii) |
| Dark-mode wiring (`prefers-color-scheme`) | Maybe re-tune the dark palette if the new brand is colorful |
| Deployment recipe (GitHub Pages from `main`) | Repo URL + OG image |

The whole point of the recent refactor: re-skinning should be ~12
CSS variables + content swaps. If you find yourself rewriting
component rules, stop and consider promoting a new token instead.

---

## The brand-token override layer

Open `assets/css/style.css`. The first ~70 lines are the contract.
Edit only the **PUBLIC BRAND API** section to re-skin:

```css
/* Typography */
--font-display:  /* titles — pick a Catania-fitting serif */
--font-body:     /* body — keep clean and readable */
--font-accent:   /* tagline accent (e.g. "Enjoy your stay!") — optional */

/* Brand palette */
--brand-primary:    /* CTAs, banners, host badge, dark footer */
--brand-on-primary: /* readable foreground on --brand-primary */
--brand-footer-bg:  /* the always-dark strip at page bottom */

/* Surface palette */
--paper, --ink, --ink-soft, --muted, --cream

/* Radii */
--radius-sm: 12px /* accordion */
--radius-md: 14px /* CTA, avatar, collage */
--radius-lg: 16px /* standard cards */
--radius-xl: 22px /* premium cards */
--radius-pill: 999px /* pills, chips */
```

Then **also**:

- Load matching webfonts in `index.html`'s `<head>` (replace the
  Google Fonts URL — currently `Playfair Display + Poppins + Sacramento`).
- Update the `<meta>` brand strings (`<title>`, OG, Twitter Card,
  description, theme-color).
- Update the dark-mode `:root` overrides at `@media (prefers-color-scheme: dark)` so the new `--brand-primary` reads on dark surfaces too.
- Bump `style.css?v=N` to bust browser cache.

---

## Asset checklist

Drop optimized JPEGs in `assets/img/`. Target ≤ 200 KB each, max long
edge ~1200 px. Replace these slots (delete the Malta ones once filled):

**Apartment & home page**
- `apt-chalkboard.jpg`, `apt-living.jpg`, `apt-dusk.jpg`, `apt-tulips.jpg`,
  `apt-pasta.jpg` — 5 photos for the home collage + houseinfo/kitchen
- `host.jpg` — host portrait (square, ~400×400)
- `reviews.jpg` — optional background for the reviews screen
- `og-image.png` — 1200×1200 brand mark on dark, for link previews
- `logo-light.svg` + `logo-dark.svg` — brand logo (light bg + dark bg variants)

**Things to Do (current Malta has 17 — pick the Catania equivalents)**
Rename `ttd-*.jpg` to whatever Catania POIs you settle on. Suggested
Sicily/Catania shortlist:
- Etna (day hike or jeep tour)
- Catania historic center / Piazza del Duomo
- Fish market (La Pescheria)
- Taormina + Isola Bella
- Aci Trezza / Cyclops Stacks
- Cefalù day trip
- Syracuse + Ortigia
- Noto baroque
- Alcantara Gorges
- A coastal beach day (Caldura, San Marco, La Plaja)
- A signature dish / food experience (granita + brioche, arancini)

Let the host pick the "signature experience" — Malta has Dario's
sailing trip; Catania might have a guided Etna sunset, a private
cooking class, an Ortigia food tour, etc. That card gets the special
`tcard--host` treatment with the floating "Hosted by …" badge.

---

## Per-screen content the host has to provide

Use the Malta source files as the layout template (open both folders
side by side). Per screen:

| Screen | What's needed |
|---|---|
| **home** | Welcome line + apartment street address + 5 photos for the collage |
| **host** | Portrait, name, years hosting, 1-line philosophy, longer "his/her story" paragraph |
| **checkin** | Check-in time, check-out time, on-arrival paragraph, departure checklist, tips list |
| **wifi** | SSID + password (encoded in `app.js` `renderWifiQr` — search `WIFI:T:`) |
| **houseinfo** | Trash day & link (Malta uses era.org.mt — Catania ASEC equivalent), AC/heating notes, washing machine, lift, parking notes |
| **kitchen** | Coffee maker model + instructions, induction/gas, dishwasher, oven, what's stocked |
| **rules** | House rules list (smoking, pets, parties, noise, max guests) |
| **emergency** | Local emergency numbers (112 EU-wide, 113 polizia, 118 ambulanza, 115 vigili del fuoco — Italy), nearest hospital, pharmacy on-duty link, host phone, host WhatsApp |
| **transport** | Airport transfer notes, bus/metro lines, ride-hail (Uber? FreeNow?), parking |
| **thingstodo** | Cards per POI (see asset checklist) — title, blurb, "View on map" Google Maps short URL |
| **eat** | Recommended restaurants/bars/coffee shops with distance and map links |
| **nearest** | Quick essentials: coffee shop, grocery, pharmacy, restaurant, sea/swim spot — each with "Get directions" link |
| **beforeyougo** | Departure reminders, optional review CTA |
| **faq** | 8–12 questions (early check-in, late check-out, AC, hot water, mosquitoes, Wi-Fi outage, etc.) |
| **reviews** | 3–5 guest quotes from Airbnb/Booking |

---

## Owner intake questionnaire (ask the user/host upfront)

Drop these as one block to the host:

1. **Brand**: do you have a logo? (light + dark variants ideally — SVG preferred)
   Preferred colors / mood (warm/cool/minimal/bright)? Any font you love?
2. **Property**: full address, floor, lift y/n, parking notes, max guests.
3. **Identity**: host name to display, portrait photo, years hosting, a
   2-line "philosophy" + a longer paragraph for the host page.
4. **Logistics**: check-in / check-out times, key handover ritual,
   departure checklist, Wi-Fi SSID + password, trash day & rules.
5. **Amenities**: coffee maker model, AC/heating, washing machine y/n,
   what's stocked in the kitchen.
6. **Rules**: smoking, pets, parties, noise, anything specific.
7. **Recommendations**: top 12–15 things to do, top 6–8 places to eat,
   the 5 nearest essentials (coffee/grocery/pharmacy/restaurant/swim).
   For each: a 1-line blurb and a Google Maps share URL
   (`https://maps.app.goo.gl/…`).
8. **Signature experience**: does the host run their own activity?
   (e.g. cooking class, guided tour). If yes, that gets the
   "Hosted by …" featured card.
9. **Photos**: 5 apartment photos, host portrait, 1 photo per POI/place,
   any brand-y hero shots. Originals at full resolution — I'll resize.
10. **Emergency**: which hospital is nearest? Which pharmacy do they
    usually point guests to?

---

## Deployment

GitHub Pages, same recipe as Malta:

1. Create the Catania repo on GitHub.
2. Re-point `origin` (see "First moves").
3. Push `main`. Pages auto-builds from root.
4. Update OG `<meta>` URLs in `index.html` to the new Pages URL.

No build step. Push = deploy in ~30 s.

---

## Working agreements (carry over from Malta)

- **Italiano** in conversation; **English** in the site copy.
- **No emojis** in code or markup unless asked.
- **No `*.md`** files unless asked (this one and `CLAUDE.md` were).
- **Brand-token first** — when changing colors/fonts/radii, edit the
  PUBLIC BRAND API block, not the component rule.
- **Single `index.html`** — all 15 screens live as sibling `.screen`
  divs. The hash router activates one at a time.
- **CSS cache version** — bump `?v=N` on `<link rel="stylesheet">`
  after CSS edits.
- **Google Maps short URLs** as `href` for place cards — preserves
  the rich preview on tap.
- **No framework** — vanilla JS, single bundle-free site. If the user
  asks "should we use React?", the answer is "no, not for this scope".

---

## Reference

- Malta `CLAUDE.md` (project memory): `/Users/andreacalabro/Sites/Personal/your-apartment-malta-guestbook/CLAUDE.md`
- Malta live site: https://mastropino.github.io/your-apartment-malta-guestbook/
- Malta repo: https://github.com/MastroPino/your-apartment-malta-guestbook

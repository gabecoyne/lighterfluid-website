# Lighter Fluid Website

Shopify theme for [lighterfluid.co](https://lighterfluid.co).

**Repo:** https://github.com/gabecoyne/lighterfluid-website
**Branch:** `master` (auto-deploys to the live theme via Shopify's GitHub integration)
**Stack:** Plain Liquid + CSS. No build step. No Tailwind. No section editing in admin.

---

## Architecture

This is intentionally a **minimal Online Store 2.0 theme**. Content lives in Liquid files; nothing is editable from the Shopify admin's section editor. To change a page, edit the corresponding `sections/main-{page}.liquid` file and push to `master`.

### Files

```
.
├── assets/                 ← CSS, fonts, images
│   ├── tokens.css          ← design tokens (mirror of Assets/Brand/tokens.css in the LF project)
│   └── site.css            ← page layout, components, dark sections, mobile nav
├── config/
│   ├── settings_data.json
│   └── settings_schema.json
├── layout/
│   └── theme.liquid        ← head, nav (via section), main yield, footer (via section)
├── locales/
│   └── en.default.json
├── sections/
│   ├── header.liquid       ← shared site nav
│   ├── footer.liquid       ← shared site footer
│   ├── main-home.liquid    ← homepage body
│   ├── main-implementation.liquid
│   ├── main-coaching.liquid
│   ├── main-work.liquid
│   ├── main-about.liquid
│   └── main-contact.liquid
├── snippets/
│   └── seo-meta.liquid     ← per-page title, OG, Twitter, JSON-LD (context-aware)
└── templates/
    ├── index.json          ← homepage → main-home
    ├── page.implementation.json
    ├── page.coaching.json
    ├── page.work.json
    ├── page.about.json
    ├── page.contact.json
    └── 404.liquid
```

### How a page request resolves

1. Shopify routes `/pages/implementation` to `templates/page.implementation.json`.
2. That JSON references the section `main-implementation`.
3. `layout/theme.liquid` wraps it with the head + header + footer.
4. `snippets/seo-meta.liquid` renders the right meta tags based on `page.handle`.

---

## Deploying

### Initial setup (once, by Gabe)

1. **Connect this repo to Shopify.** In Shopify admin: Online Store → Themes → Add theme → Connect from GitHub. Authorize, pick `gabecoyne/lighterfluid-website`, branch `master`.
2. **Create the Pages.** In Shopify admin: Online Store → Pages → Add page. Create five pages with handles matching the templates:
   - `implementation` (uses `page.implementation` template)
   - `coaching` (uses `page.coaching` template)
   - `work` (uses `page.work` template)
   - `about` (uses `page.about` template)
   - `contact` (uses `page.contact` template)
3. **Set the homepage.** Online Store → Preferences → Homepage / shop default uses `index.json` automatically.
4. **Publish the theme.** Once preview looks right, click Publish.

### Ongoing edits (Claude or human)

- Edit `sections/main-{page}.liquid` for content changes on a specific page.
- Edit `assets/site.css` or `assets/tokens.css` for design changes.
- Edit `snippets/seo-meta.liquid` for SEO / JSON-LD changes.
- Commit. Push to `master`. Shopify auto-deploys (usually within ~30 seconds).

### From the LF project

This repo lives at `Dev/lighterfluid-website/` inside the Lighter Fluid project folder. The source HTML mockups in `Projects/website-v1/` are kept as design reference. To re-port any updated HTML pages into the theme:

```bash
cd "/Users/gabecoyne/Documents/Claude/Projects/Lighter Fluid"
python3 Scripts/build_shopify_theme.py
cd Dev/lighterfluid-website
git add -A && git commit -m "Sync from website-v1" && git push
```

---

## What's intentionally not here

- **Cart, products, collections.** Add when there's something to sell.
- **Section settings / blocks.** All content is in Liquid files; nothing is editable in the section editor.
- **Build step.** No Tailwind compilation, no JS framework. If we add ecomm and need utility classes, add Tailwind then.
- **App embed entry points.** Will add when we install our first Shopify app.

---

## Setup checklist for Gabe

- [ ] Connect this repo to Shopify (Online Store → Themes → Add theme → Connect from GitHub, branch `master`)
- [ ] Create five Shopify Pages with handles: `implementation`, `coaching`, `work`, `about`, `contact`
- [ ] Verify the theme preview renders all six pages correctly (home + five sub-pages)
- [ ] Publish the theme
- [ ] Wire the contact form (currently a mockup that alerts — needs Formspree, Klaviyo, or similar)
- [ ] Add real founder portrait photography to `assets/` and update `sections/main-about.liquid`
- [ ] Add real logo (replace Remix Icon `ri-fire-line` placeholder)
- [ ] Add `og-image.jpg` to assets for social-share previews
- [ ] Configure custom domain (lighterfluid.co) in Shopify admin

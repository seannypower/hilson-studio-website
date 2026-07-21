# The Hilson Studio — Website

A static rebuild of [thehilsonstudio.com](https://www.thehilsonstudio.com), replacing the Squarespace-hosted original. Built with [Astro](https://astro.build) — every page compiles to plain HTML/CSS with a small amount of vanilla JS (nav toggle, pricing tabs, homepage slideshow). No React, no client-side framework, no server/database.

## Project structure

```
src/
  layouts/Layout.astro      shared header, announcement bar, footer, <head>
  components/Nav.astro      main dropdown nav (desktop hover / mobile tap)
  components/Gallery.astro  simple image grid with optional captions
  components/SoundCloudEmbed.astro   wraps the SoundCloud iframe embed
  pages/*.astro             one file per page, filename = URL path
  styles/global.css         all site styles (colors, type, layout, forms)
public/images/              every image, downloaded and re-hosted locally
```

Astro's file-based routing means the filename **is** the URL: `src/pages/the-gear.astro` → `/the-gear`, `src/pages/artistsbands-1.astro` → `/artistsbands-1`, etc. These slugs were kept identical to the original Squarespace URLs.

### Pages included

**In the nav** (matches the live site's menu exactly):
`/` (splash), `/about`, `/sounds`, `/space`, `/staff`, `/the-gear`, `/cost`, `/artistsbands`, `/songwriters`, `/artistsbands-1`, `/producer2`, `/liveatthehilson`, `/video-production`, `/contact`, plus `/drummers` (linked from the Musician page, not the top nav — same as the original).

**Not in the nav, built for link continuity** (these existed on the live site at these URLs but weren't linked from its menu either — see stub-note banners on each): `/about-insta`, `/holidays`, `/xmasvideos`, `/pics`, `/liveatthehilsonstudio`, `/video-booking`. Review these before pointing anything at them — some contain stale pricing/coupon info from past promotions.

## Development

```sh
npm install
npm run dev       # http://localhost:4321
npm run build     # outputs static site to ./dist
npm run preview   # serve the production build locally
```

## Contact forms — Netlify Forms

The Contact page (`/contact`) and the legacy Holidays page (`/holidays`) use [Netlify Forms](https://docs.netlify.com/forms/setup/) — no backend code, no third-party form service, no API keys. Netlify detects the `data-netlify="true"` forms automatically at deploy time, so submissions just work once deployed to Netlify. Each form has a hidden honeypot field (`bot-field`) for spam protection, mirroring the honeypot present on the original Squarespace form. Successful submissions redirect to `/thanks`.

**This only works when hosted on Netlify.** If you ever move off Netlify, you'll need to swap in something like [Formspree](https://formspree.io) instead (change the `action` and remove the `data-netlify`/`netlify-honeypot` attributes).

Submitted messages show up in the Netlify dashboard under **Forms**, and can be configured there to forward to an email address.

## Deploying

1. Push this repo to GitHub (see below).
2. Go to [app.netlify.com](https://app.netlify.com) → **Add new site → Import an existing project** → pick this repo.
3. Build settings are already set via `netlify.toml` (build command `npm run build`, publish directory `dist`) — Netlify should detect them automatically.
4. Deploy. Forms are detected automatically on the first deploy — no extra setup.
5. Point your domain (`thehilsonstudio.com`) at Netlify: Site settings → Domain management → Add a domain, then update your DNS (Netlify will show you the exact records).

Netlify's free tier covers this comfortably: 100GB bandwidth/month, 100 form submissions/month, free HTTPS, custom domain support.

## Making content edits later (with Claude Code)

Point Claude Code at this repo and describe the change in plain language. Some guidance on where things live:

- **Text changes on a specific page** → edit the matching file in `src/pages/`. Page titles/URLs map directly: "change the Cost page" → `src/pages/cost.astro`, "update the About page" → `src/pages/about.astro`.
- **Swapping or adding a photo** → drop the new image into `public/images/`, then update the `src="/images/..."` reference in the relevant page.
- **Nav menu changes** (adding/removing/renaming a menu item) → `src/components/Nav.astro`.
- **Site-wide look** (colors, fonts, spacing, header/footer) → `src/layouts/Layout.astro` for structure, `src/styles/global.css` for styling.
- **Pricing table** → `src/pages/cost.astro`, one `<div class="price-card">` per line item, grouped under `<div class="tab-panel" data-panel="...">`.
- **Contact form fields** → `src/pages/contact.astro` (or `holidays.astro` for the legacy form).

After any edit, run `npm run dev` and check the page in a browser before pushing — then `git push` to deploy (Netlify auto-deploys on push to the connected branch).

## Known simplifications / things to review

- **Fonts**: the original site uses Futura PT (a paid Adobe/Typekit font). This rebuild substitutes the free Google Font **Jost**, which is visually close but not identical. If you have a Typekit/Adobe Fonts license, swap the `@import` in `src/styles/global.css` for the real Futura PT embed.
- **`/pics`**: the original was a large tabbed portfolio aggregator that mostly re-listed photos/videos already on the Space, Live-at-Hilson, and Video Production pages. It's rebuilt here as a simplified tabbed reference rather than a full duplicate — see the note on the page itself.
- **Live-at-Hilson performance list**: the original site grouped ~54 past performances under "Full Band" and "Acoustic" headers, but didn't expose which song belonged to which category in a way that could be scraped reliably. This rebuild lists them all together under one heading.
- **Image weight**: images were re-hosted as-is from the original CDN (~90MB total across the site). Consider running them through an optimizer/compressor for faster load times — this wasn't done here to keep the rebuild scope manageable.
- **`/holidays` and `/video-booking`**: both carry stale pricing/coupon-code content from a past seasonal promotion. They're included for URL continuity (per your call to keep orphaned pages) but should be updated or removed before linking them anywhere live.

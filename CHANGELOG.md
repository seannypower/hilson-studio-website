# Changelog

Working log for the Hilson Studio static rebuild. Ordered newest-first. Use the
"Outstanding" section at the top as the jumping-off point next time you (or
Claude Code) pick this back up.

## Outstanding — needs a decision or more work

Nothing here is broken in a way that blocks going live; these are known gaps.

- **Video Production page (`/video-production`) still shows static images.**
  All 11 images on this page are actually individually embedded YouTube
  videos on the live Squarespace site (confirmed via raw-HTML inspection,
  same as the Live-at-Hilson fix). Video IDs have **not** been extracted yet.
  This is the same class of fix already applied to Live-at-Hilson, Drummers,
  and Producer2 — highest-value remaining item.
- **About page's gallery is missing 7 embedded Instagram Reels.** The
  original mixes 7 live Instagram Reel embeds with 5 static photos in one
  lightbox gallery; the rebuild currently shows all 12 as static photos.
  Recovering the specific reels is possible (URLs were found during
  investigation) but won't auto-update. For a genuinely live-updating feed,
  need a third-party widget (SnapWidget / LightWidget / Elfsight / Behold.so)
  — requires you to create an account and connect the studio's Instagram;
  once you have an embed snippet, dropping it in is quick. Alternative:
  skip the embed, just link out to Instagram with a "Follow us" button.
  **Needs your call before implementing either direction.**
- **3D Interactive Layout on `/space` is a dead link, not a rebuild bug.**
  The original SketchUp 3D Warehouse model has been deleted at the source —
  confirmed the same URL 404s on the live Squarespace site too. Currently
  replaced with a note. Needs either a replacement 3D tour/model or a
  decision to remove the section.
- **`/pics` is a simplified stand-in, not a full rebuild.** The original
  links out to 7 sub-galleries (some 50+ items) that were never in the
  site's nav or sitemap. Given `/pics` itself is unlinked from nav, each
  card currently points at whichever already-built page covers similar
  content instead. Flagged on the page itself. Revisit if those sub-galleries
  turn out to matter.
- **`/holidays` and `/video-booking` carry stale content.** Old seasonal
  pricing and an expired coupon code, kept only for URL continuity since
  they're unlinked from nav. Update or remove before ever linking to them.
- **Netlify isn't connected to GitHub yet.** The live preview
  (hilson-studio-website.netlify.app) was deployed manually via CLI once;
  it will **not** auto-update on future pushes until you link the GitHub
  repo in Netlify's dashboard (Site settings → Build & deploy → Link
  repository). Do this before relying on git push to deploy.
- **Images are unoptimized.** All ~200 images were re-hosted as-is from the
  original CDN (no compression/resizing pass). Worth doing before a real
  launch if page-load speed matters.
- **Futura PT (paid font) is substituted with Jost (free).** Close visual
  match but not identical. Swap the `@import` in `src/styles/global.css` if
  you get a Typekit/Adobe Fonts license for the original.
- **Typography/layout audit hasn't touched every page.** Deep-audited so far:
  About, Cost, Space, Live-at-Hilson, Artist/Band, Songwriter, Musician,
  Producer2. **Not yet audited** against the live site for the same class of
  issues (bold-vs-light headings, two-column layouts, missing embeds):
  Gear, Staff, Sounds, Contact, Drummers (beyond the one video fix already
  made), and all the orphaned/unlinked pages (`about-insta`, `xmasvideos`,
  `liveatthehilsonstudio` splash).

## 2026-07-21 — Layout fidelity pass, carousels, content recovery

- Built `Carousel` component (prev/next + thumbnail strip, mixed image/video
  slides) reusing the existing `Lightbox` for expand-to-fullscreen. Rolled
  out to About, Space (Studio Build Photos), Artist/Band, Songwriter,
  Musician.
- Fixed a real Astro bug: `Lightbox`'s content div is populated via
  `innerHTML` in JS at runtime, so those elements never receive Astro's
  scoped-style attribute — sizing rules were silently not applying. Made
  those styles global. (Video lightbox now properly fills ~92% of viewport
  width instead of sitting at the browser's ~300px default iframe size.)
- Fixed a CSS grid "blowout" bug: any `.two-col` item with fixed-width
  children (carousel nav arrows, thumbnail strip) refused to shrink to its
  assigned column, distorting every carousel layout. Added
  `.two-col > * { min-width: 0; }` as a standing rule.
- Corrected the site's heading system: original uses a **light** weight by
  default and only bolds specific headings as a manual editorial choice
  (confirmed page-by-page, not a single rule) — Cost's "PRICING", Sounds'
  "AUDIO SAMPLES", all of Space's section labels, Producer's "HEAR IT FOR
  YOURSELF". Previously the rebuild used one bold style everywhere.
- Rebuilt several sections that had been flattened from two-column to
  single-column stacks: About's hero, Live-at-Hilson's hero and its
  "How does it work" / "Keeping it real" / "Make an impression" sections,
  Producer2's hero, Cost's Additional Services.
- Restored all 51 Studio Build photos on `/space` (rebuild had only 6).
- Cohesive testimonial styling (italic quote, non-italic name, italic
  title/location) applied consistently on Artist/Band, Songwriter, Producer2.
- Fixed About page's Warm Audio / MCCK logos to link out to
  warmaudio.com / musiccitycontentkings.com.
- Footer copyright year → 2026.
- Recovered the real embedded YouTube videos behind Live-at-Hilson's 54
  "past performances" (previously flattened to a static thumbnail grid + a
  flat text list) and Drummers' one misplaced video (previously shown as a
  static joke photo).

## 2026-07-21 — Initial static rebuild

- Full site inventory: 14 nav-linked pages, `/drummers` (linked in-page),
  and 6 unlinked-but-live pages kept for URL continuity (`about-insta`,
  `holidays`, `xmasvideos`, `pics`, `liveatthehilsonstudio` splash,
  `video-booking`).
- Astro chosen over plain HTML for the shared header/nav/footer template
  and the repeated Studio/Who-are-you? page pattern.
- Netlify Forms wired up for Contact and Holidays forms (matches the
  original's honeypot spam protection).
- All images re-hosted locally (`public/images/`) instead of hotlinking
  Squarespace's CDN.
- SoundCloud, YouTube, and SketchUp embeds preserved.
- "Book a shoot" link corrected from a Google Form `/edit` URL (accidentally
  exposing edit access) to the public `/viewform` URL.
- GitHub repo created and pushed; Netlify site deployed manually once.

# Changelog

## 2026-07-18

- Rebrand: site name changed from "CBoi" to "Nabi" to keep this design
  identity separate from the user's main/gaming account. Updated
  `index.html` (title, meta/OG/Twitter tags, favicon initial, logo, hero
  copy, footer), `CLAUDE.md`, `docs/PROJECT_OVERVIEW.md`, and
  `docs/WEBSITE_STRUCTURE.md`.
- GitHub account rename (`cboi-ui` -> `nabi-ui`) queued but not yet done —
  GitHub only allows this through account settings, not the API. See
  `docs/TODO.md` for the follow-up steps once the user completes it
  (update git remote, verify Pages URL, update repo/Pages links in docs).
- Known gap: all portfolio screenshots in `TemplateImages/` have a "CBoi"
  watermark baked into the image pixels (tiled overlay + bottom logo).
  Text edits can't fix this — needs re-export from Roblox Studio with a
  "Nabi" watermark. Tracked in `docs/TODO.md`.
- Added `TemplateImages/MinimalistStyle/Template2.png` ("Fruits" collection
  dex panel) to `docs/PORTFOLIO_CONTENT.md` as a 4th portfolio piece; not
  yet wired into `index.html`.
- Updated the Contact section Discord tag from `choiboi12` to `nabi_ui`
  (new account made specifically for this identity) in `index.html`
  (display span + clipboard copy handler).
- Received re-watermarked ("Nabi_ui") exports for Star Shop & Popup Suite
  (all 3 images), Daily Rewards Claim, and two new Stud-Style pieces (Skull
  Shop, Inventory). One image (popups) initially still had "@CBoi" baked
  into the "Gift a Friend" card copy itself, separate from the corner
  watermark — user sent a corrected version with "@Nabi_ui" instead.
- Updated `TemplateImages/CartoonStyle/` (template1, Template2, Template3)
  and `TemplateImages/StudStyle/Template1` with the re-watermarked
  versions; added `TemplateImages/StudStyle/Template2` (Skull Shop) and
  `Template3` (Inventory) as new source images.
- Installed Pillow to resize/compress the new exports into
  `assets/images/*.jpg` (matching the site's existing flat, compressed-JPEG
  convention) and removed the raw staging folders
  (`assets/images/Cartoony/`, `assets/images/StudStyle/`) once done.
- Added "Skull Shop & Inventory" as portfolio piece #4 in
  `docs/PORTFOLIO_CONTENT.md` and wired it into `index.html`'s portfolio
  gallery as a 4th card (2-image lightbox, matching the Star Shop card
  pattern). Fruits Collection Dex renumbered to #5, still not wired in
  (Minimalist images not yet re-watermarked).
- Found and fixed a regression: `assets/images/inventory.jpg` (Frost Hunter,
  Minimalist) had been deleted from the working tree with no replacement
  provided, which would have broken that portfolio card. Restored the last
  committed (still CBoi-watermarked) version from git history as a
  stopgap — later removed for good, see below.
- Removed Frost Hunter Inventory and Fruits Collection Dex (both Clean
  Minimalist UI) from the portfolio at the user's request, rather than
  re-watermarking them. Deleted the Frost Hunter card from `index.html`
  and `assets/images/inventory.jpg`; removed both entries from
  `docs/PORTFOLIO_CONTENT.md` (renumbered remaining pieces 1-3) and
  updated the project count in `docs/WEBSITE_STRUCTURE.md`. Source images
  left untouched in `TemplateImages/MinimalistStyle/` in case they're
  wanted back later. Portfolio is now 3 pieces: Star Shop & Popup Suite,
  Daily Rewards Claim, Skull Shop & Inventory.

## 2026-07-14

- Built and shipped v1 of `index.html`: hero, portfolio (3 projects),
  services, pricing, commission process, contact.
- Optimized and embedded portfolio screenshots as `assets/images/*.jpg`
  (resized/compressed from `TemplateImages/`).
- Embedded Space Grotesk (woff2, base64) for headings instead of relying on
  a system-font fallback or an external font request.
- Added interactive background: subtle cursor parallax on the ambient glow,
  plus a click ripple/spark burst (toned down after first feedback pass,
  see `CLAUDE.md` for the "keep it subtle" note).
- Added a hover shine sweep on portfolio cards and the primary CTA button,
  plus a static top-edge gloss highlight on all panels.
- Copy pass: rewrote hero/section copy for a more human, first-person voice;
  removed em dashes; leaned into real Roblox dev vocabulary (GUI, Robux,
  cartoon-style, simulator, stud-style, Studio).
- Added Open Graph/Twitter meta tags, a favicon, and visible keyboard-focus
  states.
- Site approved by user; next step is creating the GitHub repo and enabling
  Pages.
- Discovered `TemplateImages/` had been reorganized (files renamed to
  `Template1/2/3.png`, Settings moved from Minimalist to Cartoon folder, and
  a new "Fruits" collection/dex screenshot added to Minimalist). Restructured
  the portfolio: Star Shop & Popup Suite now includes Settings as a 3rd
  image, Frost Hunter Inventory is now solo. Briefly added a 4th "Fruits
  Collection Dex" card, then user asked to keep it to 3 projects for this
  launch, removed it (image still sits unused at
  `TemplateImages/MinimalistStyle/Template2.png` for a future addition).
  See `docs/PORTFOLIO_CONTENT.md`.
- Initialized git, created the public `cboi-ui/portfolio` GitHub repo, pushed
  the initial commit, and enabled GitHub Pages (legacy branch-based
  deployment from `main`, not the Actions workflow build type, since this is
  a plain static site with no build step). Live at
  https://cboi-ui.github.io/portfolio/
- Lowered public starting prices: Simple UI $10+ -> $5+, Standard UI $30+ ->
  $25+ (Complete UI Packages unchanged at $100+). Updated both the site
  pricing cards (`index.html`) and `docs/PRICING_GUIDE.md` (public table and
  the internal Basic/Standard UI ranges, so the internal floor doesn't sit
  above what's advertised).

## 2026-07-13

- Set up project docs (`docs/`) and `CLAUDE.md`.
- Catalogued portfolio images in `TemplateImages/` into 3 grouped projects
  (Star Shop & Popups, Frost Hunter Inventory & Settings, Daily Rewards).
- Defined public pricing (3-tier) vs. internal quoting reference.

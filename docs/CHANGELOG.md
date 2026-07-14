# Changelog

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

## 2026-07-13

- Set up project docs (`docs/`) and `CLAUDE.md`.
- Catalogued portfolio images in `TemplateImages/` into 3 grouped projects
  (Star Shop & Popups, Frost Hunter Inventory & Settings, Daily Rewards).
- Defined public pricing (3-tier) vs. internal quoting reference.

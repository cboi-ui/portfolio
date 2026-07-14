# CBoi UI Commission Portfolio

This repo is a single-page portfolio/business site for CBoi's Roblox UI commission
work, plus the docs that keep the site's content consistent as it evolves.

## Structure

- `index.html` (+ any co-located CSS/JS) — the actual live site.
- `TemplateImages/` — source portfolio screenshots, organized by style
  (CartoonStyle, MinimalistStyle, StudStyle). Treat this as the source of truth
  for portfolio images; the site references copies/optimized versions of these.
- `docs/` — content and policy source-of-truth. The site's copy should always
  agree with these files.

## Rules for future sessions

- If pricing changes, update `docs/PRICING_GUIDE.md` **and** the pricing
  section of the site in the same change. Never let them drift.
- Keep the public-facing price list to the simple 3-tier version
  (Simple / Standard / Complete). The detailed complexity-based breakdown in
  `PRICING_GUIDE.md` is for CBoi's own quoting reference — do not publish the
  granular numbers on the site itself (avoids "6 frames = $60" scope-creep
  arguments from clients).
- When adding a new portfolio piece, add images to `TemplateImages/<Style>/`
  and add a matching entry to `docs/PORTFOLIO_CONTENT.md` before wiring it
  into the site.
- Log any meaningful site change (new section, redesign, pricing update,
  new portfolio piece) as a dated entry in `docs/CHANGELOG.md`.
- Check `docs/TODO.md` at the start of a session for open follow-ups (e.g.
  pending links, content still needed).
- This is a marketing/business site, not a Roblox game — `docs/DESIGN_SYSTEM.md`
  describes the *website's* visual system, not Roblox UI design principles
  (those live in `docs/SERVICES.md` / `docs/PORTFOLIO_CONTENT.md` instead).
- **Never fabricate credibility.** Do not invent portfolio projects, client
  names, testimonials, project counts, or experience numbers. Only the
  confirmed "2+ years UI experience" figure exists right now — no project
  count, no client count, no testimonials. Use only numbers the user has
  explicitly provided; omit a stat/element entirely (don't guess a
  plausible-sounding placeholder) rather than invent one, and flag the gap
  to the user instead.
- **Keep copy human, not AI-generic.** No em dashes in body copy, first-person
  voice, and lean on real Roblox dev vocabulary (GUI, Robux, Studio,
  simulator, cartoon-style, stud-style) instead of generic design-speak.
  User explicitly asked for this after the first draft read as "cringy."
- **Interactive/decorative effects (background parallax, click ripple, shine
  sweeps) default to subtle.** The first pass was too intense and had to be
  toned down; see `docs/DESIGN_SYSTEM.md` for current values. If asked to add
  motion, undershoot rather than overshoot.

## Local preview workflow (important)

The Claude Artifact hosting was unreliable for this project's preview and
repeatedly returned "Page not found" even when the published content was
verified correct. Root cause found once: the Artifact tool auto-wraps
published files in its own `<!doctype>/<head>/<body>` skeleton, so a file
that already contains its own full document tags produces a broken nested
structure. Fixing that (stripping doctype/html/head/body from the
artifact-only copy) didn't fully resolve it for the user, so **the reliable
path is a local server**, not the Artifact link:

```
py -m http.server 8080
```
run from the project root, then open `http://localhost:8080/`. It serves
`index.html` directly with its real relative asset paths
(`assets/images/...`), so what you see locally matches what will ship to
GitHub Pages exactly. No build step, no restart needed after edits, just
refresh the browser.

If you still want an Artifact preview, generate a separate copy with the
`<!doctype html>/<html>/<head>/<body>` wrapper tags stripped and portfolio
images inlined as base64 `data:` URIs (Artifacts must be fully
self-contained, no external file references) — but treat it as a bonus, not
the primary way to check work.

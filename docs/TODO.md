# TODO

- [x] Get Discord handle to display in Contact section. (`nabi_ui`, updated
      2026-07-18 from `choiboi12` as part of the rebrand.)
- [ ] Decide whether to link HiddenDevs profile, and get the URL if so. Low
      priority, site ships without it for now.
- [x] Decide whether email should be public or Discord-only. (Discord only)
- [x] Decide on stats row: no project count yet (0 commissions completed so
      far), show only "2+ years UI experience," nothing fabricated.
- [x] Build first version of the site and preview it (Claude Artifact hosting
      was unreliable in this session, ended up previewing via a local
      `py -m http.server` instead, see `CLAUDE.md` for the gotcha).
- [x] Get feedback, iterate (copy pass for tone/Roblox vocab, interactive
      background, shine effects, hero headline rewrite). Approved 2026-07-14.
- [x] Create GitHub repo under the `cboi-ui` account and enable GitHub
      Pages. Live at https://cboi-ui.github.io/portfolio/
      (repo: https://github.com/cboi-ui/portfolio). Superseded 2026-07-18
      by the account rename below — now at
      https://nabi-ui.github.io/portfolio/
      (repo: https://github.com/nabi-ui/portfolio).
- [ ] Optional later: custom domain.
- [ ] Optional later: add testimonials/real project counts once first
      commissions come in, update the hero stat pill and `DESIGN_SYSTEM.md`
      trust layer section accordingly.

## Rebrand: CBoi -> Nabi (started 2026-07-18)

- [x] Rebrand site copy, meta tags, logo, and docs from "CBoi" to "Nabi".
- [x] User renamed the GitHub account `cboi-ui` -> `nabi-ui` (2026-07-18).
      Old repo/profile links will 404 or stop redirecting if someone else
      claims `cboi-ui` in the future.
- [x] Local git remote updated to `https://github.com/nabi-ui/portfolio.git`
      and verified (`git ls-remote`). GitHub Pages confirmed live at
      `https://nabi-ui.github.io/portfolio/` (`WebFetch`-checked).
- [x] Committed and pushed the rebrand to `origin/main`; confirmed live via
      background poll of `nabi-ui.github.io/portfolio/` (title, Discord tag,
      and portfolio all showing the new branding).
- [x] Re-export/re-watermark Cartoon Simulator UI (Star Shop, popups,
      settings) and Stud-Style Daily Rewards with "Nabi_ui". Received
      2026-07-18, wired into `TemplateImages/` and `assets/images/`.
- [x] Discord handle updated to `nabi_ui` (2026-07-18), replacing
      `choiboi12` in Contact section.
- [x] New Stud-Style pieces (Skull Shop, Inventory) added as portfolio #3
      and wired into `index.html`.
- [x] Frost Hunter Inventory and Fruits Collection Dex (Clean Minimalist UI)
      removed from the portfolio at user's request (2026-07-18) instead of
      being re-watermarked. Removed from `index.html` and
      `docs/PORTFOLIO_CONTENT.md`; source images left untouched in
      `TemplateImages/MinimalistStyle/` in case they're wanted back later.
- [x] Split the 3 grouped multi-image carousel cards into 6 individual
      cards (2026-07-18, per user request to show more work up front
      instead of hiding images behind carousel dots): Star Shop, Purchase
      & Rebirth Popups, Settings Menu, Daily Rewards Claim, Skull Shop,
      Collection Inventory.

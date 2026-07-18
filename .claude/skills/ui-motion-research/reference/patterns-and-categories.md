# Patterns from real games, and animation categories

## Honesty about source depth

Frame-by-frame animation breakdowns of specific commercial games' shop/
popup UIs (exact Adopt Me or Blox Fruits tween durations, say) are not
publicly documented anywhere findable — these are proprietary
implementation details, not published design docs. What *is* well
documented is the **general pattern language** these games share, drawn
from UX case studies, UI-focused design galleries (Game UI Database), and
Roblox DevForum discussions of the same problems. Treat the patterns below
as "what this category of game consistently does," not "verified exact
values from game X."

## Recurring patterns across Roblox shop/popup UIs

From DevForum discussion threads (shop GUIs, popup animations, "how do I
make this kind of UI popup" requests — a very common recurring question
category) and shipped free UI resources (e.g. community "StoreUI" kits):

- **Shops/inventories open via scale-in from center, not slide-in from an
  edge**, in the large majority of examples — matches the `AnchorPoint`
  (0.5, 0.5) convention this project already standardized on via
  `ui-prep`.
- **Popups commonly combine two motions**: a scale tween (0.8 → 1.0 or
  similar) *and* a fade/transparency tween playing together, rather than
  either alone — this reinforces the "state changing" read (Disney's
  appeal/staging) more than a single-property tween.
- **Close (dismiss) is faster than open** — consistent with the general
  timing guidance that exits get less budget/attention than entrances.
- **A dedicated hover-only invisible button layered under the visible
  click buttons** (exactly the `HoverButton` pattern already present in
  this project's `SkullShopGui`) is a common structural solution for
  giving an entire card a hover reaction without it fighting the specific
  Buy/Sell click targets.

## Reward/loot reveal pattern (chest/gacha openings)

Consistently described pattern across mobile game UX write-ups:

1. **The "drumroll."** A deliberate pause or building animation (shake,
   glow intensifying, particles building) *between* the player's tap and
   the reveal — this is the anticipation window where the psychological
   payoff actually happens (see `motion-principles.md`). Skipping straight
   to the result loses most of the emotional value of the reveal.
   Roughly ~0.5–1.5s of build-up is typical before the reveal itself.
2. **The reveal.** Often uses a stronger easing (Back/Elastic/Bounce) than
   any routine UI element would ever use — this is the one place
   overshoot easing is not just acceptable but expected.
3. **Rarity-differentiated treatment.** Higher-rarity rewards get *more*
   —longer build-up, bigger visual/audio payoff, sometimes a distinct
   color-coded flash — not a different animation from scratch, but a
   scaled-up version of the same one. This lets a single reward-reveal
   system handle all rarities via a couple of tunable parameters (build-up
   duration, particle density, overshoot amount) rather than bespoke
   animations per item.
4. **Currency/item counting up, not jumping to the new total** — see the
   count-up category below; a total that pops from "1200" to "1450"
   instantly reads as less satisfying than one that visibly counts.

## Industry UI takeaways (non-Roblox)

From UX case-study material on Valorant/Overwatch/competitive-shooter menu
design:
- **Motion is used to reduce cognitive load in dense HUDs** — distinct
  shapes *and* distinct animation per HUD element type helps users parse a
  busy screen at a glance, rather than relying on layout alone.
- **A common critique pattern in these case studies**: menus that dedicate
  large animated space to monetization (event banners, cosmetic promos)
  at the expense of quick access to core options. Worth noting as an
  anti-pattern to avoid, not a pattern to copy, if this project ever adds
  a main-menu-style hub.
- **Winning/result-card reveals lean on strong motion + a distinct "this
  is a big moment" treatment**, differentiated clearly from routine menu
  transitions — reinforces the "save the strong easing for rare moments"
  rule from `motion-principles.md`.

## Animation category catalog

Quick reference for "what's the standard approach to X":

- **Pop / Scale-in.** Element scales from ~0 or ~0.8 to 1.0, usually paired
  with a fade. Standard for popups, cards, new item appearances. Anchor at
  center first (`ui-prep`).
- **Slide.** Element moves in from off-screen along one axis. Good for
  notifications, side panels; less common for primary modal popups (scale
  reads as more "this is a focused moment" than slide does).
- **Fade.** Transparency-only tween, often layered under a Scale/Slide
  rather than used alone — fade alone can feel weak for anything the user
  should register as A Thing Happening, but is exactly right for subtle
  states (tooltips, disabled overlays).
- **Bounce / Overshoot.** See reward pattern above — reserve for rare,
  celebratory moments.
- **Rotation.** Used sparingly in UI (spinning loading icons, a coin
  flip); needs a correctly centered `AnchorPoint` or it visibly orbits.
- **Parallax.** Background layers moving at different rates than
  foreground on cursor/scroll — this project's CLAUDE.md already notes a
  past incident of parallax being overdone and needing to be toned down;
  treat parallax intensity as a "default to subtle" case generally, not
  just for that one project.
- **Progress bars / Count-up numbers.** Numeric or bar fill should
  visibly animate toward the new value rather than jump — the motion
  itself is part of what reads as "reward," independent of the number's
  size. A count-up of ~0.5–1s for typical currency deltas is a reasonable
  default; longer for dramatic reveals.
- **Card stack / Carousel.** Sequential reveal or swipe-through; keep
  per-card transition time short (200–300ms) since these are usually
  browsed quickly and repeatedly.
- **Tooltip.** Fast fade/scale-in on hover (150–200ms), no overshoot —
  tooltips are informational, not celebratory.
- **Modal / Dropdown / Accordion.** Standard open/close timing band
  (200–400ms) from `timing-easing-feedback.md`; dropdown/accordion should
  also animate the *content it displaces* (siblings shifting down) rather
  than having content just snap into its new position.
- **Shimmer / Loading skeleton.** A gradient sweep (via `UIGradient`
  Offset animation) across placeholder shapes matching the eventual
  layout. Verified finding: users perceive skeleton-loaded content as
  loading meaningfully faster than a spinner even at identical actual load
  time — this is a real perceived-performance technique, not just
  decoration. Only worth using for loads longer than ~0.5s; shorter than
  that, skip it entirely (see `timing-easing-feedback.md`'s loading-state
  note).
- **Pulse / Glow.** Slow, looping, low-amplitude scale or transparency
  oscillation — used to draw attention to an actionable but easy-to-miss
  element (a claimable reward badge). Keep amplitude small; a pulsing
  element competes for attention for as long as it's on screen, so it
  should be reserved for things that genuinely need a persistent nudge.

## Sources
- [Roblox DevForum — Pop-up Shop GUI](https://devforum.roblox.com/t/pop-up-shop-gui/478393)
- [Roblox DevForum — How to make popup GUI slide in and out](https://devforum.roblox.com/t/how-to-make-popup-gui-slide-in-and-out/2767038)
- [Roblox DevForum — [FREE] StoreUI](https://devforum.roblox.com/t/free-storeui-an-animated-modern-gamepass-store-ui/632073)
- [Game UI Database — Crates & Booster Packs](https://gameuidatabase.com/index.php?scrn=125&set=1&tag=22,40,1)
- [Mobile game progression / treasure chests (Medium)](https://simon-truong.medium.com/mobile-game-progression-389a6623e458)
- [Valorant UI redesign case study](https://vossel.ca/valorant-redesign/)
- [Overwatch main menu critique](https://medium.com/@joelsgrobinson/overwatch-ui-redesign-b32e8fa32685)
- [Skeleton loading screen design (LogRocket)](https://blog.logrocket.com/ux-design/skeleton-loading-screen-design/)
- [Animating number counters (CSS-Tricks, technique-agnostic principles)](https://css-tricks.com/animating-number-counters/)

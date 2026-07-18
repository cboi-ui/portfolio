---
name: ui-animate
description: Implement real hover/press feedback and a full open/close (entrance/exit) animation sequence for a Roblox UI panel — writes an actual LocalScript, not just property tweaks. Use when asked to animate a panel/popup/shop opening and closing, add hover/click feedback to its buttons, or make a UI feel "polished"/"juicy"/"bouncy". Builds on ui-prep (geometry) and ui-motion-research (standards).
user-invocable: true
---

# ui-animate: implement panel open/close + hover/press animation

This is the **implementation** skill — it writes a real `LocalScript` that
drives hover, press, and entrance/exit choreography for a UI panel. It
assumes the panel's geometry is already correct (`ui-prep`) and draws its
timing/easing decisions from `ui-motion-research`'s standards rather than
improvising. Read both of those first if you haven't already touched this
panel this session.

## Prerequisites — check before writing any code

1. **Geometry**: every button/card that will scale on hover/press must have
   `AnchorPoint = (0.5, 0.5)` with compensated Position (`ui-prep`'s job).
   Animating an off-center element makes it visibly jump/orbit from a
   corner — check this first, don't discover it mid-implementation.
2. **Standards**: skim `ui-motion-research/reference/timing-easing-feedback.md`
   and `standards-and-mistakes.md` for the duration/easing table and the
   motion-token idea. This skill's defaults below already follow them, but
   re-check if the panel has unusual requirements.
3. **Map the hierarchy** before writing anything: identify the root panel
   (the thing that scales/fades in as a whole), any `UILayout` managing a
   list of items (their `Position` cannot be permanently hand-edited —
   see the item-slide technique below), the open trigger, the close
   button, and every hover/click target.

## Core technique: animate via UIScale, never the base Size/Position

Insert a `UIScale` on the object being grown/shrunk and tween *its*
`Scale` property. This leaves the object's real `Size` (already correct
from `ui-prep`) untouched, composes cleanly with nested hover targets
(a card's `UIScale` and a button inside it both scaling independently
multiply naturally — a card lifting while its Buy button *also* grows on
direct hover is a feature, not a bug), and means resetting to "normal" is
always just `Scale = 1`. See `reference/helpers.md` for the exact
`getOrCreateUIScale` + `springTo` pattern (a tween-cancelling wrapper
around `TweenService:Create` keyed by instance, so a rapid re-hover
doesn't race two tweens on the same property).

For Position-based motion (the open button's coupled text-shift, the item
slide-in), tween `Position` directly — there's nothing to reset a Size for.

## Hover/press feedback — the default per-button wiring

Every clickable element gets, at minimum:
- `AutoButtonColor = false` (the built-in darken-on-click fights a custom
  scale/press animation — always turn it off when adding your own).
- **MouseEnter** → grow via `UIScale`. For a genuinely *bouncy*, cartoony
  feel (matching Twinkle's `Presets/Hover/Bounce.lua`, which used
  `Elastic`/`Out` rather than `Back`), use `TweenInfo(0.4-0.45, Elastic,
  Out)` — noticeably springier than `Back`'s single mild overshoot, and
  the extra duration is fine here because hover isn't latency-sensitive
  the way press/click is (it's continuous/exploratory, not a discrete
  confirmed action). `Back`/`0.18-0.2s` is a fine, calmer default if the
  project doesn't want that much bounce.
  Grow amount scales inversely with element size: small buttons ~8-12%.
  **Wide, short elements (a list-row card) need a much smaller grow
  percentage** — e.g. ~2% not ~4-5% — because a uniform `UIScale` grows
  both axes by the same *percentage*, and a wide card's absolute pixel
  growth on the X axis will dwarf its Y-axis growth, reading as "growing
  sideways" even though the scaling was perfectly uniform. Check the
  element's aspect ratio before picking a card's grow amount, not just
  its role.
- **MouseLeave** → back to `Scale = 1`. Keep this one calmer/faster
  (`Back`/`0.18s`) even if hover-in uses `Elastic` — a lingering bounce on
  the way *out* reads as sluggish once attention has already moved on.
- **InputBegan** (MouseButton1/Touch) → quick squash, `Scale ≈ 0.88-0.92`,
  `TweenInfo(0.09-0.11, Quad, Out)` — fast, no bounce on the way down.
- **InputEnded** → spring back to **whichever scale is currently correct**
  (the hover-grown scale if the cursor is still over the element, `1` if
  not — track an `isHovering` boolean, don't hardcode the recovery
  target). `TweenInfo(0.18-0.2, Back, Out)` for a satisfying little pop on
  release.

This directly avoids the failure case reviewed in this project's Twinkle
model audit: a *default* press animation must stay brief (100-150ms
round-trip order of magnitude) — a 700ms Elastic squash on every routine
click reads as sluggish, not premium. Save Elastic/Bounce/longer durations
for rare celebratory moments, not routine buttons.

A close/"X" button can get a little extra personality on top of the
standard grow: a one-shot rotation "wag" (e.g. 0° → 10° → 0°, two quick
`Quad Out` tweens chained off `.Completed`) fired once per hover-enter,
independent of the persistent grow/shrink state.

## Entrance / exit choreography

**Entrance**, roughly in this order, all non-blocking (`task.delay`, never
a yielding `:Wait()` in the click handler — the open/close functions
should return immediately):
1. The open-trigger button shrinks away (`UIScale → 0`) and hides once that
   tween's own `.Time` elapses.
2. The panel becomes `Visible = true` and simultaneously: `UIScale` scales
   from a small starting value (~0.7) to `1` with `Back Out` (~0.4-0.45s,
   the one deliberate overshoot on the way in), while a wrapping
   `CanvasGroup.GroupTransparency` fades `1 → 0` with `Quad/Cubic Out`
   slightly *faster* than the scale settles (~0.25-0.3s) — finishing the
   fade before the bounce fully resolves avoids a lingering "translucent
   jelly" look.
3. After a short delay (~0.1-0.15s so the panel's own motion reads first),
   cascade in the decorative header elements (background rays, icon,
   title) with small staggered delays between each (~0.05-0.08s apart) —
   fade plus a *small* position settle (a few percent of scale, not a full
   slide) reads as "simple animation" without being fussy.
4. The close button pops in **last** (`UIScale 0 → 1`, `Back Out`) — it's
   the dismiss action, least urgent to reveal first, and arriving last
   gives the sequence a clear finishing beat.
5. List items (if any) slide in from the left, staggered — see the
   technique below.

**Exit**, deliberately simpler and faster than entrance (per the standard:
close < open, no overshoot on the way out):
1. Panel `UIScale → ~0.85` and `GroupTransparency → 1` together, one
   `Cubic/Quad In` tween, ~0.2s. No per-element reverse-cascade — reversing
   every entrance beat individually is slow and unnecessary; a single
   clean fade+shrink of the whole `CanvasGroup` reads as fast and
   intentional.
2. Once that tween's `.Time` elapses: `Visible = false`, reset every
   decorative element back to its hidden starting state (**see the pitfall
   below**), and bring the open-trigger button back with a small `Back
   Out` pop.

### Item slide-in-from-left, without fighting a UIListLayout

If items are positioned by a `UIListLayout`, **never** permanently
hand-edit their `Position` — the layout owns it. The safe technique:
read `item.Position` at animation time (this is already the real,
layout-computed resting position — layouts resolve well before a click
handler runs), shift it left by more than the item's own width
(`X.Scale - 1.1` or so, guaranteeing it starts fully outside a clipped
container), then tween back to the exact value you just read. A
`UIListLayout` only recalculates when children are added/removed/resized —
it won't fight a one-off tween that isn't touching those things. Stagger
each item's start by index (`~0.08s` apart), `Back Out`, ~0.3s each.

## The pitfall that *will* bite you: resetting state for the next open

Everything you set to a "hidden" starting value before the first entrance
(`UIScale = 0`, `Transparency = 1`, an offset `Position`) must be reset
back to that same hidden value at the **end of the exit** animation, not
just once at script load. If a `UIScale` gets created lazily the first
time some hover/press wiring runs, and your "reset to closed" function was
written before that `UIScale` existed, it'll silently skip resetting it —
the element will pop in fully visible on the *second* open instead of
playing its entrance, because nothing ever put it back to 0. This is a
real bug caught during this skill's own first implementation — **the fix
is to create every `UIScale`/capture every "rest" value up front, before
writing the reset function**, and to explicitly test a second full
open→close→open cycle (not just one), specifically to catch exactly this
class of bug. A single test pass will not surface it.

## Verification — do this every time, not just once

1. Enter Play mode (`start_stop_play`), check `get_console_output` for
   errors immediately after load.
2. `user_mouse_input` to hover each distinct interactive element,
   screenshot, confirm the grow is visible and no errors appeared.
3. `mouseButtonDown`/`mouseButtonUp` (or `mouseButtonClick`) to test press,
   screenshot mid-press if possible.
4. Click the open trigger, screenshot after the full entrance duration has
   elapsed, check console.
5. Click close, screenshot, check console, confirm the open trigger
   reappeared.
6. **Open a second time** and screenshot — this is the step that catches
   the reset-state pitfall above. Don't skip it.
7. Only report the work as done after this full cycle has been observed
   with no console errors — not after just reading the code back.

## Reference files

- `reference/helpers.md` — the exact `springTo`/`getOrCreateUIScale`/
  `isPrimaryInput`/generic-button-feedback Luau snippet, ready to paste
  into a new panel's animation script rather than re-deriving it.

---
name: ui-motion-research
description: Pre-researched knowledge base for designing Roblox UI animations — motion theory, Roblox-specific implementation (TweenService/UIScale/Style Transitions), timing/easing standards, feedback-state patterns, and common mistakes. Load before designing or implementing any hover/click/popup/reward/notification animation, or when asked "how should this animate" / "what's the right duration/easing for X."
user-invocable: true
---

# ui-motion-research: animation knowledge base

This is a **research skill, not a scripting skill** — it exists so animation
decisions (durations, easings, which state gets which treatment) are drawn
from an actual knowledge base instead of improvised per-button. Pair it with
`ui-prep` (which gets the geometry — AnchorPoint/Position — ready to animate)
and whatever scripting work implements the result.

The research behind this skill was already done (web search across motion
design theory, Roblox docs/DevForum, UX articles, accessibility guidelines,
game-feel theory — see each reference file's own Sources section). **Don't
re-derive this from scratch each session** — read the relevant reference
file(s) below and apply them. Only re-research if something here looks
stale (e.g. a Roblox beta feature ships or changes) or the user asks for
something the existing research doesn't cover.

## Reference files — read on demand, not all up front

- `reference/motion-principles.md` — *why* animation matters: purpose of
  motion, Disney's 12 principles → UI translation, Material/Apple motion
  guidelines, game feel & "juice," psychology of anticipation/reward. Read
  this when the question is "should this animate at all" or "why does this
  feel bad."
- `reference/roblox-implementation.md` — *how* to build it in Roblox:
  TweenService patterns, EasingStyle/Direction table, UIScale vs
  CanvasGroup vs UIStroke/UIGradient animation, the Style Transitions beta,
  AnchorPoint/Scale-vs-Offset interplay with tweening, and performance
  pitfalls (UIListLayout recalculation, tween object reuse). Read this
  before writing any Luau animation code.
- `reference/timing-easing-feedback.md` — the numbers: duration table by
  interaction type (hover/click/popup/toast/reward/etc), full easing-style
  reference (use case, pros/cons, when to avoid), and the UI feedback-state
  taxonomy (hover/pressed/disabled/error/success/loading/...). Read this
  when picking a specific duration or easing, or wiring up a new
  interactive state.
- `reference/patterns-and-categories.md` — recurring patterns from Roblox
  and industry games (shops, popups, reward/loot reveals, menus) and a
  catalog of animation categories (pop, slide, count-up, shimmer/skeleton,
  card stack, etc.) with per-category best practices. Read this when
  designing a new animated component and want a reference pattern rather
  than inventing one.
- `reference/standards-and-mistakes.md` — the actual recommended standard
  (durations/easings by category, in one table), how to keep it consistent
  via motion tokens instead of ad hoc per-tween values, accessibility
  (reduced motion) requirements, audio-sync guidance, and the most common
  mistakes to check any new animation against before shipping it.

## How to use this when asked to animate something

1. Skim `standards-and-mistakes.md`'s standard table first — it's the
   fastest path to "what duration/easing should this be" for the common
   cases (hover, click, popup, reward, notification, error).
2. If the component doesn't fit a standard case, check
   `patterns-and-categories.md` for the closest existing pattern before
   inventing a new one.
3. Check `roblox-implementation.md` for the correct Roblox mechanism
   (CanvasGroup for transparency/color of a whole hierarchy, UIStroke
   child for border animation, TweenService reuse for fixed repeated
   tweens, Style Transitions beta if the project has it enabled).
4. Run the result against `standards-and-mistakes.md`'s mistake checklist
   and the accessibility section before calling it done.
5. If geometry isn't animation-ready yet (off-center anchors, uncompensated
   Position), that's `ui-prep`'s job, not this skill's — hand it off rather
   than patching Position inline.

## Keeping this current

This is a living knowledge base, not a one-time snapshot. If a session
does new research (a Roblox engine change, a newly discovered pattern, a
correction the user gives), update the relevant reference file rather than
letting the correction live only in that session's transcript.

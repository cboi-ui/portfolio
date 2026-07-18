# Recommended standard, consistency systems, accessibility, and mistakes

## Motion tokens: keep it consistent instead of ad hoc

The documented failure mode (from design-systems literature, e.g. work on
motion design tokens): two different parts of the same product end up with
different durations/easings for what's conceptually the same transition,
because each was specced independently. The fix that keeps recurring across
sources is the same one color/spacing tokens solve for visual design:
**define a small, named set of duration + easing pairs once, and reuse
them by name everywhere**, instead of picking a fresh number per tween.

For this project, a practical Luau-side version of that idea (a module
exposing named presets) rather than magic numbers scattered across
scripts:

```lua
local MotionTokens = {
    Snappy   = TweenInfo.new(0.15, Enum.EasingStyle.Quad,  Enum.EasingDirection.Out), -- hover/press
    Standard = TweenInfo.new(0.25, Enum.EasingStyle.Cubic, Enum.EasingDirection.Out), -- open/close/tab-switch
    Celebrate= TweenInfo.new(0.6,  Enum.EasingStyle.Back,  Enum.EasingDirection.Out), -- rewards/unlocks
}
```

Every new interactive element should reach for one of a *small* named set
like this rather than inventing new duration/easing combinations — that's
the entire practical value of a motion-token system, and it's what makes
"is this UI consistent" answerable at a glance instead of requiring an
audit of every script.

## Recommended standard (the condensed, actionable table)

| Category | Duration | Easing | Notes |
|---|---|---|---|
| Hover | 150–250ms | Quad/Cubic Out | Subtle scale/glow only |
| Click/Press | 100–150ms | Quad Out (in), Back Out small amount (optional accent) | Briefest state |
| Menu/Tab switch | 200–300ms | Cubic InOut | Preserve context |
| Popup/Modal open | 250–400ms | Cubic Out, paired with fade | Scale from center (0.8→1.0 typical) |
| Popup/Modal close | 200–300ms | Cubic In (or matching Out reversed) | Faster than open |
| Notification/Toast | in ~400ms / hold 3–5s / out up to ~1800ms | Cubic Out (in), linear or Cubic In (out) | Don't block interaction for passive toasts |
| Reward/Unlock reveal | 500ms–2s incl. build-up | Back/Bounce/Elastic Out for the reveal itself | The one place overshoot easing is expected; scale intensity with rarity |
| Shop/Inventory item added/removed | 200–300ms | Cubic Out | Pair with a currency count-up if applicable |
| Error feedback | 300–400ms | Quad/Cubic, sharp, no loop | Brief shake or flash, then stop |
| Success feedback | 250–400ms | Cubic Out | Brief, non-blocking |
| Loading/skeleton | continuous until resolved | Linear (shimmer sweep) | Only for waits >~0.5s |
| Large/full-screen transition | 400–600ms | Cubic InOut | If it needs longer, reconsider as a loading state |

## Accessibility: motion must be optional

- **`Reduce Motion`-equivalent support matters even without a platform-
  level API to hook into** (Roblox has no built-in universal reduced-motion
  flag the way iOS/web do) — if this project ever needs it, the practical
  approach is a player-facing settings toggle that swaps the "Standard"/
  "Celebrate" motion-token durations down toward near-zero (or disables
  non-essential animation categories like parallax/pulse/glow) rather than
  building a second parallel UI.
- **Never gate essential information behind animation alone.** If a
  reward's *value* is only clearly conveyed via animation timing (e.g. a
  count-up is the only place the final number appears), a
  reduced-motion/skipped-animation path must still show the end value
  immediately — this is the direct UI parallel to WCAG's guidance that
  animation must not be the sole channel for critical information.
  Recommended durations in this guide already assume some users can/should
  skip straight to end-state (tap-to-skip on reward reveals is common and
  fine).
- **Keep the vestibular-risk categories especially conservative**:
  parallax, screen-shake-style effects, and continuous background motion
  are the specific categories called out across accessibility sources as
  the most likely to actually cause discomfort (not just annoyance) for
  vestibular-disorder users — these are exactly the categories this
  project's CLAUDE.md already flags as "default to subtle."

## Audio-sync guidance

- Treat UI sound as **one more channel of the same feedback event**, not a
  separate thing — the sound and the animation should start at the same
  frame for a click/press; a lag between them (even 50-100ms) reads as
  broken more than a purely visual delay would.
- Build UI sounds from a shared "sound family" (same synthesis/base
  character across hover/click/success/error) so the audio layer feels as
  consistent as a motion-token system makes the visual layer feel — this
  is the audio equivalent of "don't invent a new easing per screen."
- Short sounds only for high-frequency interactions (hover/click) —
  anything longer than the interaction itself becomes a annoyance on
  repeated triggering (e.g. rapid button mashing).
- Reserve distinct, longer, more elaborate sound design for the same rare
  moments that get elaborate motion (rewards, purchases, level-ups) —
  audio and motion "budget" scale together.

## Common mistakes checklist

Run every new animation against this before considering it done:

- [ ] **Overuse** — is everything on this screen moving, so nothing stands
      out? (Staging principle — pick one focal motion, keep the rest
      quiet or static.)
- [ ] **Too slow** — is anything routine/frequent (hover, press, tab
      switch) taking longer than ~300ms? Users perceive that as lag, not
      polish.
- [ ] **Inconsistent easing** — does this use a different curve than the
      rest of the interface for the same *category* of transition? Pull
      from the motion-token set instead of picking a new one.
- [ ] **Overshoot on the wrong things** — is Back/Bounce/Elastic applied to
      something routine (a settings toggle, a disabled state) rather than
      a genuine rare/celebratory moment?
- [ ] **Animated disabled state** — does a disabled control still play
      hover/press animation? It shouldn't; disabled must read as inert.
- [ ] **Off-center pivot** — does anything scale/rotate visibly from a
      corner instead of its intended pivot? (This is `ui-prep`'s exact
      job — check `AnchorPoint`/Position compensation before blaming the
      tween.)
- [ ] **No reduced-motion / skip path** for anything long (reward reveals,
      large transitions) — can impatient or motion-sensitive players get
      to the end state without waiting through it?
- [ ] **Layout thrash** — is a `UIScale`/size tween happening on an element
      governed by `UIListLayout`/`UIGridLayout`, forcing per-frame layout
      recalculation? (See `roblox-implementation.md`'s performance
      section.)
- [ ] **Audio/visual desync** — does the sound fire noticeably before or
      after the animation's start frame?
- [ ] **Essential info gated behind animation only** — if this animation
      were skipped entirely, would the user still end up knowing what
      happened (new value, success/failure, unlocked state)?

## Sources
- [Motion design tokens for design systems (Medium)](https://medium.com/@ogonzal87/animation-motion-design-tokens-8cf67ffa36e9)
- [Not Just Colors and Fonts: motion tokens (Design Systems Collective)](https://www.designsystemscollective.com/not-just-colors-and-fonts-why-motion-tokens-belong-in-every-modern-design-system-8a2dcdc00659)
- [W3C WCAG 2.3.3 — Animation from Interactions](https://www.w3.org/WAI/WCAG21/Understanding/animation-from-interactions.html)
- [Accessible motion (IBM Design / Medium)](https://medium.com/design-ibm/accessible-motion-why-its-essential-and-how-to-do-it-right-ff38afcbc7a9)
- [Designing Safer Web Animation for Motion Sensitivity (A List Apart)](https://alistapart.com/article/designing-safer-web-animation-for-motion-sensitivity/)
- [Best Practices for Game UI Sounds (SFX Engine)](https://sfxengine.com/blog/best-practices-for-game-ui-sounds)
- [Web animation mistakes to avoid (Pixel Free Studio)](https://blog.pixelfreestudio.com/web-interface-animation-mistakes-to-avoid/)
- [7 Practical Animation Tips (Emil Kowalski)](https://emilkowal.ski/ui/7-practical-animation-tips)

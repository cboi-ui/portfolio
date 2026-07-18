# Roblox implementation: how to actually build it

## TweenService basics

```lua
local TweenService = game:GetService("TweenService")

local tweenInfo = TweenInfo.new(
    0.2,                          -- duration (seconds)
    Enum.EasingStyle.Quad,        -- style (default is Quad if omitted)
    Enum.EasingDirection.Out,     -- direction (default Out)
    0,                            -- repeat count
    false,                        -- reverses
    0                             -- delay
)

local tween = TweenService:Create(button, tweenInfo, {
    Size = UDim2.fromScale(0.22, 0.09),
})
tween:Play()
```

- Multiple properties can be tweened in one `Create()` call (e.g. `Size`
  *and* `Rotation` together) — prefer this over separate simultaneous
  tweens on the same object.
- Chain sequential animations off the `Completed` event, not a `wait()` —
  `tween.Completed:Connect(function() nextTween:Play() end)`.
- **Reuse tween objects for fixed, repeated animations** (e.g. the same
  hover-grow tween fired every time the mouse enters a button) rather than
  calling `TweenService:Create()` on every trigger — creating a Tween has
  real overhead, and if the target values never change there's no reason
  to rebuild it repeatedly (this is a specific, documented DevForum
  recommendation, not a general micro-optimization guess).
- Boolean properties (`Visible`) cannot be tweened directly — animate
  `Transparency` (or `GroupTransparency` via CanvasGroup) and toggle
  `Visible` at the start/end instead, not mid-tween.
- The deprecated `TweenPosition()`/`TweenSize()` GuiObject methods should
  not be used in new code — use `TweenService:Create()` with a real
  `Tween` object instead.

## EasingStyle reference (Roblox's `Enum.EasingStyle`)

Roblox exposes 11 named curves plus a direction modifier. Full guidance on
each curve's *use case* lives in `timing-easing-feedback.md`; the Roblox-
specific notes are:

- Default `EasingStyle` if unspecified is `Quad`; default `EasingDirection`
  is `Out`.
- `Linear` ignores `EasingDirection` entirely (there's nothing to reverse).
- `Back`, `Bounce`, and `Elastic` are the three "overshoot" families — use
  them deliberately for accent moments (reward reveals, celebratory pops),
  not for routine UI motion (see `standards-and-mistakes.md`'s mistakes
  list on overshoot overuse).

## Property-specific animation techniques

- **Position/Size:** use `UDim2` **Scale**, not Offset, so the tween lands
  at the correct relative position regardless of screen size — this is
  Roblox's own stated recommendation, not just a general "scale is better"
  opinion. If a scaling element must keep its proportions, add a
  `UIAspectRatioConstraint` so tweening Size doesn't distort it.
- **Rotation:** plain numeric degrees; the element's `AnchorPoint` must
  already be centered (or wherever the intended pivot is) before tweening
  rotation, or it'll orbit around the wrong point — this is exactly what
  `ui-prep` exists to fix ahead of time.
- **Transparency/Color across a whole hierarchy:** wrap the hierarchy in a
  `CanvasGroup` and tween `GroupTransparency`/`GroupColor3` once, instead
  of tweening `BackgroundTransparency`/`ImageTransparency`/`TextTransparency`
  on every descendant individually. This is both simpler code and better
  performance (one tween instead of N).
- **Border/outline animation:** add a `UIStroke` child and tween its
  `Thickness`, `Color`, or `Transparency` — don't try to fake a border by
  animating a background Frame's size.
- **Gradient animation:** `UIGradient`'s `Offset`, `Rotation`, or `Color`
  sequence can be tweened directly for shimmer/sweep effects (see the
  shimmer pattern in `patterns-and-categories.md`).
- **Hover/press scale:** insert a `UIScale` on the button and tween *its*
  `Scale` property (e.g. 1.0 → 1.05) instead of tweening the button's own
  `Size` — this is the standard idiom for "grow slightly on hover" and
  composes cleanly with the button's real Size/Position. Requires the
  button's `AnchorPoint` to be centered first (again, `ui-prep`'s job).

## The Style Transitions beta (as of 2026)

Roblox Studio has a beta feature, **Styling Transitions** (`File > Beta
Features > Styling Transitions`), that lets property tweens be declared
directly in Stylesheets/StyleRules — a declarative, CSS-transition-like
alternative to hand-writing `TweenService` calls for simple state-driven
animations (e.g. "this StyleRule's BackgroundColor3 transitions over
0.2s"). It uses the same `EasingStyle`/`EasingDirection` vocabulary as
`TweenInfo` under the hood. This is genuinely new (announced ~May 2026)
and worth checking for on any project before hand-rolling a tween for a
simple state change — if the project already uses a Stylesheet-based UI
system, a style transition may be less code than a script. It doesn't
replace `TweenService` for anything sequential, chained, or
programmatically triggered (reward reveals, multi-step popups) — those
still need real Tween objects.

## Performance pitfalls

- **UIScale + UIListLayout is a known bad combination.** Resizing/tweening
  through a `UIScale` on an element that's a child managed by
  `UIListLayout` forces the layout to recalculate on every frame of the
  tween, which is documented on the DevForum as a real, sometimes severe,
  performance hit ("UpdateUILayouts gives huge performance impact"). If a
  list-managed item needs a hover/press scale effect, prefer scaling a
  *child* of the list item (a nested Frame) rather than the list item
  itself, so the layout doesn't need to react.
- **A `UISizeConstraint` will override layout-driven sizing** — useful
  defensively if a layout and a tween are fighting over an object's size,
  but don't rely on it as the fix for the above; it changes *what* wins,
  not whether the layout recalculates.
- **Batch, don't spam.** Firing many independent tweens in the same frame
  (e.g. animating 20 shop items in on open) is fine functionally but scales
  linearly in tween-object overhead — stagger with small `delay` values or
  a single driving tween (e.g. tween one dummy NumberValue and derive the
  rest) if the count gets large (dozens+).
- **Low-end mobile budget:** a 60fps target gives ~16.6ms per frame; a
  30fps target (acceptable for turn-based/non-twitch UI-heavy games) gives
  ~33ms. UI animation itself is rarely the bottleneck compared to
  particles/shaders, but stacking many simultaneous tweens plus layout
  recalculation plus a busy `RenderStepped` script *is* a realistic way to
  blow the budget on low-end devices — the general industry-wide fix
  pattern is: reduce concurrent animation count and shorten durations on
  detected low-end devices rather than disabling animation outright.

## Sources
- [Roblox Creator Hub — UI animation/tweens](https://create.roblox.com/docs/ui/animation)
- [Roblox DevForum — Good practice for UI Animation](https://devforum.roblox.com/t/good-practice-for-ui-animation/2970238)
- [Roblox DevForum — UpdateUILayouts performance impact](https://devforum.roblox.com/t/updateuilayouts-gives-huge-performance-impact/4064226)
- [Roblox Creator Hub — Size modifiers and constraints](https://create.roblox.com/docs/ui/size-modifiers)
- [Roblox DevForum — [Studio Beta] Styling Transitions](https://devforum.roblox.com/t/studio-beta-styling-transitions/4646870)

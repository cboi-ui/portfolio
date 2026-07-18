# Timing, easing, and feedback-state reference

## Duration standard by interaction type

General rule across every source consulted (NN/g, Material Design,
multiple UX practitioner articles): **most UI animation should land in the
100–500ms band.** Below ~100ms it isn't consciously perceived as motion;
above ~1s it reads as lag. Within that band:

| Interaction | Duration | Notes |
|---|---|---|
| Hover (enter/exit) | 150–250ms | Must feel instant; never make the user wait on a hover state. |
| Button press/release | 100–150ms | The briefest state — just long enough to register, no longer. |
| Micro-interaction (toggle, checkbox) | 120–200ms | Same tier as press. |
| Click confirmation (non-modal, e.g. "added" pulse) | 300–400ms | Enough to be seen without blocking the next action. |
| Menu/tab switch | 200–300ms | Preserve context; don't make navigation feel heavy. |
| Popup/modal open | 250–400ms | Material's guidance: scale duration to the distance/area covered. |
| Popup/modal close | 200–300ms | Slightly faster than open is common — exits get less attention. |
| Toast/notification enter | ~400ms | Matches roughly how long it takes to redirect user focus. |
| Toast/notification hold | 3–5s | 3s is the most cited "ideal" for a passive, non-blocking toast. |
| Toast/notification exit | up to ~1800ms | Slower exit gives time to register it leaving, if it's not urgent. |
| Reward/celebration reveal | 500ms–2s+ (including a deliberate pre-reveal pause) | The one category allowed to run long — see the anticipation principle in `motion-principles.md`. Don't rush this one to fit the general band. |
| Large/full-screen transition | 400–600ms | Longest "routine" case; if it needs longer, question whether it should be a loading state instead. |
| Error shake/flash | 300–400ms | Sharp and brief — a lingering error animation reads as nagging. |

**When in doubt, favor the shorter end for anything the user will trigger
repeatedly (hover, press, tab switch), and only budget extra time for
things that happen rarely (reward reveal, first-time onboarding, big
transitions).** This mirrors the "juice frequent things lightly, rare
things heavily" principle from `motion-principles.md`.

## Easing style reference (use case / pros / cons / avoid)

| Style | Typical use | Pros | Cons | Avoid when |
|---|---|---|---|---|
| **Linear** | Loading bars, continuous background motion (parallax, marquee) | Predictable, mechanical-but-honest for literal progress | Feels robotic for anything meant to feel physical | Any discrete UI transition (open/close/press) — almost never the right choice there |
| **Sine** | Gentle idle motion (a slow pulsing glow) | Very soft, subtle | Can feel imprecise/mushy for anything needing snap | Snappy feedback (press/click) |
| **Quad** | General-purpose default (Roblox's own default) | Safe, mild, works almost everywhere | Unremarkable — won't add personality | A moment that specifically needs to feel premium/springy |
| **Cubic** | Panel/menu open-close, most "standard" transitions | Good default for anything needing to feel deliberate | — | — |
| **Quart/Quint** | Larger or faster transitions needing more dramatic accel/decel | More pronounced snap than Cubic | Can feel abrupt if overused on small elements | Small, frequent micro-interactions (hover) |
| **Exponential** | Very sharp accel/decel, fast reveals | Strong, attention-grabbing | Easily feels jarring if used broadly | Anything meant to feel calm/passive |
| **Circular** | Sudden acceleration, gradual settle | Distinctive character | Less commonly recognized/idiomatic than Cubic/Quad | Default choice — reserve for specific character, not general use |
| **Back** | Button press feedback, small pop-ins | Adds tactile, physical "give" | Overshoot can look like a bug if too large | Large or frequent elements — subtle amounts only |
| **Bounce** | Reward reveals, celebratory pop-ins, playful brands | High personality, reads as fun/juicy | Wrong tone for serious/minimal UI; slow if the bounce count is high | Routine buttons, error states, anything frequent |
| **Elastic** | Loot/reward reveals, rare celebratory moments | Strongest "alive" feeling available | Easily overdone; wrong for anything not a genuine celebration | Anything that happens more than occasionally |

General rule pulled from multiple UX sources independently: **standardize
on 2–3 easing presets for the whole interface** (e.g. "Standard" = Cubic
Out for opens/closes, "Snappy" = Quad Out for hover/press, "Celebrate" =
Back/Elastic Out for rewards) rather than picking a new curve per screen —
inconsistent easing across an interface reads as unpolished even when each
individual choice is defensible in isolation. This is exactly what a
motion-token system (`standards-and-mistakes.md`) formalizes.

`EasingDirection`: **Out** (decelerate into rest) is the correct default
for almost everything appearing/settling on screen. **In** (accelerate
away) suits things leaving/dismissing. **InOut** suits motion between two
on-screen states where neither end is "rest" (e.g. a tab indicator sliding
between two tabs).

## UI feedback-state taxonomy

Every interactive element potentially needs treatment for each of these —
not every element needs every state (a decorative icon has no "disabled"),
but check the applicable ones exist and are visually distinct from each
other:

- **Hover** — signals interactivity before commitment. Subtle: slight
  scale/glow/color shift. Must resolve near-instantly (150–250ms).
- **Pressed** — confirms input was registered *before* the result is known.
  Briefest state (100–150ms); typically a scale-down or darken.
- **Released** — return to rest or transition to the result state; often
  implicit in the press animation's reverse.
- **Disabled** — must read as non-interactive at a glance (reduced
  opacity/desaturation), and critically, **should not carry a hover/press
  animation at all** — an animated disabled control is a common and
  confusing mistake.
- **Focused** (keyboard/gamepad navigation) — a visible outline/highlight
  distinct from hover, since a controller/keyboard user won't trigger
  hover at all. Relevant for Roblox `Selectable`/`SelectionImageObject`
  driven navigation, not just mouse UIs.
- **Locked** — visually distinct from merely disabled (e.g. a lock icon
  overlay) since the *reason* for non-interactivity differs (permanently
  unavailable vs. temporarily unavailable) and should communicate that
  difference.
- **Selected/Deselected** — persistent state (a chosen tab, an equipped
  item) vs. transient hover/press — should look different from hover so
  a user can tell "this is currently active" from "my mouse happens to be
  here."
- **Purchased/Unlocked** — a state-change animation (not just a static
  end-state) reinforces that the action succeeded; pairs well with the
  count-up/currency-loss patterns in `patterns-and-categories.md`.
- **Error/Warning** — sharp, brief (300–400ms), often a shake or red flash;
  should not loop or linger — a persistently animated error nags rather
  than informs.
- **Success** — typically a checkmark or green pulse; per NN/g and
  Material guidance, brief and non-blocking is correct — success doesn't
  need to hold the user's attention the way an error does.
- **Loading/Cooldown/Progress** — continuous motion (spinner, progress
  bar, skeleton shimmer) signals "working," not "stuck." See the skeleton/
  shimmer pattern in `patterns-and-categories.md` — skeleton loaders are
  only worth using for waits longer than ~0.5s; shorter than that, a
  loading indicator itself becomes visual noise for no benefit.

## Sources
- [NN/g — Executing UX Animations: Duration and Motion Characteristics](https://www.nngroup.com/articles/animation-duration/)
- [Material Design 3 — Easing and duration](https://m3.material.io/styles/motion/easing-and-duration)
- [Roblox Creator Hub — UI animation/tweens (EasingStyle table)](https://create.roblox.com/docs/ui/animation)
- [NN/g — Button States: Communicate Interaction](https://www.nngroup.com/articles/button-states-communicate-interaction/)
- [Material Design — States](https://m3.material.io/foundations/interaction/states)
- [Toast notification timing guidance (Cogtix)](https://www.cogtix.com/blogs/toast-notifications-a-guide-to-better-user-experience)
- [Toast entrance/exit timing (Medium)](https://medium.com/design-bootcamp/toast-notifications-how-to-make-it-efficient-400cab6026e9)

# Motion principles: why animation matters

## The point of UI motion (not decoration)

Motion in an interface does real communicative work — it isn't there to
"look nice." The consistent thread across Apple's HIG, Material Design, and
UX research (Nielsen Norman Group) is that motion should:

- **Communicate state change.** Showing *how* something changed (a panel
  opening, an item moving to a cart) is more legible than a hard cut,
  because it preserves the user's mental model of where things went.
- **Provide feedback.** Confirms an input was registered before any other
  result is visible (a button's press-state, a toggle's flip).
- **Show affordance.** A subtle hover lift or glow tells the user something
  is interactive *before* they commit to clicking it.
- **Direct attention (staging).** Motion is one of the strongest visual
  hierarchy tools — the eye goes to what's moving. Used well, this guides a
  user to what matters; used everywhere, it guides them to nothing (see
  "overuse" in `standards-and-mistakes.md`).
- **Establish continuity.** An element that visibly travels from A to B is
  easier to track than one that vanishes at A and appears at B — reduces
  cognitive load when reasoning about where content went.
- **Set perceived quality.** Apple's guidance is explicit that "quick,
  precise" animation reads as lightweight and premium, while unnecessary or
  sluggish motion reads as the opposite — this is the mechanism by which
  animation quality becomes a proxy for overall production quality in a
  player's mind (see the psychology section below).

Apple's HIG is also explicit about the failure mode: **avoid unnecessary
motion.** The system (and by extension, a well-built game UI) already
provides feedback for standard interactions — don't make users sit through
decorative motion on every repeated interaction just because it once looked
good in a demo.

## Disney's 12 Principles of Animation → UI

Originated by Ollie Johnston and Frank Thomas (*The Illusion of Life*,
1981). Not all 12 translate to UI work, but these do, directly:

- **Squash and stretch.** A button that compresses slightly on press reads
  as tactile/physical, not just a color change. Use sparingly and small —
  a UI element isn't a cartoon character; a few percent of scale change is
  usually enough.
- **Anticipation.** A small telegraphing motion before the main action
  (a slight pull-back before a card flies out) helps the eye track what's
  about to happen. In UI this is often just "ease-in before the real
  motion" rather than a separate animation.
- **Staging.** Keep everything that *isn't* the point of the animation
  visually quiet while it plays. Don't animate five things at once with
  equal intensity — one clear focal motion beats five competing ones.
- **Arc.** Natural motion curves rather than moving in straight lines,
  especially for anything meant to feel alive (a coin flying to a counter,
  a card being drawn) rather than mechanical (a menu simply sliding on one
  axis, which is fine to keep linear/straight — arcs aren't universal,
  they're for things standing in for physical objects).
- **Slow in, slow out** (this is just easing — see
  `timing-easing-feedback.md`). Constant-speed (linear) motion reads as
  robotic; almost everything benefits from ease-out at minimum.
- **Exaggeration.** Reward/celebration moments can push further than
  functional UI moments (a legendary item reveal can overshoot and bounce;
  a settings toggle should not).
- **Appeal.** The overall charisma of the motion + visual design combined —
  this is the difference between an interface that feels inviting and one
  that feels sterile, and it's a design-taste judgment more than a formula.

## Material Design & Apple HIG, condensed

**Material Design** (both the older "standard/deceleration/sharp" curve
model and the current M3 easing/duration tokens):
- Duration should scale with the *distance/area* an element travels or
  changes, not be a single fixed number for everything — small changes
  (icon toggle) get short durations, large changes (full-screen transition)
  get longer ones.
- Motion should feel asymmetric: fast acceleration, slower, gentler
  deceleration (ease-out) is the default "natural" feel for most UI motion.
- Elements entering the screen at full velocity and decelerating to rest
  (their "deceleration curve") suits things appearing; a "sharp" curve
  (fast in, fast out) suits things that might reverse/exit at any moment.

**Apple HIG:**
- Prefer quick, precise motion over long/elaborate motion — brevity reads
  as more premium, not less.
- Strive for physical plausibility — motion that seems to defy expected
  physics is disorienting, not delightful.
- Motion must be optional — `Reduce Motion` accessibility settings must
  actually reduce or remove non-essential animation (see
  `standards-and-mistakes.md`'s accessibility section).

## Game feel and "juice"

From game-feel literature (notably the "Juice It or Lose It" GDC Europe
2012 talk by Martin Jonasson and Petri Purho, and the broader "game feel"
literature it sits in): **juice** is the layering of abundant, redundant
audiovisual feedback onto an interactive moment — screen shake, particle
bursts, scale pulses, sound, color flash — all firing together for the same
event. The core finding isn't "add more effects," it's that **feedback
redundancy strengthens the felt connection between input and result.** A
button that only changes color on click feels less responsive than one
that also scales down 5%, even though both registered the click instantly
— the extra channel of feedback is what makes it *felt*, not just correct.

The caveat (also documented, and worth taking seriously): juice can be
overdone to the point of undermining immersion or slowing down repeated
interactions. The practical rule that emerges from both sides of that
debate: **juice the moments that matter (rewards, purchases, level-ups),
keep juice minimal-but-present on high-frequency interactions (hover,
generic buttons)** so the frequent stuff stays fast and the special stuff
stays special. This directly informs the standards table in
`standards-and-mistakes.md` — hover/click intensity is capped low, reward
moments are allowed to be more elaborate specifically because they're rare.

## Psychological impact (why polish reads as "premium")

- **Dopamine fires on anticipation, not receipt.** Neuroscience research on
  reward processing shows the dopamine response peaks *during the buildup*
  to a reward, not at the moment of delivery. This is the direct
  justification for a brief "drumroll" beat before a reward reveal (a
  pause, a shake, a glow building) rather than instantly showing the
  result — the anticipation window is where the payoff is actually felt.
- **Variable-reward schedules sustain engagement more than fixed ones** —
  well documented in compulsion-loop / operant-conditioning game design
  literature. This is a design-ethics-adjacent topic: it explains *why*
  loot/gacha reveals are paced the way they are, without this skill
  endorsing manipulative pacing — use the anticipation principle for
  legitimate reward clarity, not to manufacture compulsive engagement.
- **Consistent, responsive motion reads as trustworthy/high-quality.**
  Conversely, inconsistent timing/easing across an interface (see
  `standards-and-mistakes.md`) reads as unpolished even when no single
  animation is individually "wrong" — consistency itself is a quality
  signal, independent of any one animation's design.

## Sources
- [UI Animation — Disney's 12 Principles applied to UI (IxDF)](https://ixdf.org/literature/article/ui-animation-how-to-apply-disney-s-12-principles-of-animation-to-ui-design)
- [Disney's 12 principles in UX design (UX Collective)](https://uxdesign.cc/disneys-12-principles-of-animation-exemplified-in-ux-design-5cc7e3dc3f75)
- [Material Design 3 — Easing and duration](https://m3.material.io/styles/motion/easing-and-duration)
- [Material Design — Understanding motion](https://m2.material.io/design/motion/understanding-motion.html)
- [Apple HIG — Motion](https://developer.apple.com/design/human-interface-guidelines/motion)
- [GDC Vault — Juice It or Lose It](https://www.gdcvault.com/play/1016487/Juice-It-or-Lose)
- [Game feel — Wikipedia overview](https://en.wikipedia.org/wiki/Game_feel)
- [The Psychology of Reward Systems in games](https://www.dodefy.com/articles-posts/the-psychology-of-reward-systems-why-games-keep-us-hooked)
- [Compulsion loop — Wikipedia](https://en.wikipedia.org/wiki/Compulsion_loop)

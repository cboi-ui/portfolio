# Portfolio Website Design System

This describes the visual system of the **portfolio website itself** (not the
Roblox UI design work shown inside it). Keep the site's actual CSS in sync
with this doc when the look changes.

## Overall Direction

The website should feel like a premium Roblox UI designer portfolio — visually
quiet and professional, letting the portfolio screenshots be the main focus.

Should communicate: professional UI designer, modern digital product,
Roblox/game-development focused, high-quality freelance service.

---

## Color System

**Background:** dark charcoal/navy canvas.
Purpose: avoid pure black, create a premium atmosphere, let colorful UI
screenshots stand out.
Avoid: flat black, excessive gradients, loud background effects.

**Accent:** electric violet → cyan gradient.
Usage: CTA buttons, important links, hover states, small glow effects,
decorative highlights.
Do NOT use as the main background, on every component, or with excessive
gradients everywhere — it should feel special, not everywhere.

---

## Portfolio Presentation

Screenshots are the main visual element; the site should never compete with
project images. Portfolio cards include: screenshot preview, project title,
short description, category/tags, hover interaction.

---

## Typography

- **Headings:** rounded geometric modern font (e.g. Space Grotesk). Used for
  hero titles, section headers, important text.
- **Body:** clean modern sans-serif, prioritizing readability, professional
  appearance, clear hierarchy.

---

## Components

**Cards:** dark panels, soft borders, subtle glow, rounded corners — inspired
by modern game UI frames. Avoid excessive glassmorphism, heavy shadows,
over-decoration.

---

## Animation

Polished, not distracting.
- Scroll: fade in + slight upward movement, smooth transitions.
- Hover: small lift, soft accent glow, border highlight.
- Avoid fast animations, excessive movement, distracting effects.

### Interactive background (implemented)

- The ambient glow (`#bgGlow`) drifts a few pixels with cursor position
  (lerp-smoothed, not 1:1 tracking) so the page feels alive without being
  distracting.
- Clicking anywhere spawns a small ripple + spark burst at the cursor
  (`#fx-layer`), purely decorative (`pointer-events:none`).
- **Keep this subtle.** First pass was too intense (large ripple, bright
  opaque sparks, 5 sparks flying ~30-48px) and the user asked for it to be
  toned down. Current values: ripple scales to 4x at ~55% peak opacity,
  4 sparks traveling 14-26px at 0.55 opacity. If asked to add more motion
  here, default to smaller/dimmer, not bigger.
- Both respect `prefers-reduced-motion`: parallax is skipped entirely, and
  the click effect falls back to a plain opacity fade with no scale/travel.

### Shine

- A hover-triggered diagonal shine sweep (skewed light streak, low-opacity
  white gradient) on the two most important interactive surfaces only:
  portfolio cards and the primary CTA button. Deliberately not applied
  everywhere, to keep it feeling special rather than gimmicky.
- A static, very subtle top-edge gloss (`inset 0 1px 0 rgba(255,255,255,0.05)`)
  on all panel-style surfaces (cards, service cards, price cards, info panel,
  contact panel) for a faint beveled/game-UI-frame feel at rest.

---

## Website Personality

**Yes:** premium, modern, creative, game-focused, professional.
**No:** generic developer portfolio, corporate website, overly flashy gaming
website.

---

## Trust Layer

Roblox devs aren't just buying visuals — they're buying confidence the work
will actually get finished. Build trust with:

- Hero subline stating specialty plainly (e.g. "Roblox UI Designer — creating
  polished interfaces for simulators, games, and experiences").
- A small stats row (e.g. years designing, projects completed) — **only with
  real numbers**, see the rule below.
- Commission section stating current availability, typical turnaround, and
  payment methods accepted.

### Critical rule: no fabricated credibility

Do not invent portfolio projects, client names, testimonials, project counts,
or experience numbers. If a real number isn't provided, either omit that
element entirely or use an explicit placeholder and flag it for the user —
never guess a plausible-sounding stat. AI-generated portfolios accidentally
manufacturing fake credibility is a real failure mode; this site must stay
truthful. See [[CLAUDE.md]].

---

## Design Principle

The website is the frame. The UI artwork is the focus. The site should make a
client think: "I want this person designing my game's UI."

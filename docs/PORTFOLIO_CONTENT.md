# Portfolio Content

Source images live in `TemplateImages/<Style>/`. Each portfolio entry below is
a grouping shown as one project on the site, not a raw per-image dump.

---

## 1. Star Shop & Popup Suite

**Category:** Cartoon Simulator UI

**Images:**
- `TemplateImages/CartoonStyle/template1.png` — "Star Shop" item shop panel
  with limited-time offers, countdown timers, and Robux buy buttons.
- `TemplateImages/CartoonStyle/Template2.png` — matching popup set: purchase
  confirmation, friends-benefits invite, gift-a-friend, and rebirth
  confirmation.
- `TemplateImages/CartoonStyle/Template3.png` — settings menu in the same
  cartoon panel language (Graphics/Audio toggles, accessibility options).

**Description:** A cohesive cartoon-style UI kit for a simulator game —
bright, high-energy, built to make impulse purchases feel fun rather than
transactional. Consistent rounded-panel language and color-coded actions
(green = confirm, red = danger/purchase, blue = neutral/gift) carried across
every popup and the settings menu so players never have to relearn the
pattern.

**Skills demonstrated:** UI kit consistency across multiple screens, color
psychology for calls-to-action, monetization-friendly shop layout, countdown/
urgency UI patterns.

---

## 2. Daily Rewards Claim

**Category:** Stud-Style UI

**Images:**
- `TemplateImages/StudStyle/Template1.png` — 5-slot daily login rewards grid
  with a "limited" bonus slot.

**Description:** A pastel, blocky UI matched to a voxel-art game world.
Reward tiers are scannable at a glance, and the limited-time slot is called
out visually to drive daily-return habits without needing extra copy.

**Skills demonstrated:** Retention-pattern UI (daily rewards), matching UI
style to a specific art direction, iconography-first layout.

---

## 3. Skull Shop & Inventory

**Category:** Stud-Style UI

**Images:**
- `TemplateImages/StudStyle/Template2.png` — pirate/skull-themed shop panel
  with buy and sell actions on the same row per item (Locator Ping, Crazy
  Bomb, more below the fold).
- `TemplateImages/StudStyle/Template3.png` — matching inventory panel
  tracking multiple collections (Gifts, Books) each with their own "X out of
  10" progress count.

**Description:** A second stud-style UI pair for the same game world as
Daily Rewards Claim, built around a pirate/skull theme. The shop puts buy
and sell on one row per item instead of splitting them across separate
screens, and the inventory scales to multiple collections at once by giving
each its own labeled row and progress count rather than one big undivided
grid.

**Skills demonstrated:** Dual-action (buy/sell) list UI, multi-collection
inventory layout, theme consistency across a shop and inventory pair.

---

## Adding a new piece

1. Drop screenshots into a `TemplateImages/<Style>/` folder (new or existing).
2. Add a numbered section here following the template above.
3. Wire it into the site's portfolio gallery.
4. Log it in `CHANGELOG.md`.

## Note on removed pieces (2026-07-18)

Frost Hunter Inventory and Fruits Collection Dex (both Clean Minimalist UI)
were pulled from the portfolio at the user's request during the CBoi ->
Nabi rebrand, rather than re-watermarked. Their source images are still
sitting untouched in `TemplateImages/MinimalistStyle/` if they're ever
wanted back; nothing else in the site or docs currently advertises a
Minimalist style option.

## Note on file naming (2026-07-14)

Files in `TemplateImages/` got renamed/reorganized mid-project (case and
numbering normalized to `Template1.png`, `Template2.png`, ...). If a described
image doesn't match what's actually in the folder, trust the folder contents
and update this file, don't assume the doc is right.

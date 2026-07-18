# Portfolio Content

Source images live in `TemplateImages/<Style>/`. As of 2026-07-18 each
portfolio entry below is shown as its own card on the site (previously some
were grouped into one multi-image carousel card; switched to one card per
image so more work is visible at once instead of hidden behind
click-through dots).

---

## 1. Star Shop

**Category:** Cartoon Simulator UI

**Image:** `TemplateImages/CartoonStyle/template1.png` — "Star Shop" item
shop panel with limited-time offers, countdown timers, and Robux buy
buttons.

**Description:** A high-energy cartoon-style item shop for a simulator
game, built to make spending Robux feel exciting instead of transactional.

**Skills demonstrated:** Monetization UX, countdown/urgency UI patterns,
cartoon panel language.

---

## 2. Purchase & Rebirth Popups

**Category:** Cartoon Simulator UI

**Image:** `TemplateImages/CartoonStyle/Template2.png` — popup set:
purchase confirmation, friends-benefits invite, gift-a-friend, and rebirth
confirmation.

**Description:** A matching popup set for the Star Shop, sharing
color-coded actions (green = confirm, red = danger/purchase, blue =
neutral/gift) so players never have to relearn the pattern across screens.

**Skills demonstrated:** Color-coded actions, UI kit consistency,
confirmation-flow design.

---

## 3. Settings Menu

**Category:** Cartoon Simulator UI

**Image:** `TemplateImages/CartoonStyle/Template3.png` — settings menu in
the same cartoon panel language (Graphics/Audio toggles, accessibility
options).

**Description:** A settings menu carrying the same rounded-panel language
as the Star Shop and popups, covering graphics, audio, and accessibility
toggles (Flashing Lights, Show Blood).

**Skills demonstrated:** Toggle/switch UI, accessibility options, UI kit
consistency.

---

## 4. Daily Rewards Claim

**Category:** Stud-Style UI

**Image:** `TemplateImages/StudStyle/Template1.png` — 5-slot daily login
rewards grid with a "limited" bonus slot.

**Description:** A pastel, blocky UI matched to a voxel-art game world.
Reward tiers are scannable at a glance, and the limited-time slot is called
out visually to drive daily-return habits without needing extra copy.

**Skills demonstrated:** Retention-pattern UI (daily rewards), matching UI
style to a specific art direction, iconography-first layout.

---

## 5. Skull Shop

**Category:** Stud-Style UI

**Image:** `TemplateImages/StudStyle/Template2.png` — pirate/skull-themed
shop panel with buy and sell actions on the same row per item (Locator
Ping, Crazy Bomb, more below the fold).

**Description:** A pirate/skull-themed shop for the same stud-style world
as Daily Rewards Claim, putting buy and sell on one row per item instead of
splitting them across separate screens.

**Skills demonstrated:** Buy/sell UI, theme-consistent kit, list-based shop
layout.

---

## 6. Collection Inventory

**Category:** Stud-Style UI

**Image:** `TemplateImages/StudStyle/Template3.png` — inventory panel
tracking multiple collections (Gifts, Books) each with their own "X out of
10" progress count.

**Description:** A matching inventory panel for the same world as Skull
Shop, tracking multiple collections at once by giving each its own labeled
row and progress count instead of one big undivided grid.

**Skills demonstrated:** Multi-collection layout, progress tracking,
theme-consistent kit.

---

## Adding a new piece

1. Drop screenshots into a `TemplateImages/<Style>/` folder (new or existing).
2. Add a numbered section here following the template above (one entry per
   image/card, unless a set is genuinely meant to share a single carousel
   card).
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

---
name: ui-prep
description: Prepare a Roblox ScreenGui for future animation by centering AnchorPoints (with mathematically compensated Position), and converting Offset to Scale where safe — without changing anything visually or breaking existing scripts. Use when asked to make a GUI "animation-ready," fix anchor points, or prep a shop/popup/button for scaling/tweening.
user-invocable: true
---

# ui-prep: Roblox UI animation preparation

Standardizes a ScreenGui's AnchorPoints, Position, and Size so it can be
scale/tween-animated later, while leaving the current visual result and all
script references untouched. This is a **modifying** skill — pair it with
`ui-arch` (read-only audit) when the user just wants a report first.

## Non-negotiables

- Pixel-identical before/after. If a change can't be verified to not move
  something, don't make it.
- Never rename, delete, recreate, or reparent an Instance. Scripts elsewhere
  reference these by name/path — only touch properties.
- Don't convert every Offset to Scale reflexively. Borders, stroke
  thickness, corner radius, small fixed-size icons, and padding are usually
  *supposed* to stay pixel-fixed — converting them changes how the UI reads
  at other resolutions, which is not what "animation-ready" means. Convert
  Offset→Scale only for Position/Size that should visually scale with its
  container (e.g. a button sized in fixed px inside a parent that itself
  scales with screen size — that mismatch is a real responsiveness bug,
  not a style choice).

## Workflow

1. **Connect to Studio.** `list_roblox_studios` → if more than one instance,
   confirm which is the target with the user (this project defaults to
   "UI Design" per CLAUDE.md, but ui-prep is reusable across projects —
   don't hardcode that assumption elsewhere).
2. **Locate and map the target ScreenGui.** `search_game_tree` for the full
   hierarchy, then `inspect_instance` on every GuiObject to record current
   `AnchorPoint`, `Position`, `Size`, and any `UIAspectRatioConstraint`/
   `UIListLayout`/`UIGridLayout`/`UIPadding` on it or its parent. Do this
   for the *entire* subtree before changing anything — a button's correct
   math depends on knowing whether a layout object is driving its position
   (if `UIListLayout` controls Position, don't hand-edit Position at all;
   note it as a layout-managed element instead).
3. **Screen-capture the current state** (`screen_capture`) as your
   before-reference.
4. **Classify every GuiObject as animation-relevant or not.** Buttons
   (`ImageButton`/`TextButton`, or a `Frame`/`ImageLabel` wired to click
   detection via a script), popups/modals/major windows, and anything the
   user names explicitly are in scope. Purely decorative children
   (gradients, strokes, corner radius, background art) are not touched
   directly — they inherit correctness from their parent.
5. **For each in-scope object with `AnchorPoint ~= (0.5, 0.5)`**, apply the
   recentering formula below, then verify.
6. **For each in-scope object whose Size/Position mixes Offset and Scale in
   a way that would break proportionality** (see the non-negotiables
   above), evaluate case by case — don't batch-convert.
7. **Re-verify against the before-screenshot** (`screen_capture` again) and
   spot-check `AbsolutePosition`/`AbsoluteSize` deltas are ~0.
8. **Report** using the Deliverables format below.

## The recentering formula

Changing `AnchorPoint` moves *what point Position refers to*, not the
element itself. Left uncompensated, the element visually jumps. Shift
Position by `Size * (newAnchor - oldAnchor)` on each axis, keeping Scale
and Offset components separate — this works for *any* starting anchor, not
just the default `(0,0)`:

```lua
local function recenterAnchor(obj, newAnchor)
    local oldAnchor = obj.AnchorPoint
    local oldPos, size = obj.Position, obj.Size
    local dx, dy = newAnchor.X - oldAnchor.X, newAnchor.Y - oldAnchor.Y

    local newPos = UDim2.new(
        oldPos.X.Scale  + size.X.Scale  * dx,
        oldPos.X.Offset + size.X.Offset * dx,
        oldPos.Y.Scale  + size.Y.Scale  * dy,
        oldPos.Y.Offset + size.Y.Offset * dy
    )

    obj.AnchorPoint = newAnchor
    obj.Position = newPos
end
```

This is exact and requires no live pixel lookups — it only uses the
object's own `Position`/`Size` UDim2 components. Use it as the primary
computation.

**Then verify empirically**, since it catches anything the symbolic formula
can't see (a `UIListLayout` silently overriding Position, a constraint
resizing the object after the fact):

```lua
local before = {pos = obj.AbsolutePosition, size = obj.AbsoluteSize}
-- apply recenterAnchor(obj, Vector2.new(0.5, 0.5))
local after = {pos = obj.AbsolutePosition, size = obj.AbsoluteSize}
-- assert |before.pos - after.pos| < ~0.6px and |before.size - after.size| < ~0.6px
```

If the assertion fails, something else (a layout object, a constraint, an
`AutomaticSize`) is driving the property — stop and investigate that
object rather than forcing Position again.

## Offset → Scale conversion (only when justified)

If a button/panel's Size is fixed-pixel Offset while its parent resizes
with the screen (parent Size has a Scale component), the child will not
stay proportional — that's the failure mode worth fixing. Convert by
reading the parent's live `AbsoluteSize` and re-expressing the child's
*current* pixel size as a fraction of it:

```lua
local parentAbsSize = obj.Parent.AbsoluteSize
local newSizeScaleX = obj.AbsoluteSize.X / parentAbsSize.X
local newSizeScaleY = obj.AbsoluteSize.Y / parentAbsSize.Y
obj.Size = UDim2.new(newSizeScaleX, 0, newSizeScaleY, 0)
```

This preserves the current visual size exactly (same AbsoluteSize at the
moment of conversion) while making it scale from here on. Skip this for
anything in the non-negotiables list (strokes, corner radius, fixed icons,
padding).

## Deliverables

Report back with:

**Changes Made**
- AnchorPoints updated (object path → old anchor → new anchor).
- Position values recalculated (count + confirmation all passed the
  AbsolutePosition/AbsoluteSize verification).
- Any Offset→Scale conversions, with the specific responsiveness bug each
  one fixed.

**Warnings**
- Any object that couldn't be safely standardized and why (e.g. "Position
  driven by UIListLayout, left as-is").

**Recommendations**
- Follow-on improvements worth doing later, not done automatically.

Don't claim "pixel-identical" unless you actually ran the verification step
and it passed — a screenshot diff or an AbsolutePosition assertion, not a
visual guess.

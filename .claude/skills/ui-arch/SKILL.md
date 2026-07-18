---
name: ui-arch
description: Read-only audit of a Roblox ScreenGui's architecture, naming, responsiveness, animation-readiness, and maintainability — produces a scored report with prioritized recommendations. Never modifies the UI. Use when asked to audit, review, or assess a GUI's structure/quality, or "how healthy is this UI" style questions.
user-invocable: true
---

# ui-arch: Roblox UI architecture auditor

Audits a ScreenGui and reports findings — hierarchy quality, naming,
animation-readiness, responsiveness, layering, performance, reusability,
expandability, script-compatibility risk. **Read-only**: inspect and
report, never edit, rename, move, or delete Instances, and never change
AnchorPoint/Position/Size, unless the user explicitly asks for changes in
the same request (in which case switch to `ui-prep` for the modification
part — don't blend the two).

## Workflow

1. **Connect to Studio**, confirm target ScreenGui with the user if
   ambiguous (`list_roblox_studios`, don't assume which Studio instance or
   which GUI without checking — see project CLAUDE.md for any
   project-specific default).
2. **Inspect the full hierarchy recursively** (`search_game_tree` then
   `inspect_instance` on each node) before writing any conclusions. Note
   for each GuiObject: name, class, AnchorPoint, Position, Size, ZIndex,
   and any UILayout/UIConstraint children.
3. **Identify every interactive element** (`GuiButton` instances, plus
   anything wired to click detection via a LocalScript even if it's a
   plain Frame/ImageLabel).
4. **Screen-capture** the current state for the report and for spotting
   overlap/clipping visually.
5. Run each analysis pass below. Don't skip to the report until the whole
   subtree has been walked.
6. Produce the Audit Report in the exact format below.

Never assume what something does from its name alone (a node named
`CloseButton` might not actually close anything — check for a connected
script/event before asserting its role).

## Analysis passes

**Hierarchy** — excessive nesting, redundant/empty Frames, unnecessary
wrapper containers, containers with too many direct children, whether
another developer could navigate this cold.

**Naming** — flag generic/auto-generated names (`Frame1`, `TextLabel`,
`NewButton`) vs. descriptive ones (`ShopTabs`, `CloseButton`,
`InventoryGrid`). Note inconsistent conventions (mixed casing, mixed
term choices for the same concept).

**Animation readiness** — AnchorPoints on buttons/popups/major windows
(centered vs. corner), whether Position was ever compensated for an
off-center anchor (jump risk), presence/absence of a dedicated animation
wrapper Frame where one would help, `UIScale` usage, clipping risk from
`ClipsDescendants` on a container something will scale out of.

**Responsiveness** — Scale vs. Offset usage on Position/Size,
`UIAspectRatioConstraint`/`UISizeConstraint` presence where distortion risk
exists, `UIPadding`/`UIGridLayout`/`UIListLayout` configuration, text
scaling (`TextScaled`/`UITextSizeConstraint`). Reason about phone, tablet,
1080p, and ultrawide aspect ratios specifically, not just "should be fine."

**Layout consistency** — spacing/margin/padding/alignment consistency
across sibling components (do all shop item rows use the same padding? do
all popups center the same way?).

**Styling consistency** — fonts, text sizes/weights, corner radius, stroke
thickness, color/gradient/transparency patterns. Flag inconsistency, not
intentional artistic variation — don't recommend "fixing" a deliberately
different accent color without evidence it's accidental.

**Layering** — `ZIndex`/`DisplayOrder` conflicts, modal/popup/overlay
stacking order, anything that would render behind what it should cover.

**Performance** — duplicate `UIStroke`/`UICorner`/`UIGradient` siblings
that could be one, hidden-but-still-live UI, excessive transparency
layering, deep nesting that costs layout-recalculation performance.

**Reusability** — duplicated structures (e.g. 5 hand-built shop item Frames
with identical structure) that could be a single template + instantiation
pattern instead.

**Expandability** — can a new tab/category/reward/currency be added without
restructuring? Where's the bottleneck if so?

**Script compatibility risk** — how likely is existing script code to
reference these Instances by path/name in a way that would break if the
hierarchy were reorganized? Flag before recommending any structural
change.

**Maintainability** — overall: would a new developer understand this
quickly?

## Audit Report format

```
## Overall Score
Architecture: X/10
Animation Readiness: X/10
Responsiveness: X/10
Maintainability: X/10
Performance: X/10
Expandability: X/10
Consistency: X/10
Overall: X/10

## Strengths
- ...

## High Priority Issues
- Issue — why it matters

## Medium Priority Improvements
- ...

## Low Priority Suggestions
- ...

## Future Risks
- ...

## Recommendations
- Issue / Why it matters / Proposed solution / Expected benefit
```

Score against what's actually there, not against a hypothetical ideal —
a small, simple GUI that does its one job cleanly can legitimately score
well. Don't manufacture issues to fill every section; an empty "Low
Priority Suggestions" list is a valid outcome.

## Hard rule

If, mid-audit, something looks broken enough that it's tempting to just
fix it — don't. Note it as a High Priority Issue and ask the user whether
to hand it to `ui-prep` (or make the edit directly) instead of doing it
inline. This skill's output is a report, never a diff.

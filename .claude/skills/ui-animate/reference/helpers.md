# Reusable animation helpers

Drop this near the top of a new panel's animation `LocalScript`. It's the
exact pattern used and verified (full open/close cycle, no console errors)
in the `SkullShopGui` implementation.

```lua
local TweenService = game:GetService("TweenService")

-- Keyed by instance so a rapid re-hover/re-press cancels the previous
-- in-flight tween on that same property instead of letting two race.
local activeTweens: {[Instance]: Tween} = {}
local function springTo(instance: Instance, info: TweenInfo, goals: {[string]: any}): Tween
	local prior = activeTweens[instance]
	if prior then
		prior:Cancel()
	end
	local tween = TweenService:Create(instance, info, goals)
	activeTweens[instance] = tween
	tween:Play()
	return tween
end

local function getOrCreateUIScale(guiObject: GuiObject): UIScale
	local existing = guiObject:FindFirstChildOfClass("UIScale")
	if existing then
		return existing
	end
	local uiScale = Instance.new("UIScale")
	uiScale.Scale = 1
	uiScale.Parent = guiObject
	return uiScale
end

local function isPrimaryInput(input: InputObject): boolean
	return input.UserInputType == Enum.UserInputType.MouseButton1
		or input.UserInputType == Enum.UserInputType.Touch
end

-- Generic hover-grow + press-punch wiring. Covers the large majority of
-- buttons — call this once per button, then layer anything bespoke
-- (a rotation wag, a coupled sibling move) on top separately.
local function wireButtonFeedback(button: GuiButton, growScale: number)
	button.AutoButtonColor = false
	local uiScale = getOrCreateUIScale(button)
	local isHovering = false

	button.MouseEnter:Connect(function()
		isHovering = true
		-- Elastic/Out reads as genuinely bouncy (inspired by Twinkle's
		-- Presets/Hover/Bounce.lua) -- swap to Back/0.18s for a calmer feel.
		springTo(uiScale, TweenInfo.new(0.45, Enum.EasingStyle.Elastic, Enum.EasingDirection.Out), {Scale = growScale})
	end)
	button.MouseLeave:Connect(function()
		isHovering = false
		springTo(uiScale, TweenInfo.new(0.18, Enum.EasingStyle.Back, Enum.EasingDirection.Out), {Scale = 1})
	end)
	button.InputBegan:Connect(function(input)
		if isPrimaryInput(input) then
			springTo(uiScale, TweenInfo.new(0.09, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {Scale = 0.9})
		end
	end)
	button.InputEnded:Connect(function(input)
		if isPrimaryInput(input) then
			local target = isHovering and growScale or 1
			springTo(uiScale, TweenInfo.new(0.18, Enum.EasingStyle.Back, Enum.EasingDirection.Out), {Scale = target})
		end
	end)
end
```

## Motion-token table (defaults used above, tune per-panel if needed)

| Token | Duration | Style/Direction | Used for |
|---|---|---|---|
| HoverIn | 0.4-0.45s | Elastic, Out | Bouncy hover grow (0.18-0.2s Back/Out for a calmer feel) |
| HoverOut | 0.18-0.2s | Back, Out | Hover shrink-back — stays calm even if HoverIn is Elastic |
| CardHover | 0.2s | Back, Out | Whole-card grow — use ~2% for wide/short cards, not the button-sized 8-12% (see aspect-ratio note in SKILL.md) |
| PressDown | 0.09-0.11s | Quad, Out | Press squash |
| PressUp | 0.18-0.2s | Back, Out | Press release pop |
| Wag | 0.12s | Quad, Out | One-shot rotation flourish |
| PanelOpenScale | 0.4-0.45s | Back, Out | Panel entrance scale |
| PanelOpenFade | 0.25-0.3s | Quad/Cubic, Out | Panel entrance fade (faster than scale) |
| PanelClose | 0.2s | Cubic/Quad, In | Panel exit — scale + fade together, no bounce |
| Decor | 0.22-0.26s | Quad, Out | Header decoration fade-in |
| DecorPop | 0.28-0.3s | Back, Out | Header decoration small pop/slide, close button pop-in |
| ItemSlide | 0.3-0.35s | Back, Out | List item slide-in-from-left |

Item stagger interval: ~0.08s apart. Decoration cascade delay after panel
starts: ~0.1-0.15s, then ~0.05s between each subsequent decoration.

## State-management pattern (`isOpen`/`isAnimating` guards)

```lua
local isOpen = false
local isAnimating = false

local function openPanel()
	if isAnimating or isOpen then return end
	isAnimating = true
	isOpen = true
	-- ... fire tweens, task.delay(totalDuration, function() isAnimating = false end)
end

local function closePanel()
	if isAnimating or not isOpen then return end
	isAnimating = true
	isOpen = false
	-- ... fire tweens, task.delay(closeDuration, function()
	--     resetToClosedState() -- see SKILL.md's pitfall section
	--     isAnimating = false
	-- end)
end
```

Write `resetToClosedState()` (or equivalent) **once, after every hidden
element's `UIScale`/rest-value has already been created/captured** —
writing it earlier is exactly how the reset-state pitfall in SKILL.md
happens.

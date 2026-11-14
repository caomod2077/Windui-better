# WindUI-Better

[English](README.md) | [Tieng Viet](README_VI.md)

An enhanced edition of WindUI for Roblox with support for two-column card layouts, in-tab search filtering, nested sub-tabs with per-subtab column customization, player profile card on the sidebar, built-in gradient themes (including Liquid Glass and Ocean Blue), universal dynamic auto-translation supporting any language, watermark stats, tab access control, interactive 3D viewports, and performance optimizations.

Original library by Footagesus. Maintained and expanded by caomod2077.

---

## Performance Optimizations

Standard WindUI builds can experience frame drops and memory accumulation in complex interfaces. WindUI-Better introduces the following engine-level optimizations:

1. Automatic Dead Object Pruning: Theme objects and signals bound to destroyed UI elements are automatically unregistered instead of leaking in global tables.
2. Memoized Shape Scaling: Squircle and Glass vector calculations only execute when dimensions actually change, preventing thousands of redundant math operations per frame during animations.
3. Zero-Allocation Watermark: Frame rate calculation replaces array mutations with lightweight timestamp accumulators, eliminating per-frame memory allocation and garbage collection churn.
4. Visibility-Gated Render Loops: Icon rotation, rainbow animations, and watermark updates immediately pause when the window is closed or hidden, saving CPU cycles for the game engine.
5. Lifecycle-Bounded Component Loops: Background loops in Countdown and animated elements automatically disconnect when their parent element is destroyed.
6. Non-Blocking Translation Engine: Zero hardcoded dictionaries in library code. Background translation requests are deduplicated and cached with no HTTP traffic on startup.

---

## Quick Start

```lua
local WindUI = loadstring(game:HttpGet("https://raw.githubusercontent.com/caomod2077/Windui-better/main/windui.lua"))()

local Window = WindUI:CreateWindow({
    Title = "My Script Hub",
    Author = "by Caomod2077",
    Folder = "MyScriptHub",
    Theme = "Dark",
    Size = UDim2.fromOffset(840, 540),
    TwoColumns = true,       -- Enable 2-column card layout (default: false)
    BackgroundBlur = false,   -- Background blur effect (default: false)
    Glow = Color3.fromRGB(0, 145, 255),
    IconAnimation = "Spin",  -- "Spin" | "Pulse" | "Rainbow"
    SidebarBanner = {
        Image = "rbxassetid://133610205520685",
        Height = 75,
        Title = "Hub Version",
    },
    User = {
        Enabled = true,               -- Display player profile card on sidebar bottom
        Anonymous = false,            -- Hide player name/avatar when true
        Subtitle = "Developer",        -- Subtitle or status text
        Profile = { Enabled = true, Custom = true, Title = "Local Player" }, -- Custom mode for player utilities
        Callback = function() end,    -- Click callback
    },
})
```

Background blur can also be toggled at runtime:

```lua
Window:SetBackgroundBlur(true)
Window:SetBackgroundBlur(false)
```

---

## Complete Feature & API Reference

### 1. Window Creation Options (WindUI:CreateWindow)

| Option | Type | Default | Description |
|---|---|---|---|
| `Title` | string | `"UI Library"` | Main title text on the topbar |
| `Author` | string | `nil` | Subtitle or author credit next to title |
| `Folder` | string | `nil` | Directory name under `WindUI/` for config files |
| `Theme` | string | `"Dark"` | Active theme name from built-in or custom themes |
| `Size` | UDim2 | `UDim2.fromOffset(720, 470)` | Initial window size |
| `MinSize` | Vector2 | `Vector2.new(460, 320)` | Minimum resize dimensions |
| `MaxSize` | Vector2 | `Vector2.new(2560, 1600)` | Maximum resize dimensions |
| `TwoColumns` | boolean | `false` | Automatically alternates sections into two columns |
| `BackgroundBlur` | boolean | `false` | Enables full-screen lighting blur when open |
| `Glow` | Color3 | `nil` | Outer glow neon border color |
| `Icon` | string | `nil` | Topbar icon (rbxassetid or Lucide icon name) |
| `IconSize` | number | `22` | Pixel size of the topbar icon |
| `IconAnimation` | string | `nil` | Animation mode: `"Spin"`, `"Pulse"`, or `"Rainbow"` |
| `SidebarBanner` | table | `nil` | Header banner above tabs: `{ Image, Height, Title }` |
| `SideBarWidth` | number | `175` | Width of the left navigation sidebar |
| `HideSearchBar` | boolean | `false` | Hides the search bar on top of the tab list |
| `ScrollBarEnabled` | boolean | `false` | Enables visual custom scroll sliders |
| `Resizable` | boolean | `true` | Allows dragging the bottom-right corner to resize |
| `Transparent` | boolean | `false` | Window body transparency |
| `Acrylic` | boolean | `false` | Enables frosted acrylic glass blur effect |
| `AutoScale` | boolean | `true` | Automatically scales interface for mobile / small displays |
| `ToggleKey` | Enum.KeyCode | `nil` | Keycode to hide or reveal the window |
| `User` | table | `nil` | Player profile card on bottom of sidebar: `{ Enabled, Anonymous, Subtitle, Callback, Profile }` |
| `Topbar` | table | `{ Height = 52 }` | Topbar options: `{ Height = 52, ButtonsType = "Default" / "Mac" }` |
| `Background` | string | `nil` | Custom background image (`rbxassetid://`, `https://`) or video (`video:https://...webm`) |
| `BackgroundImageTransparency` | number | `0` | Opacity of the background image (0 to 1) |
| `KeySystem` | table | `nil` | Built-in key gate system before opening window |

---

### 2. Window Methods

- `Window:Tag({ Title, Color, Radius, Border })`: Adds a pill tag to the topbar.
- `Window:Watermark({ Title, FPS = true, Ping = true })`: Creates topbar stats badge. Returns `{ SetTitle(text), SetVisible(bool), Destroy() }`.
- `Window:SetBackgroundBlur(boolean)`: Toggles lighting blur at runtime.
- `Window:ToggleSidebar(forceState)`: Toggles or sets collapsed icon mode (52px) vs expanded mode (175px).
- `Window:SetTheme(name)`: Changes the active theme on the window.
- `Window:SetColor({ Accent, Background, Text, Dialog, ... })`: Overrides theme colors at runtime.
- `Window:EnableSounds(boolean, soundConfig)`: Toggles audio feedback with optional custom sound IDs and volume.
- `Window:SetSounds({ Tab, Toggle, Notification, Volume })`: Customizes individual sound IDs (`rbxassetid://...`) and volume.
- `Window:PlaySound(soundType)`: Plays sound (`"tab"` or `"toggle"`).
- `Window:SetUIScale(number)`: Adjusts entire UI scale factor (e.g. `0.85` for mobile).
- `Window:SetToggleKey(Enum.KeyCode)`: Updates keyboard toggle shortcut.
- `Window:SetSize(UDim2)`: Resizes window programmatically.
- `Window:SetToTheCenter()`: Centers window on screen.
- `Window:SetBackgroundImage(string)`: Updates window background (assetid or URL).
- `Window:SetBackgroundImageTransparency(number)`: Adjusts background image opacity (0 to 1).
- `Window:SetBackgroundTransparency(number)`: Adjusts window frame transparency.
- `Window:ToggleTransparency(boolean)`: Toggles window transparency on or off.
- `Window:ToggleFullscreen()`: Toggles fullscreen window sizing.
- `Window:LockAll()`, `Window:UnlockAll()`: Mass lock or unlock all controls in the entire window at once.
- `Window:GetLocked()`, `Window:GetUnlocked()`: Returns list of locked or unlocked elements.
- `Window:SetTitle(string)`, `Window:SetAuthor(string)`: Updates window title or author at runtime.
- `Window:SetIconSize(number)`: Changes topbar icon size.
- `Window:SetIconAnimation("Spin" | "Pulse" | "Rainbow")`: Changes or starts topbar icon animation.
- `Window:AnimateTitle("Gradient" | "Shimmer")`: Starts moving gradient animation on window title.
- `Window:SetSidebarBanner({ Image, Height, Title })`: Updates or creates sidebar banner.
- `Window:CreateTopbarButton(name, icon, callback, order, isRight, color, iconSize)`: Adds custom icon buttons to the topbar.
- `Window:DisableTopbarButtons(boolean)`: Disables topbar control buttons.
- `Window:Toast({ Title, Content, Duration, Icon })`: Quick notification shortcut.
- `Window:Dialog({ Title, Content, Buttons })`: Opens modal popup that can host interactive child elements.
- `Window:EditOpenButton(table)`: Customizes floating toggle button for mobile/touch devices.
- `Window:Section({ Title, Icon, Opened })`: Creates a collapsible sidebar section for grouping tabs.
- `Window:Divider()`: Inserts a visual divider line in the sidebar.
- `Window:OnOpen(fn)`, `Window:OnClose(fn)`, `Window:OnDestroy(fn)`: Window lifecycle event listeners.
- `Window.User`:
  - `Window.User:Enable()`: Shows player profile card.
  - `Window.User:Disable()`: Hides player profile card.
  - `Window.User:SetAnonymous(boolean)`: Hides player identity.
  - `Window.User:SetSubtitle(string)`: Updates subtitle text beneath display name.
  - `Window.User:GetTab()`: Retrieves the Profile Tab object. If not yet initialized, creates an empty custom tab on demand.
  - `Window.User:OpenProfile()`: Switches to the Profile Tab and highlights the profile card.
  - `Window.User.ProfileTab` / `Window.User.Tab`: Direct reference to the Profile Tab.
- `Window.ConfigManager`:
  - `ConfigManager:CreateConfig(name)`: Returns config object with `:Save()`, `:Load()`, `:Delete()`, `:Set(k, v)`, `:Get(k)`.
  - `ConfigManager:AllConfigs()`: Returns table of existing config names.
  - `ConfigManager:DeleteConfig(name)`: Deletes config file.

---

### 3. Global Library Methods (WindUI)

- `WindUI:CreateWindow(config)`: Constructs and displays a window instance.
- `WindUI:Notify({ Title, Content, Icon, Duration, Progress, Buttons })`: Displays a notification toast with optional action buttons and progress bar.
- `WindUI:Popup({ Title, Content, Icon, Thumbnail, Buttons })`: Displays modal alert with actions.
- `WindUI:SetNotificationUpper(boolean)`: Positions toast notifications at the top-right (`true`) or bottom-right (`false`).
- `WindUI:SetNotificationLower(boolean)`: Shifts notifications slightly down to clear custom bars.
- `WindUI:SetDisplayOrder(number)`: Adjusts ScreenGui DisplayOrder / ZIndex above game GUIs (aliases: `SetZIndex`, `SetLayer`).
- `WindUI:SetTheme(name)`: Changes active theme across all open interfaces. Supports flexible name matching (`"Dark"`, `"Liquid Glass"`, `"Ocean Blue"`, etc.).
- `WindUI:GetThemes()`: Returns dictionary of all registered themes.
- `WindUI:GetCurrentTheme()`: Returns name string of current theme.
- `WindUI:AddTheme(themeTable)`: Registers a custom theme palette.
- `WindUI:Gradient(keypoints, options)`: Creates gradient sequence table with rotation for themes.
- `WindUI:SetFont(fontAssetId)`: Changes global font family across all elements.
- `WindUI:SetLanguage("en" | "vi" | "es" | "fr" | "ru" | "ja" | ...)`: Universal dynamic auto-translation. Automatically translates all UI text into ANY chosen language on the fly without hardcoded dictionaries.
- `WindUI:AddTranslations(langCode, customDict)`: Adds custom manual translation overrides for any language.
- `WindUI:Translate(text, langCode, callback)`: Translates any string dynamically with caching.
- `WindUI:ToggleAcrylic(boolean)`: Toggles acrylic blur state globally.

---

### 4. Tab, Sub-Tabs, and Section Structure

- `Window:Tab({ Title, Icon, Desc, TwoColumns, LockedTo, HiddenTo, Whitelist, WhitelistHidden, WhitelistAction, ShowTabTitle, IconShape, IconColor, CustomEmptyPage })`: Creates navigation tab.
  - `TwoColumns = true`: Splits the tab into two responsive columns. Cards can specify `Side = "Left"` or `Side = "Right"`, or `FullWidth = true`.
  - `TwoColumns = false`: Keeps the tab in a single clean column.
  - `LockedTo = { userId, ... }`: Restricts tab with lock overlay for non-whitelisted users.
  - `Whitelist = { userId, ... }`: Access whitelist.
    - `WhitelistHidden = true` (default): Hides the tab completely from non-whitelisted users.
    - `WhitelistHidden = false` or `WhitelistAction = "Show"`: Keeps the tab visible in the sidebar, but locks it with an access restriction screen when clicked.
  - `IconShape = "Circle"`: Renders container badge around tab icon.
  - `IconColor = Color3.fromRGB(...)`: Custom tint for tab icon.
- `Tab:SubTabs({ { Title, Icon, TwoColumns }, ... })`: Embeds a segmented pill bar inside the tab with independent sub-pages. Each sub-page can individually customize its column layout:
  - `TwoColumns = true`: Splits the sub-page into two columns (Cards can use `Side = "Left"` or `Side = "Right"`, or `FullWidth = true`).
  - `TwoColumns = false`: Keeps the sub-page in a single full-width column.
  - `TwoColumns = nil` (default): Inherits the parent tab's `TwoColumns` setting.
  ```lua
  local sub = FarmTab:SubTabs({
      { Title = "Level Farm", Icon = "sparkles", TwoColumns = true },   -- 2 columns
      { Title = "Tuning", Icon = "wrench", TwoColumns = true },         -- 2 columns
      { Title = "Combat", Icon = "swords", TwoColumns = false },        -- 1 column
  })
  local LevelPage = sub["Level Farm"]
  LevelPage:Card({ Title = "Auto Level", Side = "Left" }):Toggle({ Title = "Enabled" })
  LevelPage:Card({ Title = "Rewards", Side = "Right" }):Slider({ Title = "Multiplier", Value = 2 })
  ```
- `Tab:AddSearch(placeholder)`: Embeds real-time filter input inside tab.
- `Tab:Banner({ Image, Height, Title, Desc })`: Adds visual header banner across the tab.
- `Tab:Card({ Title, Icon, Desc, Side = "Left" | "Right", FullWidth, Badge, BadgeColor })`: Creates framed container for controls (aliases: `Section`, `Groupbox`).
- `Section:AddBadge(text, color)`: Adds status badge to section header.
- `Section:AddHeaderButton(icon, callback)`: Adds icon action button to section header.
- `Section:Flash(color)`: Highlights section border momentarily.
- `Section:Open()`, `Section:Close()`: Programmatically expands or collapses section content.
- `Tab:Column("Left" | "Right")`: Returns column container to place cards directly.
- `Tab:AddPage(title, icon)` / `Tab:SubPage`: Creates nested sub-page view within tab.
- `Tab:CollapseAll()`, `Tab:ExpandAll()`: Toggles all collapsible cards inside tab.
- `Tab:LockAll()`, `Tab:UnlockAll()`: Toggles lock state across all child elements.

---

### 5. Controls and UI Elements

All elements can be created on `Tab`, `SubPage`, `Card`, `Section`, `Column`, or modal `Dialog`:

- `Button({ Title, Desc, Icon, Callback, Color, Justify, IconAlign, Locked, LockedTitle })`
- `Toggle({ Title, Desc, Value, Callback, Type = "Toggle" | "Checkbox", Icon, Locked })`
  - Methods: `:Set(boolean)`
- `Slider({ Title, Desc, Value = { Min, Max, Default }, Step, Callback, IsTextbox, IsTooltip, Icons = { From, To }, Width, Locked })`
  - Methods: `:Set(number)`, `:SetMin(number)`, `:SetMax(number)`
- `Dropdown({ Title, Desc, Values, Value, Multi, AllowNone, SearchBarEnabled, MenuWidth, Callback, Locked })`: Clicking anywhere on the row opens the menu; selecting in single mode automatically selects and closes the dropdown cleanly.
  - Methods: `:Select(value)`, `:Refresh(values)`, `:Open()`, `:Close()`
- `Input({ Title, Desc, Placeholder, Value, ClearTextOnFocus, Type = "Input" | "Textarea", InputIcon, Callback, Locked })`
  - Methods: `:Set(text)`, `:SetPlaceholder(text)`
- `Keybind({ Title, Desc, Value, CanChange, Blacklist, Callback, Locked })`
  - Methods: `:Set(keyName)`
- `Colorpicker({ Title, Desc, Default, Transparency, Callback, Locked })`
  - Methods: `:Set(color3, transparency)`
- `HoldButton({ Title, Desc, HoldTime, Tag, TagColor, Callback })`: Requires pressing and holding for duration before triggering.
- `ProgressBar({ Title, Desc, Value = { Min, Max, Default }, ShowValue, DisplayMode = "Percent" | "Value" | "Fraction", Format, Animate, Indeterminate, Speed })`
  - Methods: `:Set(number)`, `:Get()`, `:GetPercentage()`, `:SetMin(number)`, `:SetMax(number)`
- `Countdown({ Title, Seconds, TargetTime, OnEnd })`: Real-time countdown timer with automated callback on expiration.
- `Changelog({ Title, Version, Items = { "+ Added", "- Removed", "* Changed" } })`: Formatted release notes.
- `Code({ Title, Code, CodeSize, Height, CodeTheme, CanCopied, OnCopy })`: Syntax-highlighted code block with copy button.
  - Methods: `:SetCode(text)`
- `Viewport({ Object, Camera, Height, Interactive, Focused })`: Embeds 3D models with interactive orbit rotation and zoom.
  - Methods: `:SetObject(instance, clone)`, `:Focus()`, `:SetHeight(number)`, `:SetInteractive(boolean)`, `:SetCamera(camera)`
- `Paragraph({ Title, Desc, Image, Thumbnail, Buttons = { { Title, Icon, Variant, Callback } } })`
  - Methods: `:SetTitle(text)`, `:SetDesc(text)`, `:SetImage(img)`, `:SetThumbnail(thumb)`
- `Image({ Image, AspectRatio = "16:9", Radius })`: Responsive image frame with aspect ratio constraint.
- `Divider(title or nil)`: Horizontal separator line or labeled section divider when text is provided.
- `Space(columns or nil)`: Vertical spacing gap.
- `Group(fn)` / `HStack` / `VStack`: Horizontal or vertical grouping containers.
- `Embed({ Title, Desc, Color, Fields = { { Title, Value } }, Footer })`: Rich embed card.

---

### 6. Key System Configuration

WindUI includes a full key system modal with validation providers:

```lua
local Window = WindUI:CreateWindow({
    Title = "Secured Hub",
    Folder = "SecuredHub",
    KeySystem = {
        Title = "Key System",
        Note = "Join our Discord to obtain your license key.",
        URL = "https://discord.gg/mSrMzVuc3h",
        SaveKey = true,
        Key = "SECRET_ACCESS_KEY", -- Or table: { "KEY_A", "KEY_B" }
        -- Or custom validator:
        KeyValidator = function(enteredKey)
            return enteredKey == "SECRET_ACCESS_KEY"
        end,
        Thumbnail = {
            Image = "rbxassetid://133610205520685",
            Title = "Key Verification",
        },
    },
})
```

---

## Complete Example

This standalone boilerplate incorporates every element and API in the library. Run it directly in Roblox to test all features:

```lua
local WindUI = loadstring(game:HttpGet("https://raw.githubusercontent.com/caomod2077/Windui-better/main/windui.lua"))()

local Window = WindUI:CreateWindow({
    Title = "WindUI Full API Showcase",
    Author = "by Caomod2077",
    Folder = "WindUIFullShowcase",
    Theme = "Dark", -- Default: Dark (also supports Liquid Glass, Ocean Blue, Obsidian, Light, Mid Summer, etc.)
    Size = UDim2.fromOffset(860, 580),
    TwoColumns = true,
    BackgroundBlur = false,
    Icon = "rbxassetid://82225829203828",
    IconAnimation = "Spin",
    SidebarBanner = {
        Image = "rbxassetid://133610205520685",
        Height = 72,
        Title = "WindUI v2.0",
    },
    -- Player Profile Card on the bottom-left sidebar
    User = {
        Enabled = true,               -- Display player thumbnail, display name, and subtitle
        Anonymous = false,            -- When true, displays as Anonymous and hides avatar
        Subtitle = "Developer",        -- Subtitle or status badge text
        Profile = {
            Enabled = true,
            Custom = true,            -- Custom mode: create an empty profile tab for custom utilities
            Title = "Local Player",
        },
        Callback = function()         -- Optional click callback
            print("Player profile card clicked")
        end,
    },
})

-- Topbar Notifications & Layer Order
WindUI:SetNotificationUpper(true)
WindUI:SetDisplayOrder(999)

-- Sound System with Custom Sound IDs
Window:SetSounds({
    Tab = "rbxassetid://6895079853",
    Toggle = "rbxassetid://4590662766",
    Volume = 0.35,
})
Window:EnableSounds(true)

-- Topbar Tag & Stats Watermark
Window:Tag({
    Title = "PRO",
    Color = Color3.fromRGB(0, 145, 255),
    Radius = 8,
})

local watermark = Window:Watermark({
    Title = "WindUI Showcase",
    FPS = true,
    Ping = true,
})

-- Sidebar Sections grouping Navigation Tabs
local HomeSection = Window:Section({ Title = "Home", Icon = "star", Opened = true })
local ControlsTab = HomeSection:Tab({ Title = "Controls", Icon = "sliders-horizontal" })
local SubTabsTab = HomeSection:Tab({ Title = "Sub-Tabs Demo", Icon = "layers" })
local DisplayTab = HomeSection:Tab({ Title = "3D & Display", Icon = "box", TwoColumns = false })

Window:Divider()

local AdvancedSection = Window:Section({ Title = "Advanced", Icon = "box", Opened = true })
local LayoutTab = AdvancedSection:Tab({ Title = "Layout & Groups", Icon = "layout-grid" })
local WhitelistTab = AdvancedSection:Tab({
    Title = "Staff Only Tab",
    Icon = "shield-alert",
    Whitelist = { 12345678, 87654321 },
    WhitelistHidden = false, -- false: show in sidebar as locked | true (default): hide completely
    Desc = "Locked tab demonstrating Whitelist protection.",
})

Window:Divider()

local SettingsTab = Window:Tab({ Title = "Settings", Icon = "settings" })

-- In-tab search
ControlsTab:AddSearch("Search controls...")

local BasicCard = ControlsTab:Card({
    Title = "Basic Controls",
    Icon = "mouse-pointer-click",
    Side = "Left",
    Badge = "CORE",
    BadgeColor = Color3.fromRGB(0, 145, 255),
})

BasicCard:Button({
    Title = "Standard Button",
    Desc = "Triggers callback when clicked",
    Icon = "play",
    Callback = function()
        WindUI:Notify({ Title = "Button", Content = "Button was clicked!", Duration = 3 })
    end,
})

BasicCard:Toggle({
    Title = "Feature Toggle",
    Desc = "Standard toggle control",
    Value = false,
    Callback = function(state) end,
})

BasicCard:Toggle({
    Title = "Checkbox Mode",
    Type = "Checkbox",
    Value = true,
    Callback = function(state) end,
})

BasicCard:Slider({
    Title = "Slider with Textbox",
    Value = { Min = 0, Max = 100, Default = 50 },
    Step = 1,
    IsTextbox = true,
    IsTooltip = true,
    Callback = function(val) end,
})

BasicCard:Dropdown({
    Title = "Dropdown (Search Enabled)",
    Values = { "Apple", "Banana", "Cherry", "Dragonfruit", "Elderberry" },
    Value = "Banana",
    SearchBarEnabled = true,
    Callback = function(choice) end,
})

BasicCard:Dropdown({
    Title = "Multi-Select Dropdown",
    Values = { "Speed Boost", "Jump Power", "Invisibility", "Godmode" },
    Value = { "Speed Boost" },
    Multi = true,
    Callback = function(selected) end,
})

local AdvancedCard = ControlsTab:Card({
    Title = "Advanced Inputs",
    Icon = "keyboard",
    Side = "Right",
})

AdvancedCard:Input({
    Title = "Text Input",
    Placeholder = "Type your text here...",
    Callback = function(text) end,
})

AdvancedCard:Input({
    Title = "Multi-line Textarea",
    Type = "Textarea",
    Placeholder = "Enter multiple lines of text...",
    Callback = function(text) end,
})

AdvancedCard:Keybind({
    Title = "Action Keybind",
    Value = "E",
    Callback = function(key) end,
})

AdvancedCard:Colorpicker({
    Title = "Color with Transparency",
    Default = Color3.fromRGB(0, 191, 255),
    Transparency = 0.2,
    Callback = function(color, trans) end,
})

AdvancedCard:HoldButton({
    Title = "Hold to Confirm Action",
    Desc = "Must hold for 1.5 seconds",
    HoldTime = 1.5,
    Tag = "SAFETY",
    TagColor = Color3.fromRGB(239, 68, 68),
    Callback = function()
        WindUI:Notify({ Title = "Hold Success", Content = "Action executed safely!", Duration = 3 })
    end,
})

-- Sub-Tabs Demo (Nested tabs with customizable column layout per sub-page)
local subPages = SubTabsTab:SubTabs({
    { Title = "Level Farm", Icon = "sparkles", TwoColumns = true },   -- 2 columns
    { Title = "Tuning", Icon = "wrench", TwoColumns = true },         -- 2 columns
    { Title = "Combat", Icon = "swords", TwoColumns = false },        -- 1 column
})

-- 1. Level Farm (2-column layout)
local farmPage = subPages["Level Farm"]
local farmCard = farmPage:Card({ Title = "Auto Level Farming", Side = "Left" })
farmCard:Toggle({ Title = "Auto Mob Kill", Value = true })
farmCard:Slider({ Title = "Mob Distance", Value = 30, Min = 5, Max = 100 })

local farmRewardCard = farmPage:Card({ Title = "Reward Multipliers", Side = "Right" })
farmRewardCard:Toggle({ Title = "Double EXP", Value = true })
farmRewardCard:Slider({ Title = "Loot Magnet Range", Value = 50, Min = 10, Max = 100 })

-- 2. Tuning (2-column layout)
local tuningPage = subPages["Tuning"]
local tuningCard = tuningPage:Card({ Title = "Vehicle & Speed Tuning", Side = "Left" })
tuningCard:Slider({ Title = "Engine Torque Multiplier", Value = 150, Min = 100, Max = 500 })

local tuningHandlingCard = tuningPage:Card({ Title = "Handling & Physics", Side = "Right" })
tuningHandlingCard:Toggle({ Title = "Drift Assist", Value = false })

-- 3. Combat (1-column layout)
local combatPage = subPages["Combat"]
local combatCard = combatPage:Card({ Title = "Combat Exploits (Single Column)" })
combatCard:Button({
    Title = "Instant Kill Aura",
    Callback = function()
        WindUI:Notify({ Title = "Combat", Content = "Aura activated!", Duration = 2 })
    end,
})

-- 3D Viewport Model Viewer (Single Column)
local ViewportCard = DisplayTab:Card({
    Title = "3D Viewport Viewer",
    Icon = "box",
})

local demoPart = Instance.new("Part")
demoPart.Size = Vector3.new(2, 2, 2)
demoPart.Color = Color3.fromRGB(0, 191, 255)
demoPart.Material = Enum.Material.Neon
demoPart.Anchored = true

local viewport = ViewportCard:Viewport({
    Height = 180,
    Object = demoPart,
    Interactive = true, -- Drag with mouse/touch to rotate 360, scroll/pinch to zoom
    Focused = true,     -- Automatically centers camera on object
})

local MediaCard = DisplayTab:Card({
    Title = "Progress & Code Display",
    Icon = "file-code",
})

local progress = MediaCard:ProgressBar({
    Title = "Task Progress",
    Value = { Min = 0, Max = 100, Default = 45 },
    ShowValue = true,
    DisplayMode = "Percent",
})

MediaCard:ProgressBar({
    Title = "Indeterminate Spinner",
    Indeterminate = true,
    Speed = 1,
})

MediaCard:Code({
    Title = "Lua Code Snippet",
    Code = [[local function greet(name)
    print("Hello, " .. name)
end
greet("WindUI")]],
    CanCopied = true,
})

MediaCard:Paragraph({
    Title = "Information Paragraph",
    Desc = "This element supports multi-line text with action buttons.",
    Buttons = {
        {
            Title = "Documentation",
            Icon = "external-link",
            Callback = function() end,
        },
    },
})

-- Layout and Grouping
local ColumnsDemo = LayoutTab:Card({
    Title = "Left Column Card",
    Side = "Left",
})
ColumnsDemo:Button({ Title = "Left Column Action" })

local RightDemo = LayoutTab:Card({
    Title = "Right Column Card",
    Side = "Right",
})
RightDemo:Button({ Title = "Right Column Action" })

local FullWidthDemo = LayoutTab:Card({
    Title = "Full Width Spanning Card",
    FullWidth = true,
})
FullWidthDemo:Button({ Title = "Spans across both columns" })

-- Horizontal group of buttons (HStack)
local HStackDemo = LayoutTab:Card({
    Title = "HStack Horizontal Group",
    Side = "Left",
})
local hstack = HStackDemo:HStack()
hstack:Button({ Title = "Btn 1" })
hstack:Button({ Title = "Btn 2" })
hstack:Button({ Title = "Btn 3" })

-- Settings Tab
local InterfaceSection = SettingsTab:Section({ Title = "Interface Options" })

InterfaceSection:Dropdown({
    Title = "Theme Selector",
    Values = { "Dark", "Liquid Glass", "Ocean Blue", "Obsidian", "Light", "Mid Summer", "Lunar Moon", "Winter Frost", "Elegant Night", "Lunar Abyss", "Lunar Eclipse", "Rose", "Plant", "Indigo", "Violet", "Amber", "Midnight", "MonokaiPro" },
    Value = "Dark",
    Callback = function(theme)
        Window:SetTheme(theme)
    end,
})

InterfaceSection:Dropdown({
    Title = "Language (Universal Auto-Translation)",
    Values = { "English", "Tiếng Việt", "Español", "Français", "Русский", "日本語" },
    Value = "English",
    Callback = function(lang)
        local map = {
            ["English"] = "en",
            ["Tiếng Việt"] = "vi",
            ["Español"] = "es",
            ["Français"] = "fr",
            ["Русский"] = "ru",
            ["日本語"] = "ja",
        }
        WindUI:SetLanguage(map[lang] or "en")
    end,
})

InterfaceSection:Toggle({
    Title = "UI Sound Effects",
    Value = true,
    Callback = function(enabled)
        Window:EnableSounds(enabled)
    end,
})

InterfaceSection:Toggle({
    Title = "Background Blur",
    Value = false,
    Callback = function(enabled)
        Window:SetBackgroundBlur(enabled)
    end,
})

InterfaceSection:Toggle({
    Title = "Top-Right Notifications",
    Value = true,
    Callback = function(upper)
        WindUI:SetNotificationUpper(upper)
    end,
})

InterfaceSection:Toggle({
    Title = "Watermark Visibility",
    Value = true,
    Callback = function(visible)
        watermark:SetVisible(visible)
    end,
})

InterfaceSection:Toggle({
    Title = "Compact Sidebar Mode",
    Value = false,
    Callback = function(collapsed)
        Window:ToggleSidebar(collapsed)
    end,
})

-- Player Profile Card Controls
local UserSection = SettingsTab:Section({ Title = "Player Profile (Sidebar)" })

UserSection:Toggle({
    Title = "Show Player Profile",
    Value = true,
    Callback = function(enabled)
        if enabled then
            Window.User:Enable()
        else
            Window.User:Disable()
        end
    end,
})

UserSection:Toggle({
    Title = "Anonymous Mode",
    Value = false,
    Callback = function(anonymous)
        Window.User:SetAnonymous(anonymous)
    end,
})

UserSection:Input({
    Title = "Custom Subtitle",
    Value = "Developer",
    Callback = function(text)
        Window.User:SetSubtitle(text)
    end,
})

UserSection:Button({
    Title = "Open Player Profile Tab",
    Callback = function()
        Window.User:OpenProfile()
    end,
})

-- ========================================================
-- Custom Player Profile Tab (Universal Local Player Utilities)
-- Click the profile card at the bottom of the sidebar to view!
-- ========================================================
local ProfileTab = Window.User:GetTab()

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local LocalPlayer = Players.LocalPlayer

local function getHumanoid()
    local char = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
    return char:FindFirstChildOfClass("Humanoid")
end

local MovementCard = ProfileTab:Card({
    Title = "Character Movement",
    Icon = "zap",
    Side = "Left",
    Badge = "LOCAL",
    BadgeColor = Color3.fromRGB(0, 145, 255),
})

MovementCard:Slider({
    Title = "WalkSpeed",
    Desc = "Adjust local player movement speed",
    Value = { Min = 16, Max = 250, Default = 16 },
    Step = 1,
    IsTextbox = true,
    Callback = function(speed)
        local hum = getHumanoid()
        if hum then hum.WalkSpeed = speed end
    end,
})

MovementCard:Slider({
    Title = "JumpPower",
    Desc = "Adjust jump height and power",
    Value = { Min = 50, Max = 300, Default = 50 },
    Step = 1,
    IsTextbox = true,
    Callback = function(power)
        local hum = getHumanoid()
        if hum then
            hum.UseJumpPower = true
            hum.JumpPower = power
        end
    end,
})

local infJumpConn
MovementCard:Toggle({
    Title = "Infinite Jump",
    Desc = "Jump continuously in midair",
    Value = false,
    Callback = function(enabled)
        if enabled then
            infJumpConn = UserInputService.JumpRequest:Connect(function()
                local hum = getHumanoid()
                if hum then
                    hum:ChangeState(Enum.HumanoidStateType.Jumping)
                end
            end)
        elseif infJumpConn then
            infJumpConn:Disconnect()
            infJumpConn = nil
        end
    end,
})

local noclipConn
MovementCard:Toggle({
    Title = "Noclip",
    Desc = "Pass through walls and solid objects",
    Value = false,
    Callback = function(enabled)
        if enabled then
            noclipConn = RunService.Stepped:Connect(function()
                local char = LocalPlayer.Character
                if char then
                    for _, part in ipairs(char:GetDescendants()) do
                        if part:IsA("BasePart") and part.CanCollide then
                            part.CanCollide = false
                        end
                    end
                end
            end)
        elseif noclipConn then
            noclipConn:Disconnect()
            noclipConn = nil
        end
    end,
})

local UtilityCard = ProfileTab:Card({
    Title = "Player Utilities & Camera",
    Icon = "user-cog",
    Side = "Right",
})

UtilityCard:Slider({
    Title = "Field of View (FOV)",
    Desc = "Adjust camera field of view",
    Value = { Min = 70, Max = 120, Default = 70 },
    Step = 1,
    IsTextbox = true,
    Callback = function(fov)
        workspace.CurrentCamera.FieldOfView = fov
    end,
})

UtilityCard:Button({
    Title = "Reset Character",
    Desc = "Instantly respawn local character",
    Icon = "skull",
    Variant = "Destructive",
    Callback = function()
        local hum = getHumanoid()
        if hum then hum.Health = 0 end
    end,
})

UtilityCard:Button({
    Title = "Copy Coordinates",
    Desc = "Copy your current Vector3 coordinates",
    Icon = "copy",
    Variant = "Secondary",
    Callback = function()
        local char = LocalPlayer.Character
        local root = char and char:FindFirstChild("HumanoidRootPart")
        if root then
            local posStr = string.format("%.2f, %.2f, %.2f", root.Position.X, root.Position.Y, root.Position.Z)
            if setclipboard then setclipboard(posStr) elseif toclipboard then toclipboard(posStr) end
            WindUI:Notify({ Title = "Copied", Content = "Position copied: " .. posStr, Duration = 2 })
        end
    end,
})

local ConfigSection = SettingsTab:Section({ Title = "Config Management" })
local ConfigManager = Window.ConfigManager
local DemoConfig = ConfigManager and ConfigManager:CreateConfig("demo") or nil

if DemoConfig then
    ConfigSection:Button({
        Title = "Save Config",
        Callback = function()
            DemoConfig:Save()
            WindUI:Notify({ Title = "Config", Content = "Configuration saved successfully!", Duration = 2 })
        end,
    })

    ConfigSection:Button({
        Title = "Load Config",
        Callback = function()
            DemoConfig:Load()
            WindUI:Notify({ Title = "Config", Content = "Configuration loaded!", Duration = 2 })
        end,
    })
end

-- Custom floating open button for mobile
Window:EditOpenButton({
    Title = "Open UI",
    CornerRadius = UDim.new(0, 16),
    StrokeThickness = 2,
    Draggable = true,
    Color = ColorSequence.new(
        Color3.fromRGB(0, 191, 255),
        Color3.fromRGB(255, 105, 180)
    ),
    Enabled = true,
    OnlyMobile = false,
})

-- Automatically display the first tab
ControlsTab:Select()
```

---

## Built-in Themes

| Theme Name | Description |
|---|---|
| `Dark` | Standard clean modern dark theme (Default) |
| `Liquid Glass` | Transparent frosted glass with cyan neon glow and see-through background |
| `Ocean Blue` | Deep oceanic navy and cyan gradient palette (formerly Blue Something) |
| `Obsidian` | Dark charcoal base with orange accents |
| `Light` | Clean bright daytime theme |
| `Rose` | Dark wine and rose palette |
| `Plant` | Deep forest green with emerald accents |
| `Red` | Crimson dark theme |
| `Indigo` | Deep indigo blue palette |
| `Sky` | Cyan and navy aesthetic |
| `Violet` | Purple and violet palette |
| `Amber` | Warm amber gradient theme |
| `Emerald` | Deep emerald green palette |
| `Midnight` | Navy and deep royal blue |
| `Crimson` | Crimson and ruby dark palette |
| `MonokaiPro` | Monokai Pro colors |
| `CottonCandy` | Pastel pink and purple theme |
| `Mellowsi` | Muted brown and gold palette |
| `Rainbow` | Vibrant multi-color gradient theme |
| `Mid Summer` | Warm coral and sunset gradients |
| `Lunar Moon` | Monochrome silver gradients |
| `Winter Frost` | Ice-blue gradients |
| `Elegant Night` | Deep violet gradients |
| `Lunar Abyss` | Dark indigo gradients |
| `Lunar Eclipse` | Deep rose and magenta gradients |

---

## License

This project is licensed under the [MIT License](LICENSE).

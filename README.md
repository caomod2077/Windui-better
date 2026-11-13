# WindUI-Better

[English](README.md) | [Tieng Viet](README_VI.md)

An enhanced edition of WindUI for Roblox with support for two-column card layouts, in-tab search filtering, built-in gradient themes, watermark stats, tab access control, interactive 3D viewports, and performance optimizations.

Original library by Footagesus. Maintained and expanded by caomod2077.

---

## Performance Optimizations

Standard WindUI builds can experience frame drops and memory accumulation in complex interfaces. WindUI-Better introduces the following engine-level optimizations:

1. Automatic Dead Object Pruning: Theme objects and signals bound to destroyed UI elements are automatically unregistered instead of leaking in global tables.
2. Memoized Shape Scaling: Squircle and Glass vector calculations only execute when dimensions actually change, preventing thousands of redundant math operations per frame during animations.
3. Zero-Allocation Watermark: Frame rate calculation replaces array mutations with lightweight timestamp accumulators, eliminating per-frame memory allocation and garbage collection churn.
4. Visibility-Gated Render Loops: Icon rotation, rainbow animations, and watermark updates immediately pause when the window is closed or hidden, saving CPU cycles for the game engine.
5. Lifecycle-Bounded Component Loops: Background loops in Countdown and animated elements automatically disconnect when their parent element is destroyed.

---

## Quick Start

```lua
local WindUI = loadstring(game:HttpGet("https://raw.githubusercontent.com/caomod2077/Windui-better/main/windui.lua"))()

local Window = WindUI:CreateWindow({
    Title = "My Script Hub",
    Author = "by Creator",
    Folder = "MyScriptHub",
    Theme = "Dark",
    Size = UDim2.fromOffset(840, 540),
    TwoColumns = true,      -- Enable 2-column card layout (default: false)
    BackgroundBlur = false,  -- Background blur effect (default: false)
    Glow = Color3.fromRGB(0, 145, 255),
    IconAnimation = "Spin", -- "Spin" | "Pulse" | "Rainbow"
    SidebarBanner = {
        Image = "rbxassetid://133610205520685",
        Height = 75,
        Title = "Hub Version",
    },
})
```

Background blur can also be toggled at runtime:

```lua
Window:SetBackgroundBlur(true)
Window:SetBackgroundBlur(false)
```

---

## API Reference

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
| `ToggleKey` | Enum.KeyCode | `nil` | Keycode to hide or reveal the window |
| `User` | table | `nil` | User card on bottom of sidebar: `{ Enabled, Anonymous, Subtitle, Callback }` |
| `Topbar` | table | `{ Height = 52 }` | Topbar options: `{ Height = 52, ButtonsType = "Default" / "Mac" }` |

---

### 2. Window Methods

- `Window:Tag({ Title, Color, Radius, Border })`: Adds a pill tag to the topbar.
- `Window:Watermark({ Title, FPS = true, Ping = true })`: Creates topbar stats badge. Returns `{ SetTitle(text), SetVisible(bool), Destroy() }`.
- `Window:SetBackgroundBlur(boolean)`: Toggles lighting blur at runtime.
- `Window:ToggleSidebar(forceState)`: Toggles or sets collapsed icon mode (52px) vs expanded mode (175px).
- `Window:SetColor({ Accent, Background, Text, Dialog, ... })`: Overrides theme colors at runtime.
- `Window:SetUIScale(number)`: Adjusts entire UI scale factor (e.g. `0.85` for mobile).
- `Window:SetToggleKey(Enum.KeyCode)`: Updates keyboard toggle shortcut.
- `Window:SetSize(UDim2)`: Resizes window programmatically.
- `Window:SetToTheCenter()`: Centers window on screen.
- `Window:SetBackgroundImage(string)`: Updates window background (assetid or URL).
- `Window:SetBackgroundImageTransparency(number)`: Adjusts background image opacity (0 to 1).
- `Window:SetIconAnimation("Spin" | "Pulse" | "Rainbow")`: Changes or starts topbar icon animation.
- `Window:AnimateTitle("Gradient" | "Shimmer")`: Starts moving gradient animation on window title.
- `Window:SetSidebarBanner({ Image, Height, Title })`: Updates or creates sidebar banner.
- `Window:Toast({ Title, Content, Duration, Icon })`: Quick notification shortcut.
- `Window:Dialog({ Title, Content, Buttons })`: Opens modal popup that can host interactive child elements.
- `Window:EnableSounds(boolean)`: Toggles audio feedback for tab changes and buttons.
- `Window:PlaySound(soundType)`: Plays sound (`"tab"` or `"toggle"`).
- `Window:EditOpenButton(table)`: Customizes floating toggle button for mobile/touch devices.
- `Window:OnOpen(fn)`, `Window:OnClose(fn)`, `Window:OnDestroy(fn)`: Window lifecycle event listeners.
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
- `WindUI:SetDisplayOrder(number)`: Adjusts ScreenGui DisplayOrder / ZIndex above game GUIs (aliases: `SetZIndex`, `SetLayer`).
- `WindUI:SetTheme(name)`: Changes active theme across all open interfaces.
- `WindUI:GetThemes()`: Returns dictionary of all registered themes.
- `WindUI:GetCurrentTheme()`: Returns name string of current theme.
- `WindUI:AddTheme(themeTable)`: Registers a custom theme palette.
- `WindUI:Gradient(keypoints, options)`: Creates gradient sequence table with rotation for themes.

---

### 4. Tab and Section Structure

- `Window:Tab({ Title, Icon, Desc, TwoColumns, LockedTo, HiddenTo, Whitelist, ShowTabTitle })`: Creates navigation tab.
  - `LockedTo = { userId, ... }`: Restricts tab with lock overlay for non-whitelisted users.
  - `Whitelist = { userId, ... }`: Hides tab completely from non-whitelisted users.
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

---

### 5. Controls and UI Elements

All elements can be created on `Tab`, `Card`, `Section`, `Column`, or modal `Dialog`:

- `Button({ Title, Desc, Icon, Callback, Color, Justify, IconAlign, Locked, LockedTitle })`
- `Toggle({ Title, Desc, Value, Callback, Type = "Toggle" | "Checkbox", Icon, Locked })`
  - Methods: `:Set(boolean)`
- `Slider({ Title, Desc, Value = { Min, Max, Default }, Step, Callback, IsTextbox, IsTooltip, Icons = { From, To }, Width, Locked })`
  - Methods: `:Set(number)`, `:SetMin(number)`, `:SetMax(number)`
- `Dropdown({ Title, Desc, Values, Value, Multi, AllowNone, SearchBarEnabled, MenuWidth, Callback, Locked })`
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
- `DiscordCard({ Invite, Title, Desc, Icon, OnJoin, AutoFetch, Side })`: Live Discord server widget.
- `Embed({ Title, Desc, Color, Fields = { { Title, Value } }, Footer })`: Rich embed card.

---

## Complete Example

This standalone boilerplate incorporates every element and API in the library. Run it directly in Roblox to test all features:

```lua
local WindUI = loadstring(game:HttpGet("https://raw.githubusercontent.com/caomod2077/Windui-better/main/windui.lua"))()

local Window = WindUI:CreateWindow({
    Title = "WindUI Full API Showcase",
    Author = "by Developer",
    Folder = "WindUIFullShowcase",
    Theme = "Dark",
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
})

WindUI:SetNotificationUpper(true)
WindUI:SetDisplayOrder(999)
Window:EnableSounds(true)

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

local ControlsTab = Window:Tab({ Title = "Controls", Icon = "sliders-horizontal" })
local DisplayTab = Window:Tab({ Title = "3D & Display", Icon = "box" })
local CommunityTab = Window:Tab({ Title = "Community", Icon = "users" })
local LayoutTab = Window:Tab({ Title = "Layout", Icon = "layout-grid" })
local SettingsTab = Window:Tab({ Title = "Settings", Icon = "settings" })

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
    Callback = function(state)
        print("Toggle state:", state)
    end,
})

BasicCard:Toggle({
    Title = "Checkbox Mode",
    Type = "Checkbox",
    Value = true,
    Callback = function(state)
        print("Checkbox state:", state)
    end,
})

BasicCard:Slider({
    Title = "Slider with Textbox",
    Value = { Min = 0, Max = 100, Default = 50 },
    Step = 1,
    IsTextbox = true,
    IsTooltip = true,
    Callback = function(val)
        print("Slider value:", val)
    end,
})

BasicCard:Dropdown({
    Title = "Single Dropdown",
    Values = { "Apple", "Banana", "Cherry", "Dragonfruit" },
    Value = "Banana",
    Callback = function(choice)
        print("Selected fruit:", choice)
    end,
})

BasicCard:Dropdown({
    Title = "Multi-Select Dropdown",
    Values = { "Speed Boost", "Jump Power", "Invisibility", "Godmode" },
    Value = { "Speed Boost" },
    Multi = true,
    Callback = function(selected)
        print("Selected count:", type(selected) == "table" and #selected or 0)
    end,
})

local AdvancedCard = ControlsTab:Card({
    Title = "Advanced Inputs",
    Icon = "keyboard",
    Side = "Right",
})

AdvancedCard:Input({
    Title = "Text Input",
    Placeholder = "Type your text here...",
    Callback = function(text)
        print("Input submitted:", text)
    end,
})

AdvancedCard:Input({
    Title = "Multi-line Textarea",
    Type = "Textarea",
    Placeholder = "Enter multiple lines of text...",
    Callback = function(text)
        print("Textarea:", text)
    end,
})

AdvancedCard:Keybind({
    Title = "Action Keybind",
    Value = "E",
    Callback = function(key)
        WindUI:Notify({ Title = "Keybind Fired", Content = "Key " .. tostring(key) .. " pressed!", Duration = 2 })
    end,
})

AdvancedCard:Colorpicker({
    Title = "Color with Transparency",
    Default = Color3.fromRGB(0, 191, 255),
    Transparency = 0.2,
    Callback = function(color, trans)
        print("Color selected:", color, "Transparency:", trans)
    end,
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

local ViewportCard = DisplayTab:Card({
    Title = "3D Viewport Viewer",
    Icon = "box",
    Side = "Left",
    FullWidth = true,
})

local demoPart = Instance.new("Part")
demoPart.Size = Vector3.new(2, 2, 2)
demoPart.Color = Color3.fromRGB(0, 191, 255)
demoPart.Material = Enum.Material.Neon
demoPart.Anchored = true

local viewport = ViewportCard:Viewport({
    Height = 180,
    Object = demoPart,
    Interactive = true, -- Drag with mouse/touch to rotate, scroll/pinch to zoom
    Focused = true,     -- Automatically centers camera on object
})

local MediaCard = DisplayTab:Card({
    Title = "Progress & Code Display",
    Icon = "file-code",
    Side = "Left",
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
    Code = "local function greet(name)
    print('Hello, ' .. name)
end
greet('WindUI')",
    CanCopied = true,
})

MediaCard:Paragraph({
    Title = "Information Paragraph",
    Desc = "This element supports multi-line text with action buttons.",
    Buttons = {
        {
            Title = "Documentation",
            Icon = "external-link",
            Callback = function()
                print("Opening documentation")
            end,
        },
    },
})

CommunityTab:Banner({
    Image = "rbxassetid://133610205520685",
    Height = 90,
    Title = "Official Community",
    Desc = "Live announcements, updates, and releases.",
})

CommunityTab:Countdown({
    Title = "Next Release Countdown",
    Seconds = 7200,
    OnEnd = function()
        WindUI:Notify({ Title = "Countdown", Content = "Event is now live!", Duration = 5 })
    end,
})

CommunityTab:Changelog({
    Version = "2.1.0",
    Items = {
        "+ Engine performance optimizations and dead object cleanup",
        "+ Full Viewport 3D interactive model integration",
        "+ Zero-allocation watermark and render loop gating",
        "- Removed redundant math recalculations",
        "* Smoother animations across all elements",
    },
})

CommunityTab:DiscordCard({
    Invite = "mSrMzVuc3h",
    Title = "Foxname Community",
    Desc = "Join our server for script releases and support.",
})

CommunityTab:Embed({
    Title = "System Status",
    Desc = "All background services operating normally.",
    Color = Color3.fromRGB(34, 197, 94),
    Fields = {
        { Title = "Server Version", Value = "v2.1.0-better" },
        { Title = "Uptime", Value = "99.98%" },
    },
    Footer = "Status checked automatically",
})

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

local InterfaceSection = SettingsTab:Section({ Title = "Interface Options" })

InterfaceSection:Dropdown({
    Title = "Theme Selector",
    Values = { "Dark", "Obsidian", "Light", "Mid Summer", "Lunar Moon", "Winter Frost", "Elegant Night", "Lunar Abyss", "Lunar Eclipse", "Rose", "Plant", "Indigo", "Violet", "Amber", "Midnight", "MonokaiPro" },
    Value = "Dark",
    Callback = function(theme)
        WindUI:SetTheme(theme)
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

InterfaceSection:Slider({
    Title = "UI Scale",
    Value = { Min = 0.65, Max = 1.25, Default = 1.0 },
    Step = 0.05,
    Callback = function(scale)
        Window:SetUIScale(scale)
    end,
})

InterfaceSection:Button({
    Title = "Interactive Dialog Modal",
    Callback = function()
        local dlg = Window:Dialog({
            Title = "Dialog Controls",
            Content = "Adjust settings inside popup modal:",
            Buttons = {
                { Title = "Apply", Variant = "Primary" },
                { Title = "Cancel", Variant = "Secondary" },
            },
        })
        dlg:Toggle({ Title = "Turbo Mode", Value = true })
        dlg:Slider({ Title = "Speed Factor", Value = 75, Min = 10, Max = 100 })
    end,
})

local ConfigSection = SettingsTab:Section({ Title = "Config Management" })
local ConfigManager = Window.ConfigManager
local DemoConfig = ConfigManager:CreateConfig("demo")

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
```

---

## Built-in Themes

| Theme Name | Description |
|---|---|
| `Dark` | Standard dark theme |
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

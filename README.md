# WindUI-Better

[English](README.md) | [Tiếng Việt](README_VI.md)

An enhanced edition of WindUI for Roblox with support for two-column card layouts, in-tab search filtering, built-in gradient themes, watermark stats, tab access control, and runtime window customization.

Original library by Footagesus. Maintained and expanded by caomod2077.

---

## Quick Start

```lua
local WindUI = loadstring(game:HttpGet("https://raw.githubusercontent.com/caomod2077/Windui-better/main/windui.lua"))()

local Window = WindUI:CreateWindow({
    Title = "My Script Hub",
    Author = "by Creator",
    Theme = "Dark",
    Size = UDim2.fromOffset(800, 520),
    TwoColumns = true, -- Optional 2-column card layout (default: false)
    BackgroundBlur = false, -- Optional background blur (default: false)
    Glow = Color3.fromRGB(0, 145, 255),
    IconAnimation = "Spin", -- "Spin" | "Pulse" | "Rainbow"
    SidebarBanner = {
        Image = "rbxassetid://133610205520685",
        Height = 75,
        Title = "Hub Version",
    },
})
```

Background blur can also be toggled at runtime without recreating the window:

```lua
Window:SetBackgroundBlur(true)
Window:SetBackgroundBlur(false)
```

---

## Features and API Reference

### Two-Column Card Layout (TwoColumns)

By default, `TwoColumns = false` keeps the standard single-column layout. When enabled in `CreateWindow`, sections alternate between left and right columns.

- Override two-column mode per tab:
  ```lua
  local SingleColTab = Window:Tab({ Title = "Info", TwoColumns = false })
  ```
- Make a section span both columns:
  ```lua
  Tab:Section({ Title = "Full Width Section", FullWidth = true })
  ```
- Explicitly assign cards to a side:
  ```lua
  local LeftCard = Tab:Card({ Title = "Farming", Side = "Left" })
  local RightCard = Tab:Card({ Title = "Combat", Side = "Right" })
  ```

---

### Watermark and Stats (Window:Watermark)

Displays an FPS and Ping counter on the window topbar:

```lua
local watermark = Window:Watermark({
    Title = "Script Hub",
    FPS = true,
    Ping = true,
})

watermark:SetTitle("Custom Title")
watermark:SetVisible(true)
```

---

### In-Tab Search (Tab:AddSearch)

Adds a real-time search input inside a tab to filter cards and elements by title:

```lua
MainTab:AddSearch("Search features...")
```

---

### Sidebar and Tab Banners

- Header banner above the sidebar tab list:
  ```lua
  Window:SetSidebarBanner({
      Image = "rbxassetid://133610205520685",
      Height = 75,
      Title = "Welcome",
  })
  ```
- Banner at the top of a specific tab:
  ```lua
  DiscordTab:Banner({
      Image = "rbxassetid://133610205520685",
      Height = 90,
      Title = "Community Server",
      Desc = "Join our Discord for updates, configs, and announcements.",
  })
  ```

---

### Tag Badges on Controls

All standard controls (`Button`, `Toggle`, `Slider`, `Dropdown`, `Input`, `Paragraph`) support pill badges:

```lua
Tab:Toggle({
    Title = "Auto Farm",
    Tag = "HOT",
    TagColor = Color3.fromRGB(249, 115, 22),
    Value = true,
    Callback = function(v) end,
})

Tab:Button({
    Title = "Quick Reset",
    Tag = "ACTION",
    TagColor = Color3.fromRGB(239, 68, 68),
    Callback = function() end,
})
```

---

### HoldButton (Hold to Confirm)

Requires holding down the button for a set duration before firing the callback:

```lua
Tab:HoldButton({
    Title = "Emergency Reset",
    Desc = "Hold for 2 seconds to confirm",
    HoldTime = 2,
    Tag = "HOLD",
    TagColor = Color3.fromRGB(239, 68, 68),
    Callback = function()
        print("Confirmed")
    end,
})
```

---

### Changelog and Countdown

- Automated countdown timer:
  ```lua
  Tab:Countdown({
      Title = "Event Ends In",
      Seconds = 86400,
      OnEnd = function()
          print("Event ended")
      end,
  })
  ```
- Formatted update log (`+` green, `-` red, `*` blue):
  ```lua
  Tab:Changelog({
      Version = "2.0.0",
      Items = {
          "+ Added 2-column card layout",
          "+ Added HoldButton and Countdown",
          "- Fixed memory leaks in loops",
          "* Updated theme handling",
      },
  })
  ```

---

### Notifications (WindUI:Notify)

Toasts with optional progress bar and interactive action buttons:

```lua
WindUI:Notify({
    Title = "Update Complete",
    Content = "WindUI is ready to use.",
    Icon = "sparkles",
    Progress = 0.85,
    Duration = 8,
    Buttons = {
        {
            Title = "Accept",
            Variant = "Primary",
            Callback = function() end,
        },
        {
            Title = "Dismiss",
            Variant = "Secondary",
            Callback = function() end,
        },
    },
})
```

---

### Dialog Modals (Window:Dialog)

Popups that can embed controls (Toggles, Sliders, Dropdowns, Inputs):

```lua
local dlg = Window:Dialog({
    Title = "Settings Confirmation",
    Content = "Adjust settings before continuing:",
    Buttons = {
        { Title = "Apply", Variant = "Primary" },
        { Title = "Cancel", Variant = "Secondary" },
    },
})

dlg:Toggle({ Title = "Fast Mode", Value = true })
dlg:Slider({ Title = "Speed", Value = 80, Min = 10, Max = 100 })
dlg:Dropdown({ Title = "Preset", Values = { "Low", "Medium", "High" }, Value = "High" })
```

---

### Collapsible Sidebar (Window:ToggleSidebar)

Switches the sidebar between full width (175px) and collapsed icon mode (52px):

```lua
Window:ToggleSidebar()       -- Toggle
Window:ToggleSidebar(true)   -- Force collapsed
Window:ToggleSidebar(false)  -- Force expanded
```

---

### Tab Access Control (LockedTo / Whitelist)

Restricts tab access by player UserId:

```lua
-- Locked with an alert overlay
Window:Tab({
    Title = "Admin",
    Icon = "shield-alert",
    LockedTo = { 12345678, 87654321 },
})

-- Completely hidden from non-whitelisted users
Window:Tab({
    Title = "Private",
    Icon = "lock",
    Whitelist = { 12345678 },
})
```

---

### Runtime Color Overrides (Window:SetColor)

Modifies theme colors on the fly without declaring a custom theme:

```lua
Window:SetColor({
    Accent = Color3.fromRGB(0, 255, 170),
    Background = Color3.fromRGB(15, 18, 24),
})
```

---

### Audio Feedback (Window:EnableSounds)

Enables sound effects on tab switching and control interaction:

```lua
Window:EnableSounds(true)
```

---

### Notification Position and Layer Order

- Anchor notifications to top-right instead of bottom-right:
  ```lua
  WindUI:SetNotificationUpper(true)
  ```
- Change DisplayOrder to render above other CoreGui elements:
  ```lua
  WindUI:SetDisplayOrder(999) -- Aliased as SetZIndex and SetLayer
  ```

---

## Complete Example

```lua
local WindUI = loadstring(game:HttpGet("https://raw.githubusercontent.com/caomod2077/Windui-better/main/windui.lua"))()

local Window = WindUI:CreateWindow({
    Title = "WindUI Feature Demo",
    Author = "Example",
    Folder = "WindUIFeatureDemo",
    Theme = "Dark", -- Also try Lunar Moon, Winter Frost, Elegant Night, Lunar Abyss, Lunar Eclipse
    Size = UDim2.fromOffset(860, 580),
    TwoColumns = true,
    BackgroundBlur = false,
    Icon = "rbxassetid://82225829203828",
    IconAnimation = "Spin",
    SidebarBanner = {
        Image = "rbxassetid://133610205520685",
        Height = 72,
        Title = "WindUI Demo",
    },
})

WindUI:SetNotificationUpper(true)
WindUI:SetDisplayOrder(999)
Window:EnableSounds(true)

local watermark = Window:Watermark({ Title = "WindUI Demo", FPS = true, Ping = true })
local MainTab = Window:Tab({ Title = "Features", Icon = "terminal" })
local UpdatesTab = Window:Tab({ Title = "Updates", Icon = "zap" })
local SettingsTab = Window:Tab({ Title = "Settings", Icon = "settings" })

MainTab:AddSearch("Search controls")

local Controls = MainTab:Card({
    Title = "Controls",
    Icon = "sliders-horizontal",
    Side = "Left",
    Badge = "DEMO",
    BadgeColor = Color3.fromRGB(255, 126, 95),
})

Controls:Toggle({
    Title = "Enable feature",
    Desc = "Toggle callback example",
    Tag = "LIVE",
    Value = false,
    Callback = function(value)
        print("Enabled:", value)
    end,
})

Controls:Slider({
    Title = "Intensity",
    Value = 35,
    Min = 0,
    Max = 100,
    Callback = function(value)
        print("Intensity:", value)
    end,
})

Controls:Dropdown({
    Title = "Mode",
    Values = { "Balanced", "Fast", "Precise" },
    Value = "Balanced",
    Callback = function(value)
        print("Mode:", value)
    end,
})

Controls:Input({
    Title = "Text input",
    Placeholder = "Type something",
    Callback = function(value)
        print("Input:", value)
    end,
})

Controls:Button({
    Title = "Show notification",
    Callback = function()
        WindUI:Notify({ Title = "WindUI", Content = "Button pressed", Duration = 4 })
    end,
})

Controls:HoldButton({
    Title = "Hold to confirm",
    HoldTime = 1.5,
    Callback = function()
        WindUI:Notify({ Title = "Confirmed", Content = "Hold completed", Duration = 3 })
    end,
})

local Info = MainTab:Card({ Title = "Status", Icon = "activity", Side = "Right" })
Info:Paragraph({ Title = "Ready", Content = "This card is independent of any game-specific logic." })
Info:Keybind({ Title = "Toggle window", Value = "RightControl" })

UpdatesTab:Countdown({ Title = "Demo countdown", Seconds = 3600 })
UpdatesTab:Changelog({
    Version = "1.0.0",
    Items = { "+ Built-in custom themes", "+ Search and two-column cards", "+ Runtime appearance controls" },
})

local Interface = SettingsTab:Section({ Title = "Interface" })
Interface:Dropdown({
    Title = "Theme",
    Values = { "Dark", "Obsidian", "Light", "Mid Summer", "Lunar Moon", "Winter Frost", "Elegant Night", "Lunar Abyss", "Lunar Eclipse" },
    Value = "Mid Summer",
    Callback = function(theme)
        WindUI:SetTheme(theme)
    end,
})
Interface:Toggle({
    Title = "Background blur",
    Value = false,
    Callback = function(value)
        Window:SetBackgroundBlur(value)
    end,
})
Interface:Toggle({
    Title = "Watermark",
    Value = true,
    Callback = function(value)
        watermark:SetVisible(value)
    end,
})
Interface:Toggle({
    Title = "Compact sidebar",
    Value = false,
    Callback = function(value)
        Window:ToggleSidebar(value)
    end,
})
Interface:Button({
    Title = "Change accent",
    Callback = function()
        Window:SetColor({ Accent = Color3.fromRGB(0, 200, 160) })
    end,
})
Interface:Button({
    Title = "Open dialog",
    Callback = function()
        Window:Dialog({
            Title = "Demo dialog",
            Content = "Dialog controls can be added here.",
            Buttons = {
                { Title = "Close", Callback = function() end },
            },
        })
    end,
})

local Config = SettingsTab:Section({ Title = "Configuration" })
local ConfigManager = Window.ConfigManager
local DemoConfig = ConfigManager:CreateConfig("demo")
Config:Button({
    Title = "Save config",
    Callback = function()
        DemoConfig:Save()
    end,
})
Config:Button({
    Title = "Load config",
    Callback = function()
        DemoConfig:Load()
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
| `Midnight` | Navy and deep royal blue |
| `MonokaiPro` | Monokai Pro colors |
| `Mid Summer` | Warm coral and sunset gradients |
| `Lunar Moon` | Monochrome silver gradients |
| `Winter Frost` | Ice-blue gradients |
| `Elegant Night` | Deep violet gradients |
| `Lunar Abyss` | Dark indigo gradients |
| `Lunar Eclipse` | Deep rose and magenta gradients |

---

## License

This project is licensed under the [MIT License](LICENSE).

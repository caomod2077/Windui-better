# 🍃 WindUI-Better

> **An enhanced, modern, community-driven edition of WindUI for Roblox.**  
> Built with customizable 2-column card layouts, frosted glass aesthetics, live FPS/Ping watermarks, in-tab search filtering, interactive notifications, animated icons, and community-requested features.

Original library created by **Footagesus**. Enhanced, maintained, and expanded by **caomod2077** and the WindUI community.

---

## ⚡ Quick Start

```lua
local WindUI = loadstring(game:HttpGet("https://raw.githubusercontent.com/caomod2077/Windui-better/main/windui.lua"))()

local Window = WindUI:CreateWindow({
    Title = "My Script Hub",
    Author = "by Creator",
    Theme = "Dark",
    Size = UDim2.fromOffset(800, 520),
    TwoColumns = true, -- Enable modern 2-column card layout (optional, default is false)
    BackgroundBlur = false, -- Optional; can also be changed at runtime
    Glow = Color3.fromRGB(0, 145, 255), -- Glowing neon window outline
    IconAnimation = "Spin", -- "Spin" | "Pulse" | "Rainbow"
    SidebarBanner = {
        Image = "rbxassetid://133610205520685",
        Height = 75,
        Title = "Hub Version",
    },
})
```

`BackgroundBlur` is disabled by default in the showcase because some users find it distracting. Enable or disable it at runtime without recreating the window:

```lua
Window:SetBackgroundBlur(true)  -- Enable while the window is open
Window:SetBackgroundBlur(false) -- Disable and remove the blur effect
```

---

## 🌟 Visual Feature Showcase

### 1. 🔲 Modern 2-Column Card Layout & Frosted Glass (`TwoColumns`)

- **Universal compatibility**: By default, `TwoColumns = false`, preserving the traditional 1-column layout for existing scripts.
- Set `TwoColumns = true` in `CreateWindow` to automatically alternate sections into Left & Right columns.
- **Frosted Glass Styling**: Cards feature a semi-transparent appearance (`0.93` transparency) with clean outlines, allowing your background gradients to shine through cleanly.
- **Per-Tab Override**: A specific tab can enable or disable 2-column mode:
  ```lua
  local SingleColTab = Window:Tab({ Title = "Info", TwoColumns = false })
  ```
- **Per-Section Override**: Use `FullWidth = true` to force a section to stretch across both columns:
  ```lua
  Tab:Section({ Title = "Full Width Section", FullWidth = true })
  ```
- **Explicit Side Placement**: You can explicitly pin cards to the left or right:
  ```lua
  local LeftCard = Tab:Card({ Title = "Farming", Side = "Left" })
  local RightCard = Tab:Card({ Title = "Combat", Side = "Right" })
  ```

---

### 2. 📊 Live Watermark & Server Stats (`Window:Watermark`)

- Display real-time **FPS**, **Ping (ms)**, and an active online status badge directly on the topbar:
  ```lua
  local watermark = Window:Watermark({
      Title = "Script Hub",
      FPS = true,
      Ping = true,
  })
  watermark:SetTitle("Custom Hub")
  watermark:SetVisible(true)
  ```

---

### 3. 🔍 In-Tab Real-Time Feature Search (`Tab:AddSearch`)

- Add a live search bar inside any tab to filter cards and features dynamically as the user types:
  ```lua
  MainTab:AddSearch("Search features in Main...")
  ```

---

### 4. 🖼️ Sidebar Banners & Tab Banners

- **Sidebar Banner**: Header banner above the sidebar tabs list:
  ```lua
  Window:SetSidebarBanner({
      Image = "rbxassetid://133610205520685",
      Height = 75,
      Title = "Welcome",
  })
  ```
- **In-Tab Banner**: Prominent visual banner at the top of a tab:
  ```lua
  DiscordTab:Banner({
      Image = "rbxassetid://133610205520685",
      Height = 90,
      Title = "Community Server",
      Desc = "Join our Discord for updates, configs, and announcements.",
  })
  ```

---

### 5. 🏷️ Tag Pills on All Controls

- Every control (`Button`, `Toggle`, `Slider`, `Dropdown`, `Input`, `Paragraph`) supports visual tag badges:
  ```lua
  Tab:Toggle({
      Title = "Auto Farm",
      Tag = "HOT",
      TagColor = Color3.fromRGB(249, 115, 22),
      Value = true,
      Callback = function(v) end,
  })
  
  Tab:Button({
      Title = "Instant Kill",
      Tag = "OP",
      TagColor = Color3.fromRGB(239, 68, 68),
      Callback = function() end,
  })
  ```

---

### 6. ⏳ HoldButton Component (Safety Confirmation)

- A safety confirmation button requiring the user to press and hold for $N$ seconds. Features an animated filling green progress bar to prevent accidental clicks:
  ```lua
  Tab:HoldButton({
      Title = "Emergency Server Reset",
      Desc = "Hold 2 seconds to activate",
      HoldTime = 2,
      Tag = "HOLD",
      TagColor = Color3.fromRGB(239, 68, 68),
      Callback = function()
          print("Action confirmed!")
      end,
  })
  ```

---

### 7. 📋 Changelog & ⏱️ Live Event Countdown Components

- **Countdown**: An automated countdown clock (`dd : hh : mm : ss`) that ticks every second and fires a callback upon completion:
  ```lua
  Tab:Countdown({
      Title = "Limited Event Ends In",
      Seconds = 86400, -- Countdown from 24 hours
      OnEnd = function()
          print("Countdown completed!")
      end,
  })
  ```
- **Changelog**: A beautifully formatted update log element with automatic color-coded markers (`+` Green, `-` Red, `*` Blue):
  ```lua
  Tab:Changelog({
      Version = "2.0.0",
      Items = {
          "+ Added 2-column card layout",
          "+ Added HoldButton and Countdown",
          "- Fixed memory leaks in loops",
          "* Upgraded animation smoothness",
      },
  })
  ```

---

### 8. 🔔 Interactive Notifications with Action Buttons & Progress

- `WindUI:Notify` supports interactive action buttons and a live progress bar:
  ```lua
  WindUI:Notify({
      Title = "Update Complete",
      Content = "WindUI Better v2.0 is installed and ready to use.",
      Icon = "sparkles",
      Progress = 0.85, -- 85% progress bar
      Duration = 8,
      Buttons = {
          {
              Title = "Accept",
              Variant = "Primary",
              Callback = function()
                  print("Accepted!")
              end,
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

### 9. 🪟 Interactive Dialogs with Controls

- `Window:Dialog` can host Sliders, Toggles, Dropdowns, and Inputs directly inside popup modals:
  ```lua
  local dlg = Window:Dialog({
      Title = "Interactive Dialog",
      Content = "Tune controls directly inside popup modals:",
      Buttons = {
          { Title = "Apply", Variant = "Primary" },
          { Title = "Cancel", Variant = "Secondary" },
      },
  })
  dlg:Toggle({ Title = "Super Fast Mode", Value = true })
  dlg:Slider({ Title = "Performance Level", Value = 80, Min = 10, Max = 100 })
  dlg:Dropdown({ Title = "Execution Engine", Values = { "Native", "Interpreted", "JIT Fast" }, Value = "JIT Fast" })
  ```

---

### 10. ↔️ Collapsible Sidebar (`Window:ToggleSidebar`)

- Collapse the sidebar into a slim 52px icon mode to give maximum space to the main content:
  ```lua
  Window:ToggleSidebar() -- Toggles between 52px icon mode and 175px expanded mode
  ```

---

### 11. 🔒 Tab Security & Access Gating (`LockedTo` / `HiddenTo` / `Whitelist`)

- Restrict or hide specific tabs by player UserId:
  ```lua
  Window:Tab({
      Title = "Admin & Testing",
      Icon = "shield-alert",
      LockedTo = { 12345678, 87654321 }, -- Tab is locked for other players
      -- Or:
      Whitelist = { 12345678 }, -- Tab is completely invisible to non-whitelisted players
  })
  ```

---

### 12. 🎨 Runtime Theme Overrides (`Window:SetColor`)

- Modify theme accent, background, text, or dialog colors on the fly without declaring a new theme:
  ```lua
  Window:SetColor({
      Accent = Color3.fromRGB(0, 255, 170),
      Background = Color3.fromRGB(15, 18, 24),
  })
  ```

---

### 13. 🔊 Audio Feedback (`Window:EnableSounds`)

- Subtle audio feedback on tab selection and control toggles:
  ```lua
  Window:EnableSounds(true)
  ```

---

### 14. 🔝 Notification Placement & Layer Order

- Shift notification toast position to top-right:
  ```lua
  WindUI:SetNotificationUpper(true)
  ```
- Change DisplayOrder to ensure the UI renders above custom game GUIs:
  ```lua
  WindUI:SetDisplayOrder(999) -- Aliased as SetZIndex and SetLayer
  ```

---

## 📖 Complete Boilerplate Example

```lua
local WindUI = loadstring(game:HttpGet("https://raw.githubusercontent.com/caomod2077/Windui-better/main/windui.lua"))()

local Window = WindUI:CreateWindow({
    Title = "WindUI Feature Demo",
    Author = "Example",
    Folder = "WindUIFeatureDemo",
    Theme = "Mid Summer", -- Also try Lunar Moon, Winter Frost, Elegant Night, Lunar Abyss, Lunar Eclipse
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

## 🎨 Built-in Themes

| Theme Name | Description |
|---|---|
| `Dark` | Standard clean modern dark theme |
| `Obsidian` | Deep charcoal base with electric blue accents |
| `Light` | Clean bright daytime theme |
| `Rose` | Elegant wine & dark rose palette |
| `Plant` | Deep forest green with emerald accents |
| `Red` | Crimson dark theme |
| `Indigo` | Deep indigo blue palette |
| `Sky` | Cyan and navy aesthetic |
| `Violet` | Purple & violet neon look |
| `Amber` | Warm amber gradient theme |
| `Midnight` | Navy and deep royal blue |
| `MonokaiPro` | Developer-favorite Monokai Pro colors |
| `Mid Summer` | Warm coral and sunset gradients |
| `Lunar Moon` | Monochrome silver gradients |
| `Winter Frost` | Cool ice-blue gradients |
| `Elegant Night` | Deep violet gradients |
| `Lunar Abyss` | Dark indigo gradients |
| `Lunar Eclipse` | Deep rose and magenta gradients |

---

## 📄 License

This project is licensed under the [MIT License](LICENSE).  
Feel free to use it in all your scripts, projects, and hubs!

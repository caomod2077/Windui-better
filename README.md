# 🍃 WindUI-Better

> **An enhanced, modern, community-driven edition of WindUI for Roblox.**  
> Built with customizable 2-column card layouts, frosted glass aesthetics, high-density typography, live FPS/Ping watermarks, in-tab search filtering, interactive notifications, animated icons, and community-requested features.

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
    BackgroundBlur = true, -- Automatically blurs background when window is open
    Glow = Color3.fromRGB(0, 145, 255), -- Glowing neon window outline
    IconAnimation = "Spin", -- "Spin" | "Pulse" | "Rainbow"
    SidebarBanner = {
        Image = "rbxassetid://133610205520685",
        Height = 75,
        Title = "Hub Version",
    },
})
```

---

## 🌟 What's New in WindUI-Better

### 1. 🔲 Configurable 2-Column Card Layout (`TwoColumns`)
- **Universal compatibility**: By default, `TwoColumns = false`, preserving the traditional 1-column layout for existing scripts.
- Set `TwoColumns = true` in `CreateWindow` to automatically alternate sections into Left & Right columns.
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

### 2. 💎 Ultra-Subtle Frosted Glass Cards
- Section and Card backgrounds now feature an ultra-clean **frosted glass appearance (`0.93` transparency)** with fine outlines (`0.85` transparency).
- Window gradients and background images shine through smoothly without turning into opaque blocks.

### 3. 📐 High-Density Compact Typography & Padding
- Control titles are set to **14px SemiBold** and descriptions to **12px Medium**.
- Compact internal paddings (8–9px) allow **6 to 9 controls to fit in a single card view** without requiring excessive scrolling.
- Window sizes are balanced for both PC (`720x475` to `880x560`) and mobile devices (automatic safe margin calculation).

### 4. 🚀 Silky Smooth, Non-Intrusive Animations
- Jittery scale oscillations on click have been removed.
- Chevron dropdown arrows now rotate smoothly with clean `Quad Out` easing.
- Interactive **Card Hover Glow** highlights the card border when mouse hovers over the header.

### 5. 📊 Live Watermark & Server Stats (`Window:Watermark`)
- Display real-time **FPS**, **Ping (ms)**, and an active online status badge in the topbar:
  ```lua
  local watermark = Window:Watermark({
      Title = "Script Hub",
      FPS = true,
      Ping = true,
  })
  watermark:SetTitle("Custom Hub")
  watermark:SetVisible(true)
  ```

### 6. 📱 Universal Open Button (PC & Mobile Support)
- The floating **Open Button** works on both PC and mobile:
  ```lua
  Window:EditOpenButton({
      Title = "Open Hub",
      Icon = "rbxassetid://86146615808159",
      CornerRadius = UDim.new(0, 16),
      StrokeThickness = 2,
      Draggable = true,
      Color = ColorSequence.new(Color3.fromRGB(0, 191, 255), Color3.fromRGB(255, 105, 180)),
      Enabled = true,
      OnlyMobile = false, -- false = visible on PC as well!
  })
  -- Or force it anytime via code:
  Window:ForceOpenButton(true)
  ```

### 7. 🔍 In-Tab Real-Time Feature Search (`Tab:AddSearch`)
- Add a live search bar inside any tab to filter cards and features dynamically as the user types:
  ```lua
  MainTab:AddSearch("Search features in Main...")
  ```

### 8. 🖼️ Tab Banners & Sidebar Banners
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

### 9. 🏷️ Tag Pills on All Controls
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

### 10. ⏳ HoldButton Component
- A safety confirmation button requiring the user to press and hold for $N$ seconds. Features a filling green progress bar to prevent accidental clicks:
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

### 11. 📋 Changelog Component
- A beautifully formatted update log element with automatic color-coded markers:
  - `+` = Green (Added)
  - `-` = Red (Removed/Fixed)
  - `*` or `~` = Blue (Changed/Improved)
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

### 12. ⏱️ Live Event Countdown Component
- An automated countdown clock (`dd : hh : mm : ss`) that ticks every second and fires a callback upon completion:
  ```lua
  Tab:Countdown({
      Title = "Limited Event Ends In",
      Seconds = 7200, -- 2 hours from now
      OnEnd = function()
          print("Countdown completed!")
      end,
  })
  ```

### 13. 🔒 Tab Security & Access Gating (`LockedTo` / `HiddenTo` / `Whitelist`)
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

### 14. 🔔 Interactive Notifications with Action Buttons & Progress
- `WindUI:Notify` now supports interactive buttons and progress bars:
  ```lua
  WindUI:Notify({
      Title = "New Update Available",
      Content = "Version 2.0 is out! Do you want to load it now?",
      Icon = "sparkles",
      Progress = 0.75, -- 75% progress bar
      Duration = 10,
      Buttons = {
          {
              Title = "Update Now",
              Variant = "Primary",
              Callback = function()
                  print("Updating...")
              end,
          },
          {
              Title = "Later",
              Variant = "Secondary",
          },
      },
  })
  ```

### 15. 🪟 Interactive Dialogs with Controls
- `Window:Dialog` can now host Sliders, Toggles, Dropdowns, and Inputs inside popup modals:
  ```lua
  local dlg = Window:Dialog({
      Title = "Quick Configuration",
      Content = "Tune settings directly from this popup:",
      Buttons = {
          { Title = "Done", Variant = "Primary" },
      },
  })
  dlg:Toggle({ Title = "Fast Mode", Value = true })
  dlg:Slider({ Title = "Speed", Value = 50, Min = 10, Max = 100 })
  dlg:Dropdown({ Title = "Mode", Values = { "Safe", "Aggressive" }, Value = "Safe" })
  ```

### 16. 🎨 Runtime Theme Overrides (`Window:SetColor`)
- Modify theme accent, background, text, or dialog colors on the fly without declaring a new theme:
  ```lua
  Window:SetColor({
      Accent = Color3.fromRGB(0, 255, 170),
      Background = Color3.fromRGB(15, 18, 24),
  })
  ```

### 17. ↔️ Collapsible Sidebar (`Window:ToggleSidebar`)
- Collapse the sidebar into a slim 52px icon mode to give maximum space to the main content:
  ```lua
  Window:ToggleSidebar() -- Toggles between 52px icon mode and 175px expanded mode
  ```

### 18. 🔊 Sound Effects (`Window:EnableSounds`)
- Subtle audio feedback on tab selection and control toggles:
  ```lua
  Window:EnableSounds(true)
  ```

### 19. 🔝 Notification Placement & Layer Order
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
    Title = "Ultimate Hub",
    Author = "by Developer",
    Theme = "Dark",
    Size = UDim2.fromOffset(840, 540),
    TwoColumns = true, -- Modern 2-column layout
    BackgroundBlur = true,
    Glow = Color3.fromRGB(0, 145, 255),
    Icon = "rbxassetid://82225829203828",
    IconAnimation = "Spin",
    SidebarBanner = {
        Image = "rbxassetid://133610205520685",
        Height = 75,
        Title = "v2.0 Community",
    },
})

-- Topbar Watermark
local watermark = Window:Watermark({
    Title = "Ultimate Hub",
    FPS = true,
    Ping = true,
})

-- Floating Open Button for PC & Mobile
Window:EditOpenButton({
    Title = "Open Hub",
    Icon = "rbxassetid://86146615808159",
    CornerRadius = UDim.new(0, 16),
    StrokeThickness = 2,
    Draggable = true,
    Color = ColorSequence.new(Color3.fromRGB(0, 191, 255), Color3.fromRGB(255, 105, 180)),
    Enabled = true,
    OnlyMobile = false,
})

-- Navigation Tabs
local MainTab = Window:Tab({ Title = "Main", Icon = "terminal" })
local UpdatesTab = Window:Tab({ Title = "Updates", Icon = "zap" })
local SettingsTab = Window:Tab({ Title = "Settings", Icon = "settings" })

-- In-Tab Real-time Search
MainTab:AddSearch("Search features...")

-- Left Column Card
local FarmCard = MainTab:Card({
    Title = "Auto Farming",
    Icon = "wheat",
    Side = "Left",
    Badge = "HOT",
    BadgeColor = Color3.fromRGB(249, 115, 22),
})

FarmCard:Toggle({
    Title = "Auto Farm Mobs",
    Desc = "Attacks nearest enemies automatically",
    Tag = "OP",
    TagColor = Color3.fromRGB(239, 68, 68),
    Value = true,
    Callback = function(v)
        print("Auto Farm:", v)
    end,
})

FarmCard:Slider({
    Title = "Attack Distance",
    Value = 25,
    Min = 5,
    Max = 100,
    Callback = function(v)
        print("Distance:", v)
    end,
})

FarmCard:HoldButton({
    Title = "Emergency Reset",
    Desc = "Hold 2s to clear all aggro",
    HoldTime = 2,
    Callback = function()
        Window:Toast({ Title = "Reset", Content = "Aggro cleared!" })
    end,
})

-- Right Column Card
local PlayerCard = MainTab:Card({
    Title = "Player Modifications",
    Icon = "user",
    Side = "Right",
})

PlayerCard:Toggle({
    Title = "Infinite Stamina",
    Value = true,
})

PlayerCard:Dropdown({
    Title = "Speed Mode",
    Values = { "Normal", "Fast", "Insane" },
    Value = "Fast",
})

-- Countdown in Updates Tab
UpdatesTab:Countdown({
    Title = "Season 2 Starts In",
    Seconds = 86400,
})

UpdatesTab:Changelog({
    Version = "2.0.0",
    Items = {
        "+ Modern 2-column card layout",
        "+ HoldButton and Countdown elements",
        "+ Tag pills on all controls",
        "- Fixed window scaling bugs",
    },
})

-- Settings Tab
local InterfaceSection = SettingsTab:Section({ Title = "Interface Controls" })

InterfaceSection:Button({
    Title = "Toggle Sidebar Mode",
    Callback = function()
        Window:ToggleSidebar()
    end,
})

InterfaceSection:Button({
    Title = "Change Accent Color",
    Callback = function()
        Window:SetColor({ Accent = Color3.fromRGB(0, 255, 170) })
    end,
})
```

---

## 🎨 Built-in Themes

| Theme Name | Description |
|---|---|
| `Dark` | Standard clean modern dark theme |
| `Obsidian` | Deep charcoal base with electric blue accents |
| `BigFroot` | Warm dark obsidian with vibrant orange accents |
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

---

## 📄 License

This project is licensed under the [MIT License](LICENSE).  
Feel free to use it in all your scripts, projects, and hubs!


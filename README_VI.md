# WindUI-Better

[English](README.md) | [Tiếng Việt](README_VI.md)

Phiên bản WindUI mở rộng cho Roblox, hỗ trợ giao diện 2 cột dạng Card, tìm kiếm tính năng trực tiếp trong Tab, tích hợp sẵn các theme Gradient, hiển thị Watermark (FPS / Ping), phân quyền truy cập Tab theo UserId và tùy biến giao diện lúc đang chạy.

Thư viện gốc bởi Footagesus. Bản mở rộng và duy trì bởi caomod2077.

---

## Cài đặt và Khởi tạo nhanh

```lua
local WindUI = loadstring(game:HttpGet("https://raw.githubusercontent.com/caomod2077/Windui-better/main/windui.lua"))()

local Window = WindUI:CreateWindow({
    Title = "My Script Hub",
    Author = "by Creator",
    Theme = "Dark",
    Size = UDim2.fromOffset(800, 520),
    TwoColumns = true, -- Bật bố cục 2 cột (mặc định: false)
    BackgroundBlur = false, -- Làm mờ hậu cảnh (mặc định: false)
    Glow = Color3.fromRGB(0, 145, 255),
    IconAnimation = "Spin", -- "Spin" | "Pulse" | "Rainbow"
    SidebarBanner = {
        Image = "rbxassetid://133610205520685",
        Height = 75,
        Title = "Hub Version",
    },
})
```

Làm mờ hậu cảnh (BackgroundBlur) có thể bật hoặc tắt trực tiếp lúc đang chạy mà không cần tạo lại cửa sổ:

```lua
Window:SetBackgroundBlur(true)  -- Bật làm mờ
Window:SetBackgroundBlur(false) -- Tắt làm mờ
```

---

## Chi tiết các tính năng và API

### Bố cục 2 cột dạng Card (TwoColumns)

Mặc định `TwoColumns = false` sẽ giữ giao diện 1 cột truyền thống. Khi đặt `TwoColumns = true` trong `CreateWindow`, các section sẽ tự động chia đều sang 2 cột trái và phải.

- Tắt hoặc bật 2 cột cho riêng từng Tab:
  ```lua
  local SingleColTab = Window:Tab({ Title = "Info", TwoColumns = false })
  ```
- Cho phép 1 Section chiếm trọn chiều ngang cả 2 cột:
  ```lua
  Tab:Section({ Title = "Full Width Section", FullWidth = true })
  ```
- Ghim Card cố định sang cột trái hoặc phải:
  ```lua
  local LeftCard = Tab:Card({ Title = "Farming", Side = "Left" })
  local RightCard = Tab:Card({ Title = "Combat", Side = "Right" })
  ```

---

### Watermark và thông số hệ thống (Window:Watermark)

Hiển thị FPS, Ping (ms) và trạng thái trực tiếp trên thanh tiêu đề:

```lua
local watermark = Window:Watermark({
    Title = "Script Hub",
    FPS = true,
    Ping = true,
})

watermark:SetTitle("Tên Mới")
watermark:SetVisible(true)
```

---

### Tìm kiếm tính năng trong Tab (Tab:AddSearch)

Thêm thanh tìm kiếm theo thời gian thực vào bất kỳ Tab nào để lọc Card và nút bấm:

```lua
MainTab:AddSearch("Tìm kiếm tính năng...")
```

---

### Banner Sidebar và Banner Tab

- Banner trên đầu danh sách Tab ở Sidebar:
  ```lua
  Window:SetSidebarBanner({
      Image = "rbxassetid://133610205520685",
      Height = 75,
      Title = "Welcome",
  })
  ```
- Banner trên đầu một Tab cụ thể:
  ```lua
  DiscordTab:Banner({
      Image = "rbxassetid://133610205520685",
      Height = 90,
      Title = "Máy chủ Discord",
      Desc = "Tham gia server để cập nhật script và cấu hình.",
  })
  ```

---

### Tag badge trên các control

Tất cả các thành phần UI (`Button`, `Toggle`, `Slider`, `Dropdown`, `Input`, `Paragraph`) đều hỗ trợ gắn nhãn (Tag badge):

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

### Nút giữ để xác nhận (HoldButton)

Yêu cầu người dùng nhấn và giữ trong khoảng thời gian nhất định (giây) kèm thanh tiến trình để tránh bấm nhầm:

```lua
Tab:HoldButton({
    Title = "Reset Khẩn Cấp",
    Desc = "Giữ 2 giây để xác nhận",
    HoldTime = 2,
    Tag = "HOLD",
    TagColor = Color3.fromRGB(239, 68, 68),
    Callback = function()
        print("Đã xác nhận thao tác")
    end,
})
```

---

### Changelog và đếm ngược sự kiện (Countdown)

- Đồng hồ đếm ngược tự động (`ngày : giờ : phút : giây`):
  ```lua
  Tab:Countdown({
      Title = "Sự kiện kết thúc sau",
      Seconds = 86400,
      OnEnd = function()
          print("Sự kiện đã kết thúc")
      end,
  })
  ```
- Bảng nhật ký cập nhật (đánh dấu màu tự động: `+` xanh lá, `-` đỏ, `*` xanh dương):
  ```lua
  Tab:Changelog({
      Version = "2.0.0",
      Items = {
          "+ Thêm giao diện 2 cột dạng Card",
          "+ Thêm HoldButton và Countdown",
          "- Khắc phục leak bộ nhớ",
          "* Nâng cấp chuyển động",
      },
  })
  ```

---

### Thông báo tương tác (WindUI:Notify)

Thông báo toast có thanh tiến trình và các nút bấm tương tác:

```lua
WindUI:Notify({
    Title = "Cập nhật thành công",
    Content = "WindUI đã sẵn sàng sử dụng.",
    Icon = "sparkles",
    Progress = 0.85,
    Duration = 8,
    Buttons = {
        {
            Title = "Đồng ý",
            Variant = "Primary",
            Callback = function() end,
        },
        {
            Title = "Đóng",
            Variant = "Secondary",
            Callback = function() end,
        },
    },
})
```

---

### Hộp thoại Dialog tích hợp control (Window:Dialog)

Hộp thoại modal có thể chứa trực tiếp các control như Toggle, Slider, Dropdown, Input:

```lua
local dlg = Window:Dialog({
    Title = "Xác nhận cài đặt",
    Content = "Tùy chỉnh thông số trước khi tiếp tục:",
    Buttons = {
        { Title = "Áp dụng", Variant = "Primary" },
        { Title = "Hủy", Variant = "Secondary" },
    },
})

dlg:Toggle({ Title = "Chế độ nhanh", Value = true })
dlg:Slider({ Title = "Tốc độ", Value = 80, Min = 10, Max = 100 })
dlg:Dropdown({ Title = "Mức độ", Values = { "Thấp", "Trung bình", "Cao" }, Value = "Cao" })
```

---

### Thu gọn Sidebar (Window:ToggleSidebar)

Thu nhỏ thanh Sidebar về chế độ icon gọn gàng (52px) hoặc mở rộng đầy đủ (175px):

```lua
Window:ToggleSidebar()       -- Chuyển đổi trạng thái
Window:ToggleSidebar(true)   -- Buộc thu gọn
Window:ToggleSidebar(false)  -- Buộc mở rộng
```

---

### Phân quyền truy cập Tab (LockedTo / Whitelist)

Khóa hoặc ẩn Tab theo UserId của người chơi:

```lua
-- Khóa Tab (vẫn thấy nhưng hiển thị màn hình khóa yêu cầu quyền)
Window:Tab({
    Title = "Admin",
    Icon = "shield-alert",
    LockedTo = { 12345678, 87654321 },
})

-- Ẩn hoàn toàn Tab đối với người không có trong danh sách
Window:Tab({
    Title = "Riêng tư",
    Icon = "lock",
    Whitelist = { 12345678 },
})
```

---

### Ghi đè màu Theme khi đang chạy (Window:SetColor)

Đổi màu Accent, Background hoặc Text ngay trong lúc chạy mà không cần tạo theme mới:

```lua
Window:SetColor({
    Accent = Color3.fromRGB(0, 255, 170),
    Background = Color3.fromRGB(15, 18, 24),
})
```

---

### Hiệu ứng âm thanh (Window:EnableSounds)

Bật âm thanh phản hồi khi chuyển Tab hoặc tương tác với control:

```lua
Window:EnableSounds(true)
```

---

### Vị trí thông báo và Thứ tự hiển thị (DisplayOrder / ZIndex)

- Đặt thông báo hiển thị ở góc trên bên phải thay vì góc dưới:
  ```lua
  WindUI:SetNotificationUpper(true)
  ```
- Đặt DisplayOrder để UI luôn hiển thị đè lên các GUI khác trong game:
  ```lua
  WindUI:SetDisplayOrder(999) -- Hỗ trợ alias SetZIndex và SetLayer
  ```

---

## Code mẫu đầy đủ (Boilerplate Example)

```lua
local WindUI = loadstring(game:HttpGet("https://raw.githubusercontent.com/caomod2077/Windui-better/main/windui.lua"))()

local Window = WindUI:CreateWindow({
    Title = "WindUI Feature Demo",
    Author = "Example",
    Folder = "WindUIFeatureDemo",
    Theme = "Dark", -- Có thể thử các theme: Lunar Moon, Winter Frost, Elegant Night, Lunar Abyss, Lunar Eclipse
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
local MainTab = Window:Tab({ Title = "Tính năng", Icon = "terminal" })
local UpdatesTab = Window:Tab({ Title = "Cập nhật", Icon = "zap" })
local SettingsTab = Window:Tab({ Title = "Cài đặt", Icon = "settings" })

MainTab:AddSearch("Tìm kiếm tính năng...")

local Controls = MainTab:Card({
    Title = "Điều khiển",
    Icon = "sliders-horizontal",
    Side = "Left",
    Badge = "DEMO",
    BadgeColor = Color3.fromRGB(255, 126, 95),
})

Controls:Toggle({
    Title = "Bật chức năng",
    Desc = "Ví dụ callback cho toggle",
    Tag = "LIVE",
    Value = false,
    Callback = function(value)
        print("Trạng thái:", value)
    end,
})

Controls:Slider({
    Title = "Cường độ",
    Value = 35,
    Min = 0,
    Max = 100,
    Callback = function(value)
        print("Cường độ:", value)
    end,
})

Controls:Dropdown({
    Title = "Chế độ",
    Values = { "Cân bằng", "Nhanh", "Chính xác" },
    Value = "Cân bằng",
    Callback = function(value)
        print("Chế độ:", value)
    end,
})

Controls:Input({
    Title = "Nhập văn bản",
    Placeholder = "Nhập nội dung...",
    Callback = function(value)
        print("Nội dung:", value)
    end,
})

Controls:Button({
    Title = "Hiện thông báo",
    Callback = function()
        WindUI:Notify({ Title = "WindUI", Content = "Đã nhấn nút", Duration = 4 })
    end,
})

Controls:HoldButton({
    Title = "Giữ để xác nhận",
    HoldTime = 1.5,
    Callback = function()
        WindUI:Notify({ Title = "Xác nhận", Content = "Đã hoàn thành giữ", Duration = 3 })
    end,
})

local Info = MainTab:Card({ Title = "Trạng thái", Icon = "activity", Side = "Right" })
Info:Paragraph({ Title = "Sẵn sàng", Content = "Card độc lập không phụ thuộc logic game." })
Info:Keybind({ Title = "Phím tắt đóng mở", Value = "RightControl" })

UpdatesTab:Countdown({ Title = "Đếm ngược demo", Seconds = 3600 })
UpdatesTab:Changelog({
    Version = "1.0.0",
    Items = { "+ Tích hợp sẵn các custom theme", "+ Tìm kiếm và card 2 cột", "+ Tùy biến giao diện runtime" },
})

local Interface = SettingsTab:Section({ Title = "Giao diện" })
Interface:Dropdown({
    Title = "Theme",
    Values = { "Dark", "Obsidian", "Light", "Mid Summer", "Lunar Moon", "Winter Frost", "Elegant Night", "Lunar Abyss", "Lunar Eclipse" },
    Value = "Mid Summer",
    Callback = function(theme)
        WindUI:SetTheme(theme)
    end,
})
Interface:Toggle({
    Title = "Làm mờ hậu cảnh",
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
    Title = "Thu gọn Sidebar",
    Value = false,
    Callback = function(value)
        Window:ToggleSidebar(value)
    end,
})
Interface:Button({
    Title = "Đổi màu Accent",
    Callback = function()
        Window:SetColor({ Accent = Color3.fromRGB(0, 200, 160) })
    end,
})
Interface:Button({
    Title = "Mở hộp thoại Dialog",
    Callback = function()
        Window:Dialog({
            Title = "Hộp thoại mẫu",
            Content = "Có thể đặt các control tùy chỉnh tại đây.",
            Buttons = {
                { Title = "Đóng", Callback = function() end },
            },
        })
    end,
})

local Config = SettingsTab:Section({ Title = "Cấu hình" })
local ConfigManager = Window.ConfigManager
local DemoConfig = ConfigManager:CreateConfig("demo")
Config:Button({
    Title = "Lưu cấu hình",
    Callback = function()
        DemoConfig:Save()
    end,
})
Config:Button({
    Title = "Tải cấu hình",
    Callback = function()
        DemoConfig:Load()
    end,
})
```

---

## Danh sách Theme có sẵn

| Tên Theme | Mô tả |
|---|---|
| `Dark` | Giao diện tối tiêu chuẩn |
| `Obsidian` | Nền than tối kết hợp màu cam |
| `Light` | Giao diện sáng ban ngày |
| `Rose` | Tông màu rượu vang và cánh hồng tối |
| `Plant` | Tông xanh lục rừng rậm và ngọc lục bảo |
| `Red` | Giao diện tối màu đỏ thẫm |
| `Indigo` | Tông màu xanh chàm sâu |
| `Sky` | Tông xanh ngọc bích và xanh hải quân |
| `Violet` | Tông màu tím violet |
| `Amber` | Tông màu hổ phách ấm áp |
| `Midnight` | Xanh bóng đêm và xanh hoàng gia |
| `MonokaiPro` | Bộ màu Monokai Pro lập trình viên |
| `Mid Summer` | Gradient hoàng hôn và màu san hô ấm |
| `Lunar Moon` | Gradient màu bạc đơn sắc |
| `Winter Frost` | Gradient màu băng giá |
| `Elegant Night` | Gradient màu tím đêm sang trọng |
| `Lunar Abyss` | Gradient màu chàm vũ trụ tối |
| `Lunar Eclipse` | Gradient màu hoa hồng và đỏ thẫm |

---

## Giấy phép (License)

Dự án phát hành theo giấy phép [MIT License](LICENSE).

# WindUI-Better

[English](README.md) | [Tiếng Việt](README_VI.md)

Phiên bản WindUI mở rộng cho Roblox, hỗ trợ giao diện 2 cột dạng Card, tìm kiếm tính năng trực tiếp trong Tab, tích hợp sẵn các theme Gradient, hiển thị Watermark (FPS / Ping), phân quyền truy cập Tab theo UserId, khung hiển thị mô hình 3D tương tác (Viewport) và tối ưu hóa hiệu năng giảm giật lag.

Thư viện gốc bởi Footagesus. Bản mở rộng và duy trì bởi caomod2077.

---

## Các cải tiến tối ưu hiệu năng (Performance Optimizations)

Nhiều người dùng gặp hiện tượng tụt FPS hoặc đơ giao diện khi dùng WindUI trên các executor. Bản WindUI-Better đã được tinh chỉnh lại các điểm nghẽn hiệu năng sau:

1. Tự động dọn dẹp đối tượng rác (Dead Object Pruning): Toàn bộ đối tượng theme và tín hiệu kết nối gắn với phần tử UI bị xóa (destroy) sẽ được tự động giải phóng khỏi bảng lưu trữ toàn cục, chấm dứt tình trạng rò rỉ bộ nhớ (memory leak).
2. Tối ưu tính toán kích thước (Memoized Shape Calculations): Khung Squircle và viền Glass chỉ tính toán lại ma trận bo góc khi kích thước pixel thực sự thay đổi, loại bỏ hàng nghìn phép toán vector dư thừa mỗi khung hình.
3. Đo FPS / Ping không phân mảnh bộ nhớ (Zero-Allocation Watermark): Bỏ việc chèn và xóa mảng (`table.insert`/`table.remove`) liên tục mỗi frame, thay bằng bộ đếm tích lũy thời gian đơn giản không cấp phát mảng, giúp bộ dọn rác (GC) của Lua không bị quá tải.
4. Tự ngắt vòng lặp khi ẩn giao diện: Hoạt ảnh xoay icon, chuyển màu cầu vồng và vòng lặp watermark sẽ tạm dừng hoàn toàn khi đóng cửa sổ hoặc ẩn phần tử, nhường 100% tài nguyên CPU cho game.
5. Kiểm soát vòng đời vòng lặp nền: Các tác vụ đếm ngược (Countdown) tự động ngắt kết nối khi component cha bị gỡ bỏ.

---

## Cài đặt và Khởi tạo nhanh

```lua
local WindUI = loadstring(game:HttpGet("https://raw.githubusercontent.com/caomod2077/Windui-better/main/windui.lua"))()

local Window = WindUI:CreateWindow({
    Title = "My Script Hub",
    Author = "by Creator",
    Folder = "MyScriptHub",
    Theme = "Dark",
    Size = UDim2.fromOffset(840, 540),
    TwoColumns = true,      -- Bật bố cục 2 cột (mặc định: false)
    BackgroundBlur = false,  -- Làm mờ hậu cảnh (mặc định: false)
    Glow = Color3.fromRGB(0, 145, 255),
    IconAnimation = "Spin", -- "Spin" | "Pulse" | "Rainbow"
    SidebarBanner = {
        Image = "rbxassetid://133610205520685",
        Height = 75,
        Title = "Hub Version",
    },
})
```

Làm mờ hậu cảnh (BackgroundBlur) có thể bật hoặc tắt trực tiếp lúc đang chạy:

```lua
Window:SetBackgroundBlur(true)
Window:SetBackgroundBlur(false)
```

---

## Tài liệu API chi tiết (A-Z)

### 1. Tùy chọn khi khởi tạo cửa sổ (WindUI:CreateWindow)

| Tham số | Kiểu dữ liệu | Mặc định | Mô tả chi tiết |
|---|---|---|---|
| `Title` | string | `"UI Library"` | Tiêu đề chính hiển thị trên thanh topbar |
| `Author` | string | `nil` | Phụ đề hoặc tên tác giả bên cạnh tiêu đề |
| `Folder` | string | `nil` | Tên thư mục con lưu config bên trong thư mục `WindUI/` |
| `Theme` | string | `"Dark"` | Tên theme khởi đầu từ danh sách có sẵn hoặc tự thêm |
| `Size` | UDim2 | `UDim2.fromOffset(720, 470)` | Kích thước ban đầu của cửa sổ |
| `MinSize` | Vector2 | `Vector2.new(460, 320)` | Kích thước thu nhỏ tối thiểu |
| `MaxSize` | Vector2 | `Vector2.new(2560, 1600)` | Kích thước phóng to tối đa |
| `TwoColumns` | boolean | `false` | Tự động chia các Card/Section thành 2 cột trái và phải |
| `BackgroundBlur` | boolean | `false` | Bật hiệu ứng làm mờ hậu cảnh (BlurEffect trong Lighting) |
| `Glow` | Color3 | `nil` | Màu viền phát sáng neon bao quanh cửa sổ |
| `Icon` | string | `nil` | Icon trên topbar (rbxassetid hoặc tên Lucide icon) |
| `IconSize` | number | `22` | Kích thước pixel của icon topbar |
| `IconAnimation` | string | `nil` | Hiệu ứng chuyển động icon: `"Spin"`, `"Pulse"`, hoặc `"Rainbow"` |
| `SidebarBanner` | table | `nil` | Banner nằm ở đầu thanh Sidebar: `{ Image, Height, Title }` |
| `SideBarWidth` | number | `175` | Độ rộng của thanh điều hướng Sidebar |
| `HideSearchBar` | boolean | `false` | Ẩn thanh tìm kiếm tab trên cùng của Sidebar |
| `ScrollBarEnabled` | boolean | `false` | Hiển thị thanh cuộn tùy chỉnh |
| `Resizable` | boolean | `true` | Cho phép kéo góc dưới bên phải để thay đổi kích thước |
| `Transparent` | boolean | `false` | Độ trong suốt của thân cửa sổ |
| `Acrylic` | boolean | `false` | Bật hiệu ứng kính mờ Acrylic |
| `AutoScale` | boolean | `true` | Tự động co giãn kích thước phù hợp với màn hình mobile |
| `ToggleKey` | Enum.KeyCode | `nil` | Phím tắt bàn phím để ẩn hoặc hiện cửa sổ |
| `User` | table | `nil` | Khung thông tin người chơi ở góc dưới Sidebar |
| `Topbar` | table | `{ Height = 52 }` | Tùy chọn topbar: `{ Height = 52, ButtonsType = "Default" / "Mac" }` |
| `Background` | string | `nil` | Hình nền cửa sổ (assetid, link ảnh URL hoặc `video:link.webm`) |
| `BackgroundImageTransparency` | number | `0` | Độ trong suốt ảnh nền (0 đến 1) |
| `KeySystem` | table | `nil` | Hệ thống kiểm tra Key bản quyền trước khi mở UI |

---

### 2. Các phương thức của Cửa sổ (Window Methods)

- `Window:Tag({ Title, Color, Radius, Border })`: Thêm nhãn tag lên thanh topbar.
- `Window:Watermark({ Title, FPS = true, Ping = true })`: Tạo khung hiển thị FPS / Ping trên topbar. Trả về đối tượng `{ SetTitle(text), SetVisible(bool), Destroy() }`.
- `Window:SetBackgroundBlur(boolean)`: Bật hoặc tắt làm mờ hậu cảnh lúc đang chạy.
- `Window:ToggleSidebar(forceState)`: Đổi trạng thái thu gọn Sidebar dạng icon (52px) hoặc mở rộng (175px).
- `Window:SetColor({ Accent, Background, Text, Dialog, ... })`: Đổi màu trực tiếp cho theme đang chạy.
- `Window:SetUIScale(number)`: Điều chỉnh tỷ lệ thu phóng toàn bộ giao diện (ví dụ `0.85` cho mobile).
- `Window:SetToggleKey(Enum.KeyCode)`: Đổi phím tắt ẩn hiện cửa sổ.
- `Window:SetSize(UDim2)`: Đổi kích thước cửa sổ bằng mã.
- `Window:SetToTheCenter()`: Canh cửa sổ vào chính giữa màn hình.
- `Window:SetBackgroundImage(string)`: Đổi ảnh nền cửa sổ (assetid hoặc link ảnh trực tiếp).
- `Window:SetBackgroundImageTransparency(number)`: Đổi độ trong suốt của ảnh nền (0 đến 1).
- `Window:SetIconAnimation("Spin" | "Pulse" | "Rainbow")`: Đổi hoặc kích hoạt hoạt ảnh cho icon topbar.
- `Window:AnimateTitle("Gradient" | "Shimmer")`: Bật dải màu chuyển động lướt qua tiêu đề.
- `Window:SetSidebarBanner({ Image, Height, Title })`: Cập nhật banner trên Sidebar.
- `Window:Toast({ Title, Content, Duration, Icon })`: Hiện nhanh một thông báo ngắn.
- `Window:Dialog({ Title, Content, Buttons })`: Mở hộp thoại modal popup, có thể chứa trực tiếp các control bên trong.
- `Window:EnableSounds(boolean)`: Bật hoặc tắt âm thanh khi click nút và chuyển tab.
- `Window:PlaySound(soundType)`: Phát âm thanh thủ công (`"tab"` hoặc `"toggle"`).
- `Window:EditOpenButton(table)`: Tùy chỉnh nút bấm mở GUI nổi trên màn hình cho mobile.
- `Window:Section({ Title, Icon, Opened })`: Tạo nhóm phân mục trên Sidebar để gom các Tab.
- `Window:Divider()`: Thêm đường kẻ phân cách trong Sidebar.
- `Window:OnOpen(fn)`, `Window:OnClose(fn)`, `Window:OnDestroy(fn)`: Bắt sự kiện khi cửa sổ đóng, mở hoặc bị xóa.
- `Window.ConfigManager`:
  - `ConfigManager:CreateConfig(name)`: Tạo file config. Đối tượng trả về có: `:Save()`, `:Load()`, `:Delete()`, `:Set(k, v)`, `:Get(k)`.
  - `ConfigManager:AllConfigs()`: Lấy danh sách tên các config đã lưu.
  - `ConfigManager:DeleteConfig(name)`: Xóa một file config.

---

### 3. Các phương thức toàn cục (WindUI)

- `WindUI:CreateWindow(config)`: Khởi tạo và hiển thị cửa sổ.
- `WindUI:Notify({ Title, Content, Icon, Duration, Progress, Buttons })`: Hiển thị thông báo góc màn hình có nút bấm và thanh tiến trình.
- `WindUI:Popup({ Title, Content, Icon, Thumbnail, Buttons })`: Hiển thị popup xác nhận chính giữa màn hình.
- `WindUI:SetNotificationUpper(boolean)`: Đặt thông báo nằm ở góc trên bên phải (`true`) hoặc góc dưới bên phải (`false`).
- `WindUI:SetNotificationLower(boolean)`: Hạ thấp vị trí thông báo để tránh cấn các thanh GUI game.
- `WindUI:SetDisplayOrder(number)`: Đặt thứ tự hiển thị ScreenGui lên trên các GUI khác trong game (hỗ trợ alias `SetZIndex`, `SetLayer`).
- `WindUI:SetTheme(name)`: Áp dụng theme trên toàn bộ thư viện.
- `WindUI:GetThemes()`: Trả về bảng danh sách tất cả theme có sẵn.
- `WindUI:GetCurrentTheme()`: Trả về chuỗi tên theme hiện tại.
- `WindUI:AddTheme(themeTable)`: Đăng ký một bảng theme màu sắc tùy chỉnh mới.
- `WindUI:Gradient(keypoints, options)`: Hàm tiện ích tạo dải màu Gradient xoay góc cho theme.
- `WindUI:SetFont(fontAssetId)`: Đổi phông chữ hiển thị toàn cục.
- `WindUI:SetLanguage(langCode)`: Đổi ngôn ngữ bản địa hóa (localization).
- `WindUI:ToggleAcrylic(boolean)`: Bật hoặc tắt hiệu ứng kính mờ Acrylic toàn cục.

---

### 4. Cấu trúc Tab và Section

- `Window:Tab({ Title, Icon, Desc, TwoColumns, LockedTo, HiddenTo, Whitelist, ShowTabTitle, IconShape, IconColor, CustomEmptyPage })`: Tạo tab mới.
  - `LockedTo = { userId, ... }`: Khóa tab và hiện giao diện yêu cầu phân quyền đối với ai không có trong danh sách.
  - `Whitelist = { userId, ... }`: Ẩn hoàn toàn tab khỏi danh sách đối với người không có quyền.
  - `IconShape = "Circle"`: Đặt khung bo tròn bao quanh icon tab.
  - `IconColor = Color3.fromRGB(...)`: Tô màu riêng cho icon của tab.
  - `CustomEmptyPage = { Icon, IconSize, Title, Desc }`: Giao diện hiển thị khi tab chưa có phần tử nào.
- `Tab:AddSearch(placeholder)`: Nhúng thanh tìm kiếm lọc tính năng theo thời gian thực vào tab.
- `Tab:Banner({ Image, Height, Title, Desc })`: Tạo banner hình ảnh ở đầu tab.
- `Tab:Card({ Title, Icon, Desc, Side = "Left" | "Right", FullWidth, Badge, BadgeColor })`: Khung card chứa control (alias: `Section`, `Groupbox`).
- `Section:AddBadge(text, color)`: Gắn huy hiệu nhỏ lên thanh tiêu đề của card.
- `Section:AddHeaderButton(icon, callback)`: Thêm nút icon thao tác nhanh trên thanh tiêu đề của card.
- `Section:Flash(color)`: Hiệu ứng nhấp nháy viền card để gây chú ý.
- `Section:Open()`, `Section:Close()`: Mở hoặc đóng nội dung card thu gọn.
- `Tab:Column("Left" | "Right")`: Lấy đối tượng cột để đặt card cố định vào bên trái hoặc phải.
- `Tab:AddPage(title, icon)` / `Tab:SubPage`: Tạo trang con lồng bên trong tab.
- `Tab:CollapseAll()`, `Tab:ExpandAll()`: Thu gọn hoặc mở rộng toàn bộ các card trong tab.
- `Tab:LockAll()`, `Tab:UnlockAll()`: Khóa hoặc mở khóa tất cả phần tử trong tab.

---

### 5. Danh mục các thành phần UI (Controls & Elements)

Có thể tạo trên `Tab`, `Card`, `Section`, `Column` hoặc trong `Dialog`:

- `Button({ Title, Desc, Icon, Callback, Color, Justify, IconAlign, Locked, LockedTitle })`: Nút bấm kích hoạt hàm.
- `Toggle({ Title, Desc, Value, Callback, Type = "Toggle" | "Checkbox", Icon, Locked })`: Công tắc bật tắt hoặc ô kiểm checkbox.
  - Hàm điều khiển: `:Set(boolean)`
- `Slider({ Title, Desc, Value = { Min, Max, Default }, Step, Callback, IsTextbox, IsTooltip, Icons = { From, To }, Width, Locked })`: Thanh kéo chọn giá trị số, hỗ trợ ô nhập text và bong bóng tooltip.
  - Hàm điều khiển: `:Set(number)`, `:SetMin(number)`, `:SetMax(number)`
- `Dropdown({ Title, Desc, Values, Value, Multi, AllowNone, SearchBarEnabled, MenuWidth, Callback, Locked })`: Menu thả xuống chọn 1 hoặc nhiều mục, hỗ trợ tìm kiếm mục chọn.
  - Hàm điều khiển: `:Select(value)`, `:Refresh(values)`, `:Open()`, `:Close()`
- `Input({ Title, Desc, Placeholder, Value, ClearTextOnFocus, Type = "Input" | "Textarea", InputIcon, Callback, Locked })`: Ô nhập văn bản một dòng hoặc nhiều dòng (Textarea).
  - Hàm điều khiển: `:Set(text)`, `:SetPlaceholder(text)`
- `Keybind({ Title, Desc, Value, CanChange, Blacklist, Callback, Locked })`: Nút gán phím tắt bàn phím hoặc chuột.
  - Hàm điều khiển: `:Set(keyName)`
- `Colorpicker({ Title, Desc, Default, Transparency, Callback, Locked })`: Bảng chọn màu HSV / RGB / Hex kèm thanh chỉnh độ mờ đục.
  - Hàm điều khiển: `:Set(color3, transparency)`
- `HoldButton({ Title, Desc, HoldTime, Tag, TagColor, Callback })`: Nút giữ an toàn, yêu cầu đè chuột hoặc cảm ứng trong một số giây nhất định để kích hoạt.
- `ProgressBar({ Title, Desc, Value = { Min, Max, Default }, ShowValue, DisplayMode = "Percent" | "Value" | "Fraction", Format, Animate, Indeterminate, Speed })`: Thanh hiển thị tiến độ hoàn thành hoặc hiệu ứng xoay tải vô tận.
  - Hàm điều khiển: `:Set(number)`, `:Get()`, `:GetPercentage()`, `:SetMin(number)`, `:SetMax(number)`
- `Countdown({ Title, Seconds, TargetTime, OnEnd })`: Đồng hồ đếm ngược thời gian thực, tự động chạy callback khi hết giờ.
- `Changelog({ Title, Version, Items = { "+ Thêm", "- Xóa", "* Sửa" } })`: Khung ghi chú cập nhật phiên bản tô màu trực quan.
- `Code({ Title, Code, CodeSize, Height, CodeTheme, CanCopied, OnCopy })`: Khung hiển thị code có highlight cú pháp và nút sao chép nhanh.
  - Hàm điều khiển: `:SetCode(text)`
- `Viewport({ Object, Camera, Height, Interactive, Focused })`: Khung 3D ViewportFrame hiển thị mô hình part, nhân vật, vũ khí trực tiếp trong UI, hỗ trợ kéo chuột xoay 360 độ và phóng to thu nhỏ.
  - Hàm điều khiển: `:SetObject(instance, clone)`, `:Focus()`, `:SetHeight(number)`, `:SetInteractive(boolean)`, `:SetCamera(camera)`
- `Paragraph({ Title, Desc, Image, Thumbnail, Buttons = { { Title, Icon, Variant, Callback } } })`: Đoạn văn bản mô tả kèm nút bấm phụ.
  - Hàm điều khiển: `:SetTitle(text)`, `:SetDesc(text)`, `:SetImage(img)`, `:SetThumbnail(thumb)`
- `Image({ Image, AspectRatio = "16:9", Radius })`: Khung ảnh responsive giữ tỷ lệ khung hình.
- `Divider(title or nil)`: Đường kẻ ngang phân cách, hoặc đường kẻ có chữ tiêu đề ở giữa nếu truyền chuỗi.
- `Space(columns or nil)`: Khoảng cách đệm theo chiều dọc.
- `Group(fn)` / `HStack` / `VStack`: Khung bố cục nhóm phần tử theo chiều ngang hoặc dọc.
- `DiscordCard({ Invite, Title, Desc, Icon, OnJoin, AutoFetch, Side })`: Widget máy chủ Discord tự động lấy số thành viên online.
- `Embed({ Title, Desc, Color, Fields = { { Title, Value } }, Footer })`: Khung tin nhắn dạng Embed phong cách Discord.

---

### 6. Cấu hình hệ thống xác thực Key (KeySystem)

WindUI tích hợp sẵn hộp thoại nhập Key bản quyền trước khi mở cửa sổ chính:

```lua
local Window = WindUI:CreateWindow({
    Title = "Secured Hub",
    Folder = "SecuredHub",
    KeySystem = {
        Title = "Key System",
        Note = "Tham gia server Discord de lay key mien phi.",
        URL = "https://discord.gg/mSrMzVuc3h",
        SaveKey = true,
        Key = "SECRET_ACCESS_KEY",
        KeyValidator = function(enteredKey)
            return enteredKey == "SECRET_ACCESS_KEY"
        end,
        Thumbnail = {
            Image = "rbxassetid://133610205520685",
            Title = "Xac Thuc Ban Quyen",
        },
    },
})
```

---

## Code mẫu đầy đủ (Boilerplate Example)

Mẫu script độc lập dưới đây kết hợp tất cả các thành phần và API của thư viện. Bạn có thể copy và chạy trực tiếp trong Roblox để trải nghiệm toàn bộ tính năng:

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

-- Sidebar Sections grouping Navigation Tabs
local HomeSection = Window:Section({ Title = "Home", Icon = "star", Opened = true })
local ControlsTab = HomeSection:Tab({ Title = "Controls", Icon = "sliders-horizontal" })
local DisplayTab = HomeSection:Tab({ Title = "3D & Display", Icon = "box" })
local CommunityTab = HomeSection:Tab({ Title = "Community", Icon = "users" })

Window:Divider()

local AdvancedSection = Window:Section({ Title = "Advanced", Icon = "box", Opened = true })
local LayoutTab = AdvancedSection:Tab({ Title = "Layout & Groups", Icon = "layout-grid" })
local SecurityTab = AdvancedSection:Tab({
    Title = "Locked Tab",
    Icon = "shield-alert",
    LockedTo = { 12345678, 87654321 },
    Desc = "Locked tab demonstrating LockedTo ID protection.",
})

Window:Divider()

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

-- 3D Viewport Model Viewer
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
    Interactive = true, -- Drag with mouse/touch to rotate 360, scroll/pinch to zoom
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

CommunityTab:Banner({
    Image = "rbxassetid://133610205520685",
    Height = 90,
    Title = "Official Community",
    Desc = "Live announcements, updates, and releases.",
})

CommunityTab:Countdown({
    Title = "Next Release Countdown",
    Seconds = 7200,
    OnEnd = function() end,
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
| `Emerald` | Tông màu xanh ngọc lục bảo đậm |
| `Midnight` | Xanh bóng đêm và xanh hoàng gia |
| `Crimson` | Đỏ thẫm ruby đậm |
| `MonokaiPro` | Bộ màu Monokai Pro lập trình viên |
| `CottonCandy` | Tông hồng tím kẹo bông pastel |
| `Mellowsi` | Tông nâu và vàng cát cổ điển |
| `Rainbow` | Dải màu cầu vồng rực rỡ |
| `Mid Summer` | Gradient hoàng hôn và màu san hô ấm |
| `Lunar Moon` | Gradient màu bạc đơn sắc |
| `Winter Frost` | Gradient màu băng giá |
| `Elegant Night` | Gradient màu tím đêm sang trọng |
| `Lunar Abyss` | Gradient màu chàm vũ trụ tối |
| `Lunar Eclipse` | Gradient màu hoa hồng và đỏ thẫm |

---

## Giấy phép (License)

Dự án phát hành theo giấy phép [MIT License](LICENSE).

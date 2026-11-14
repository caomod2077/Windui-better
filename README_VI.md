# WindUI-Better

[English](README.md) | [Tiếng Việt](README_VI.md)

Phiên bản WindUI mở rộng cho Roblox, hỗ trợ giao diện 2 cột dạng Card, tìm kiếm tính năng trực tiếp trong Tab, thanh tab con lồng nhau (Sub-Tabs) cho phép tùy chỉnh 2 cột hoặc 1 cột cho từng trang riêng biệt, khung hiển thị hồ sơ người chơi (Player Profile Card) ở góc dưới Sidebar, tích hợp sẵn các theme Gradient (bao gồm theme kính trong suốt Liquid Glass và Ocean Blue), hệ thống tự động dịch đa ngôn ngữ thông minh cho mọi thứ tiếng không gò bó từ điển cứng, hiển thị Watermark (FPS / Ping), phân quyền truy cập Tab theo UserId, khung hiển thị mô hình 3D tương tác (Viewport) và tối ưu hóa hiệu năng giảm giật lag.

Thư viện gốc bởi Footagesus. Bản mở rộng và duy trì bởi caomod2077.

---

## Các cải tiến tối ưu hiệu năng (Performance Optimizations)

Nhiều người dùng gặp hiện tượng tụt FPS hoặc đơ giao diện khi dùng WindUI trên các executor. Bản WindUI-Better đã được tinh chỉnh lại các điểm nghẽn hiệu năng sau:

1. Tự động dọn dẹp đối tượng rác (Dead Object Pruning): Toàn bộ đối tượng theme và tín hiệu kết nối gắn với phần tử UI bị xóa (destroy) sẽ được tự động giải phóng khỏi bảng lưu trữ toàn cục, chấm dứt tình trạng rò rỉ bộ nhớ (memory leak).
2. Tối ưu tính toán kích thước (Memoized Shape Calculations): Khung Squircle và viền Glass chỉ tính toán lại ma trận bo góc khi kích thước pixel thực sự thay đổi, loại bỏ hàng nghìn phép toán vector dư thừa mỗi khung hình.
3. Đo FPS / Ping không phân mảnh bộ nhớ (Zero-Allocation Watermark): Bỏ việc chèn và xóa mảng (`table.insert`/`table.remove`) liên tục mỗi frame, thay bằng bộ đếm tích lũy thời gian đơn giản không cấp phát mảng, giúp bộ dọn rác (GC) của Lua không bị quá tải.
4. Tự ngắt vòng lặp khi ẩn giao diện: Hoạt ảnh xoay icon, chuyển màu cầu vồng và vòng lặp watermark sẽ tạm dừng hoàn toàn khi đóng cửa sổ hoặc ẩn phần tử, nhường 100% tài nguyên CPU cho game.
5. Kiểm soát vòng đời vòng lặp nền: Các tác vụ đếm ngược (Countdown) tự động ngắt kết nối khi component cha bị gỡ bỏ.
6. Hệ thống dịch không gây nghẽn: Xóa sạch toàn bộ từ điển cứng trong mã nguồn. Các yêu cầu dịch online chạy bất đồng bộ với cơ chế chống trùng lặp và lưu cache, không phát sinh bất kỳ request HTTP nào lúc khởi động ứng dụng.

---

## Cài đặt và Khởi tạo nhanh

```lua
local WindUI = loadstring(game:HttpGet("https://raw.githubusercontent.com/caomod2077/Windui-better/main/windui.lua"))()

local Window = WindUI:CreateWindow({
    Title = "My Script Hub",
    Author = "by Creator",
    Folder = "MyScriptHub",
    Theme = "Dark",         -- Mặc định: Dark (hỗ trợ Liquid Glass, Ocean Blue, Obsidian, Light, v.v.)
    Size = UDim2.fromOffset(840, 540),
    TwoColumns = true,       -- Bật bố cục 2 cột (mặc định: false)
    BackgroundBlur = false,   -- Làm mờ hậu cảnh (mặc định: false)
    Glow = Color3.fromRGB(0, 145, 255),
    IconAnimation = "Spin",  -- "Spin" | "Pulse" | "Rainbow"
    SidebarBanner = {
        Image = "rbxassetid://133610205520685",
        Height = 75,
        Title = "Hub Version",
    },
    User = {
        Enabled = true,               -- Hiện khung hồ sơ người chơi ở góc dưới Sidebar
        Anonymous = false,            -- Ẩn avatar và tên khi bật true
        Subtitle = "Premium Lifetime", -- Dòng chữ phụ đề / trạng thái
        Callback = function() end,    -- Bắt sự kiện khi click vào khung hồ sơ
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
| `Theme` | string | `"Dark"` | Tên theme khởi đầu từ danh sách có sẵn hoặc tự thêm (Mặc định: Dark) |
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
| `User` | table | `nil` | Khung thông tin người chơi ở góc dưới Sidebar: `{ Enabled, Anonymous, Subtitle, Callback }` |
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
- `Window:SetTheme(name)`: Đổi theme cho cửa sổ đang chạy. Hỗ trợ tìm kiếm linh hoạt không phân biệt hoa thường.
- `Window:SetColor({ Accent, Background, Text, Dialog, ... })`: Đổi màu trực tiếp cho theme đang chạy.
- `Window:EnableSounds(boolean, soundConfig)`: Bật hoặc tắt âm thanh khi click nút và chuyển tab, hỗ trợ cấu hình âm thanh tùy chọn.
- `Window:SetSounds({ Tab, Toggle, Notification, Volume })`: Tự do cài đặt ID âm thanh riêng (`rbxassetid://...`) và âm lượng thay vì bị ép dùng mặc định.
- `Window:PlaySound(soundType)`: Phát âm thanh thủ công (`"tab"` hoặc `"toggle"`).
- `Window:SetUIScale(number)`: Điều chỉnh tỷ lệ thu phóng toàn bộ giao diện (ví dụ `0.85` cho mobile).
- `Window:SetToggleKey(Enum.KeyCode)`: Đổi phím tắt ẩn hiện cửa sổ.
- `Window:SetSize(UDim2)`: Đổi kích thước cửa sổ bằng mã.
- `Window:SetToTheCenter()`: Canh cửa sổ vào chính giữa màn hình.
- `Window:SetBackgroundImage(string)`: Đổi ảnh nền cửa sổ (assetid hoặc link ảnh trực tiếp).
- `Window:SetBackgroundImageTransparency(number)`: Đổi độ trong suốt của ảnh nền (0 đến 1).
- `Window:SetBackgroundTransparency(number)`: Đổi độ trong suốt của khung cửa sổ.
- `Window:ToggleTransparency(boolean)`: Bật hoặc tắt trạng thái trong suốt của cửa sổ.
- `Window:ToggleFullscreen()`: Bật hoặc tắt chế độ phóng to toàn màn hình.
- `Window:LockAll()`, `Window:UnlockAll()`: Khóa hoặc mở khóa hàng loạt toàn bộ phần tử trong cả cửa sổ.
- `Window:GetLocked()`, `Window:GetUnlocked()`: Lấy danh sách các phần tử đang bị khóa hoặc mở khóa.
- `Window:SetTitle(string)`, `Window:SetAuthor(string)`: Cập nhật tiêu đề hoặc phụ đề tác giả khi đang chạy.
- `Window:SetIconSize(number)`: Đổi kích thước icon topbar.
- `Window:SetIconAnimation("Spin" | "Pulse" | "Rainbow")`: Đổi hoặc kích hoạt hoạt ảnh cho icon topbar.
- `Window:AnimateTitle("Gradient" | "Shimmer")`: Bật dải màu chuyển động lướt qua tiêu đề.
- `Window:SetSidebarBanner({ Image, Height, Title })`: Cập nhật banner trên Sidebar.
- `Window:CreateTopbarButton(name, icon, callback, order, isRight, color, iconSize)`: Thêm nút bấm icon tùy chỉnh lên thanh topbar.
- `Window:DisableTopbarButtons(boolean)`: Vô hiệu hóa hoặc bật lại các nút đóng, thu nhỏ trên topbar.
- `Window:Toast({ Title, Content, Duration, Icon })`: Hiện nhanh một thông báo ngắn.
- `Window:Dialog({ Title, Content, Buttons })`: Mở hộp thoại modal popup, có thể chứa trực tiếp các control bên trong.
- `Window:EditOpenButton(table)`: Tùy chỉnh nút bấm mở GUI nổi trên màn hình cho mobile.
- `Window:Section({ Title, Icon, Opened })`: Tạo nhóm phân mục trên Sidebar để gom các Tab.
- `Window:Divider()`: Thêm đường kẻ phân cách trong Sidebar.
- `Window:OnOpen(fn)`, `Window:OnClose(fn)`, `Window:OnDestroy(fn)`: Bắt sự kiện khi cửa sổ đóng, mở hoặc bị xóa.
- `Window.User`:
  - `Window.User:Enable()`: Bật hiển thị khung hồ sơ người chơi.
  - `Window.User:Disable()`: Ẩn khung hồ sơ người chơi.
  - `Window.User:SetAnonymous(boolean)`: Bật hoặc tắt chế độ ẩn danh.
  - `Window.User:SetSubtitle(string)`: Đổi dòng chữ phụ đề bên dưới tên hiển thị.
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
- `WindUI:SetTheme(name)`: Áp dụng theme trên toàn bộ thư viện. Hỗ trợ tên linh hoạt (`"Dark"`, `"Liquid Glass"`, `"Ocean Blue"`, v.v.).
- `WindUI:GetThemes()`: Trả về bảng danh sách tất cả theme có sẵn.
- `WindUI:GetCurrentTheme()`: Trả về chuỗi tên theme hiện tại.
- `WindUI:AddTheme(themeTable)`: Đăng ký một bảng theme màu sắc tùy chỉnh mới.
- `WindUI:Gradient(keypoints, options)`: Hàm tiện ích tạo dải màu Gradient xoay góc cho theme.
- `WindUI:SetFont(fontAssetId)`: Đổi phông chữ hiển thị toàn cục.
- `WindUI:SetLanguage("en" | "vi" | "es" | "fr" | "ru" | "ja" | ...)`: Tự động dịch động toàn diện mọi ngôn ngữ theo thời gian thực (hỗ trợ tra cứu online và lưu cache mượt mà).
- `WindUI:AddTranslations(lang, dict)`: Bổ sung từ điển dịch tùy ý hoặc ghi đè từ ngữ cho bất kỳ ngôn ngữ nào.
- `WindUI:Translate(text, lang, callback)`: Hàm tiện ích dịch nhanh một đoạn văn bản theo từ điển hoặc online API.
- `WindUI:ToggleAcrylic(boolean)`: Bật hoặc tắt hiệu ứng kính mờ Acrylic toàn cục.

---

### 4. Cấu trúc Tab, Tab Con (Sub-Tabs) và Section

- `Window:Tab({ Title, Icon, Desc, TwoColumns, LockedTo, HiddenTo, Whitelist, WhitelistHidden, WhitelistAction, ShowTabTitle, IconShape, IconColor, CustomEmptyPage })`: Tạo tab mới.
  - `TwoColumns = true`: Chia tab thành 2 cột độc lập. Các Card có thể đặt `Side = "Left"` hoặc `Side = "Right"`, hoặc `FullWidth = true`.
  - `TwoColumns = false`: Giữ tab ở dạng 1 cột duy nhất.
  - `LockedTo = { userId, ... }`: Khóa tab và hiện giao diện yêu cầu phân quyền đối với ai không có trong danh sách.
  - `Whitelist = { userId, ... }`: Danh sách UserId được cấp quyền.
    - `WhitelistHidden = true` (mặc định): Ẩn hoàn toàn tab khỏi sidebar đối với ai không có quyền.
    - `WhitelistHidden = false` hoặc `WhitelistAction = "Show"`: Vẫn hiện tab trong sidebar, nhưng khóa tab và hiện màn hình yêu cầu quyền khi bấm vào.
  - `IconShape = "Circle"`: Đặt khung bo tròn bao quanh icon tab.
  - `IconColor = Color3.fromRGB(...)`: Tô màu riêng cho icon của tab.
- `Tab:SubTabs({ { Title, Icon, TwoColumns }, ... })`: Tạo thanh tab con dạng viên thuốc (pill bar) nằm ngang ngay trên đầu tab với các trang độc lập. Mỗi tab con có thể tự do tùy chỉnh số cột riêng biệt:
  - `TwoColumns = true`: Chia trang con thành 2 cột trái và phải (Card có thể đặt `Side = "Left"` hoặc `Side = "Right"`, hoặc `FullWidth = true`).
  - `TwoColumns = false`: Giữ trang con ở dạng 1 cột duy nhất trải rộng toàn chiều ngang.
  - `TwoColumns = nil` (mặc định): Kế thừa thiết lập `TwoColumns` của tab cha.
  ```lua
  local sub = FarmTab:SubTabs({
      { Title = "Level Farm", Icon = "sparkles", TwoColumns = true },   -- 2 cột
      { Title = "Tuning", Icon = "wrench", TwoColumns = true },         -- 2 cột
      { Title = "Combat", Icon = "swords", TwoColumns = false },        -- 1 cột
  })
  local LevelPage = sub["Level Farm"]
  LevelPage:Card({ Title = "Auto Level", Side = "Left" }):Toggle({ Title = "Enabled" })
  LevelPage:Card({ Title = "Rewards", Side = "Right" }):Slider({ Title = "Multiplier", Value = 2 })
  ```
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

Có thể tạo trên `Tab`, `SubPage`, `Card`, `Section`, `Column` hoặc trong `Dialog`:

- `Button({ Title, Desc, Icon, Callback, Color, Justify, IconAlign, Locked, LockedTitle })`: Nút bấm kích hoạt hàm.
- `Toggle({ Title, Desc, Value, Callback, Type = "Toggle" | "Checkbox", Icon, Locked })`: Công tắc bật tắt hoặc ô kiểm checkbox.
  - Hàm điều khiển: `:Set(boolean)`
- `Slider({ Title, Desc, Value = { Min, Max, Default }, Step, Callback, IsTextbox, IsTooltip, Icons = { From, To }, Width, Locked })`: Thanh kéo chọn giá trị số, hỗ trợ ô nhập text và bong bóng tooltip.
  - Hàm điều khiển: `:Set(number)`, `:SetMin(number)`, `:SetMax(number)`
- `Dropdown({ Title, Desc, Values, Value, Multi, AllowNone, SearchBarEnabled, MenuWidth, Callback, Locked })`: Menu thả xuống chọn 1 hoặc nhiều mục. Bấm vào bất kỳ đâu trên dòng đều mở menu mượt mà; chọn mục đơn tự động áp dụng và đóng menu gọn gàng.
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
        Subtitle = "Premium Lifetime", -- Subtitle or status badge text
        Callback = function()         -- Click callback when player profile card is clicked
            WindUI:Notify({
                Title = "Profile",
                Content = "Logged in as " .. game:GetService("Players").LocalPlayer.DisplayName,
                Duration = 3,
            })
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
local DisplayTab = HomeSection:Tab({ Title = "3D & Display", Icon = "box" })

Window:Divider()

local AdvancedSection = Window:Section({ Title = "Advanced", Icon = "box", Opened = true })
local LayoutTab = AdvancedSection:Tab({ Title = "Layout & Groups", Icon = "layout-grid" })
local WhitelistTab = AdvancedSection:Tab({
    Title = "VIP Member Tab",
    Icon = "crown",
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
    Value = "Premium Lifetime",
    Callback = function(text)
        Window.User:SetSubtitle(text)
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

## Danh sách Theme có sẵn

| Tên Theme | Mô tả |
|---|---|
| `Dark` | Giao diện tối tiêu chuẩn (Mặc định) |
| `Liquid Glass` | Kính trong suốt nhìn xuyên thấu thế giới game, viền dạ quang xanh ngọc và khung mờ |
| `Ocean Blue` | Tông màu đại dương xanh thẳm và dải gradient cyan (tên cũ Blue Something) |
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

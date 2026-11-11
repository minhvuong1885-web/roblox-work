---
loai: script
cap-nhat: 2026-11-09
tags: [studio, loi, camera]
---

# Sửa lỗi camera Studio không xoay được bằng chuột phải

## Dấu hiệu
- W/A/S/D và lăn chuột vẫn được, nhưng **giữ chuột phải kéo không xoay**.
- Chỉ bị ở một place (place khác vẫn bình thường).

## Nguyên nhân
Công cụ chụp ảnh của Claude (MCP `screen_capture` có đặt vị trí camera) đổi camera sang chế độ **Scriptable** rồi không trả lại. Chế độ này bị **lưu luôn vào file place**, nên đóng mở lại vẫn kẹt. Chỉ đổi về Fixed một lần là không đủ, nó sẽ bị đổi lại.

## Cách sửa (đã xác nhận chạy được)
Dán vào **Command Bar** (dưới cùng Studio) rồi nhấn Enter, hoặc bảo Claude chạy:

```lua
local c = workspace.CurrentCamera
c.CameraType = Enum.CameraType.Fixed
c.FieldOfView = 70
c.CameraSubject = nil
if _G.__camGuard then _G.__camGuard:Disconnect() end
_G.__camGuard = c:GetPropertyChangedSignal("CameraType"):Connect(function()
	if c.CameraType ~= Enum.CameraType.Fixed then c.CameraType = Enum.CameraType.Fixed end
end)
```

- Dòng `_G.__camGuard` là **bộ giữ**: camera bị đổi về Scriptable thì tự đổi lại Fixed.
- Bộ giữ mất khi đóng Studio. **Mở lại place mà bị khoá thì chạy lại đoạn trên.**

## Cách kiểm tra nhanh
So với một place mới (Baseplate): camera bình thường có `CameraType = Fixed`.

## Không làm
- Không xoá camera (`Camera:Destroy()`): không giúp gì.
- Claude hạn chế chụp ảnh có đặt camera; sau mỗi lần chụp phải chạy lại cách sửa trên.

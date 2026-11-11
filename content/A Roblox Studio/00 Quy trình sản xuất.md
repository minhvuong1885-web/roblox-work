---
loai: quy-trinh
cap-nhat: 2026-11-09
tags: [quy-trinh]
---

# Quy trình sản xuất 1 video – nhánh Roblox Studio

Đọc trước: [[Yêu cầu của tôi]] · [[Master Prompt]]
Nhánh còn lại: [[00 Quy trình Blender]] (render bằng Blender)

Video thuộc nhánh này: [[The Mask]] 🎭 · [[The Silent Mind]] 🧠

## Công cụ
| Việc | Công cụ |
|---|---|
| Dựng cảnh, nhân vật, animation | **Roblox Studio**, kết nối với Claude qua MCP (`.mcp.json` trong `C:\Users\admin\Roblox script&work`) |
| Cảnh khó (sinh vật lớn, VFX lớn) | Blender (tuỳ chọn, chưa cài) |
| Quay | OBS, 1080×1920, 60fps, đồ hoạ mức 10 |
| Dựng video, phụ đề | CapCut |
| Giọng | ElevenLabs **Adam** |

## Công thức truyện
1. **Luật của thế giới (0–5s):** "In this world, …"
2. **Mở rộng thế giới và mối đe doạ** (5–30s)
3. **Cài manh mối** (30–60s): câu nói của người già, vật nhỏ tưởng không liên quan
4. **Nhân vật chính** là đứa trẻ bình thường hoặc bị coi thường
5. **Twist** lật trope: thứ vô dụng hoá ra là chìa khoá, kẻ ác thật ra vô tội…
6. **Kết** công bằng, đảo vị thế; hoặc kết nghịch lý để kéo bình luận

Kịch bản khoảng **230–250 từ**. Giọng Adam tăng tốc **1.5x** → video **~1:00–1:10**. Hook rút còn 0–3s.

## Các bước
1. Viết kịch bản → duyệt
2. Tạo giọng Adam. **Animate theo giọng**, không làm ngược lại.
3. Shot list khoảng 35–45 shot, mỗi câu một cảnh (tính theo giọng đã tăng tốc 1.5x)
4. Dựng map và nhân vật → duyệt từng phần
5. Làm animation, biểu cảm, camera
6. Thêm VFX và chỉnh ánh sáng theo cảnh
7. Quay bằng OBS
8. Dựng ở CapCut: nhạc, SFX, phụ đề, flash, shake

## Hình ảnh
- Khung 9:16, 1080×1920; bố cục cho màn hình dọc ngay từ đầu, không crop từ khung ngang
- Mỗi shot dài 1–3 giây; camera luôn chuyển động: dolly, pan, bay qua, zoom nhanh vào mặt khi có twist
- Ánh sáng: Technology = Future. Ban ngày ấm → đêm xanh tím, đuốc cam → bình minh vàng cam. Dùng ColorCorrection, Bloom, SunRays, DepthOfField, Atmosphere
- VFX: ParticleEmitter, Beam, Trail, Highlight, PointLight

## Âm thanh
- Nhạc orchestral fantasy: tò mò ở đầu → căng ở cao trào → hùng tráng ở kết
- SFX khớp từng hành động

## Phụ đề và đăng
- Phụ đề 1–2 chữ mỗi lần, đặt ở giữa, chữ trắng đậm viền đen, không che mặt nhân vật
- Tiêu đề dạng **"The [Danh từ] + emoji"**
- Mô tả: `#shorts #roblox #entertainment #viral #relatable`. Không có CTA.

## Cấu trúc trong Studio
```
ReplicatedStorage/Cinema/Modules  (Timeline, CameraRig, Actor, FaceSystem, Subtitle, VFX, Audio, Lighting)
ReplicatedStorage/Episodes/EPxx   (cấu hình shot của từng tập)
Workspace/Sets                    (map, mỗi khu một thư mục)
Workspace/Actors, Workspace/CamPoints
StarterPlayerScripts/Director     (LocalScript chạy tập)
```

## An toàn
- Làm trên place riêng của dự án video, không làm trên game thật.
- Asset lấy từ Store: **gỡ hết script** trước khi dùng.

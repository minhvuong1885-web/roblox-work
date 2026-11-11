---
loai: quy-trinh
nhanh: blender
cap-nhat: 2026-11-09
tags: [quy-trinh, blender]
---

# Quy trình sản xuất – nhánh Blender

Đọc trước: [[Yêu cầu của tôi]] · [[Master Prompt]]
Nhánh còn lại: [[00 Quy trình sản xuất]] (Roblox Studio)

> Nhánh này dành cho các video **render bằng Blender** để có hình mịn, bóng mềm, xóa phông như các kênh Roblox Story render (khác với quay màn hình game). Mọi quy tắc trong [[Yêu cầu của tôi]] vẫn áp dụng: phong cách Gneef, hook "In this world…", giọng Adam 1.5x, ~1:00–1:10, nhạc nền cố định, duyệt từng bước bằng ảnh chụp.

## Công cụ
| Việc | Công cụ |
|---|---|
| Lấy nhân vật, quần áo, đạo cụ, map | **Roblox Studio** (MCP) + Creator Store → *Export Selection* (.obj/.fbx) |
| Rig nhân vật | Rig R6/R15 cho Blender (gắn mesh đã export) |
| Dựng cảnh, ánh sáng, camera, animation | **Blender 5.2.3 LTS** (đã cài 2026-11-09: `C:\Program Files\Blender Foundation\Blender 5.2\blender.exe`). Claude điều khiển qua **Blender MCP** (addon `blender_mcp_addon` đã bật; server `uvx mcp-for-blender`, cổng 9876; đã khai báo mục "Blender" trong `.mcp.json`). Mỗi lần làm: mở Blender → N → tab **BlenderMCP** → *Connect to MCP server*. Telemetry tắt. Dự phòng: script bpy chạy nền |
| Render | Eevee (nhanh, mặc định) · Cycles (cảnh quan trọng) |
| Giọng | ElevenLabs **Adam**, 1.5x |
| Phụ đề | Whisper (lấy timing từng từ) + chữ theo kịch bản → file .ass → ffmpeg in vào video. Hoặc CapCut |
| Ghép, nhạc, SFX | ffmpeg / CapCut |

## Các bước
1. Viết kịch bản → duyệt (dùng khung: In this world → 2 ví dụ → tình tiết → cao trào → twist → kết)
2. Tạo giọng Adam → lấy timing từng câu. **Animate theo giọng.**
3. Shot list (mỗi câu một shot, 1–3 giây)
4. Trong Studio: phối avatar từng nhân vật (accessories phù hợp), chọn map/đạo cụ trên Creator Store → **duyệt ảnh chụp**
5. Export sang Blender: nhân vật (.fbx có rig), map và đạo cụ (.obj). Gỡ script trước khi export
6. Trong Blender: dựng cảnh, gắn rig, thay texture mặt theo biểu cảm, ánh sáng → render 1 ảnh mỗi cảnh → **duyệt**
7. Animation + camera theo timing giọng → render nháp độ phân giải thấp → **duyệt**
8. Render bản cuối 1080×1920 (30 hoặc 60 fps)
9. Ghép: giọng + nhạc nền (fade cuối) + SFX + phụ đề → MP4 H.264/AAC

## Hình ảnh
- Khung 9:16 ngay trong camera Blender (Resolution 1080×1920)
- Ánh sáng: HDRI hoặc Sun mềm + đèn phụ ấm; bật Ambient Occlusion, Bloom (Eevee); Depth of Field nhẹ để xóa phông
- Ngày ấm → đêm xanh tím, đuốc cam → bình minh vàng cam
- Biểu cảm: mặt Roblox là texture, mỗi biểu cảm một ảnh, đổi theo keyframe
- Camera luôn chuyển động: dolly, pan, zoom nhanh vào mặt ở twist

## Cấu trúc thư mục dự án (đề xuất)
```
blender/<Tên video>/
  assets/   (obj, fbx, texture mặt)
  scenes/   (.blend từng cảnh)
  scripts/  (bpy)
  render/   (frames, video)
```

## Video thuộc nhánh này
- [[The Cursed Bell]] 🔔 – kịch bản đã duyệt
- [[The Endless Line - Tập 0 (Blender)]] 🚂 – tập giới thiệu series The Endless Line (bắt đầu 2026-11-11)
- [[The Empty Egg]] 🥚 – video đầu tiên làm trọn bằng Blender (v3, 2026-11-10); ghi chi tiết cách làm để dùng lại

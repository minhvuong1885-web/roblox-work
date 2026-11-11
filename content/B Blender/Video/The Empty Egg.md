---
loai: video
nhanh: blender
trang-thai: da-render-v3-cho-duyet
ngay: 2026-11-10
tags: [video, blender, the-empty-egg]
---

# The Empty Egg 🥚 – cách đã làm ra video

Quy trình chung: [[00 Quy trình Blender]] · Yêu cầu: [[Yêu cầu của tôi]]
Kết quả: `blender/The Empty Egg/video/the-empty-egg-v3.mp4` – 1:11, 1080×1920, 24 fps, giọng Adam 1.5x, nhạc nền, phụ đề nảy từng chữ.
Thư mục dự án: `C:\Users\admin\Roblox script&work\blender\The Empty Egg\` (mọi script ở `scripts\`).

## 1. Lấy tài nguyên từ Roblox Studio (không sửa, không publish place)
- Mở Place1 → Explorer, **chuột phải từng nhân vật / folder (Plaza, Houses, Eggs) → Save/Export → Export as Obj**.
  - ⚠️ Đừng dùng File → Export: nó xuất cả place (918 MB).
  - ⚠️ Texture nhân vật đều tên `Handle1_diff.png` → export từng nhân vật riêng và **đổi tên texture theo nhân vật** ngay, nếu không sẽ ghi đè nhau.
- 8 nhân vật + Plaza/Houses/Eggs → `assets/`.

## 2. Dựng cảnh trong Blender 5.2
- Import .obj → ghép map → gắn **rig R6** cho từng nhân vật (đầu, thân, 2 tay, 2 chân xoay quanh khớp vai/hông).
- File chính: `TheEmptyEgg_anim_v2.blend` (v1: `TheEmptyEgg_anim.blend`, gốc: `TheEmptyEgg_scene.blend`).
- Houses nặng → Decimate kiểu **Dissolve** (Collapse làm vỡ nhà).

## 3. Giọng & timing (theo vault)
- vidIQ voiceover giọng **Adam** → `scripts/vo.py` tăng 1.5x bằng WSOLA (giữ cao độ).
- `scripts/align.py`: Whisper lấy timing từng từ → `audio/timing.json`, `words.json`.
- `scripts/subs.py` → `audio/subs.ass` (chữ vàng viền đen, nảy từng từ).

## 4. Shot & animation (31 shot)
- `stage.py` / `stage2.py`: dựng từng shot (vị trí nhân vật, camera, đèn, ngày/đêm/bão).
- `shots.py`: hàm chung + vật liệu VFX (lửa procedural, thủy tinh viền sáng, khói `smoke_mat`), đổi trời `world_mode('day'/'night'/'storm')`.
- `anim.py`: handler `frame_change_pre` – mỗi shot có setup chạy 1 lần + motion(t) mỗi khung, **bám timing giọng** từ `timing.json`.
- **Lớp "sống"** cho mọi nhân vật: thở, dồn trọng tâm, đầu đung đưa, gật khi nói, chớp mắt ngẫu nhiên → nhân vật không bao giờ đứng cứng.
- VFX chiêu: quả cầu phát sáng có vệt sao chổi, hào quang, tia lửa, khói, ánh mắt phát sáng, va chạm + ngã.

## 5. Làm bối cảnh "như thật"
- Tài nguyên CC0 từ **Poly Haven**: HDRI ngày/đêm, texture cỏ đá, cây, cỏ, đá tảng, dương xỉ (`fetch_polyhaven.ps1`, `env_import.py`, `env_build.py`, `env_fix1-4.py`).
- Rải cây cỏ bằng Geometry Nodes, loại điểm trùng nhà/quảng trường/đường.
- Sương mù: hộp volume **FogBox** (đừng dùng volume cho World → trời đen).
- Compositor: Bloom (`scene.compositing_node_group`, Glare Type = Bloom), màu **AgX – Punchy**.
- HDRI đêm phải bật fake user, không sẽ mất khi lưu.

## 6. Mặt biểu cảm (meme)
- Nhờ ChatGPT vẽ sheet 10 mặt nền trong suốt → `faces/split_faces.py` tách ra → thư viện 30 mặt `faces/meme-set-01..03` (xem README từng bộ).
- `env_faces2.py`: gộp 30 mặt thành 1 atlas, đổi mặt bằng cách dịch Mapping node (không phải compile lại shader). Mặt bị lật gương → sửa trong `face()`.
- Quy tắc chọn mặt: **đúng tình huống**, mặt hợp nữ (kissy, dreamy_crush) chỉ nhân vật nữ dùng. Bảng chọn mặt từng shot: `FACE_OVR` trong `anim.py`. Nói thì đổi neutral ↔ talk, chớp mắt dùng blink.

## 7. Render & ghép
- Render nền: `full_v3.ps1` → `blender -b ... -P render_bg.py -- 1 1716 100 16 render/frames_v3` với `EGG_FAST=1` (Eevee, tắt raytracing).
- GTX 1650 4 GB: 6–10 s/khung (cảnh đêm/trận đánh chậm nhất) → cả video ~4 giờ. Chỉ chạy 1 tiến trình Blender.
- Ghép: ffmpeg (`demucs-env\Scripts\ffmpeg.exe`) chạy trong `audio/`: khung hình + giọng + nhạc nền + `-vf ass=subs.ass` → H.264/AAC.
- **Sửa 1 đoạn**: chỉnh `anim.py` → render lại riêng khoảng khung đó → ghép lại, không cần render cả video.

## Bài học
- Duyệt bằng ảnh chụp từng bước (cảnh, mặt, clip thử) trước khi render cả video.
-

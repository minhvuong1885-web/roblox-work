---
loai: video
nhanh: blender
series: The Endless Line
tap: 0
trang-thai: dang-dung-boi-canh
ngay: 2026-11-11
tags: [video, blender, the-endless-line]
---

# The Endless Line – Tập 0 (Blender) 🚂🌌

Kịch bản: [[The Endless Line - Tập 0]] · Series: [[The Endless Line]] · Quy trình: [[00 Quy trình Blender]] · Yêu cầu: [[Yêu cầu của tôi]]

**Vì sao làm bằng Blender:** tập 0 gần như toàn cảnh vũ trụ, mặt trời biến dạng, nhà ga khổng lồ, đoàn ~20 toa — cần ánh sáng khối, motion blur, xóa phông, VFX mà Roblox Studio làm không đẹp. Bối cảnh dựng thẳng trong Blender; chỉ **nhân vật và 1 toa tàu** lấy từ Studio.

## File & thư mục
- File chính: `C:\Users\admin\Roblox script&work\blender\The Endless Line\scenes\EndlessLine_EP0.blend` (1080×1920, 24 fps, EEVEE)
- Collection theo phân đoạn: `01_Hook_Galaxy`, `02_Montage`, `03_LastStation`, `04_Departure`, `05_Sun`, `06_Teasers`, `07_End`, `_Train`, `_Characters`
- Thư mục: `assets/` (obj, fbx, mặt) · `scenes/` · `scripts/` (bpy) · `render/` · `audio/` · `video/`

## Tiến độ
| # | Việc | Trạng thái |
|---|---|---|
| 1 | Kịch bản | chờ duyệt |
| 2 | Giọng Adam 1.5x + timing | chưa |
| 3 | Shot list | chưa |
| 4 | Nhà ga mái vòm + thành phố đổ nát lúc bình minh | dựng xong v1, chờ duyệt ảnh |
| 5 | Đoàn tàu 20 toa + đầu máy hơi nước (dựng mới trong Blender) | dựng xong v1 |
| 6 | Đường ray xoắn xuyên mây (sàn xoắn bê tông quanh tháp, biển mây) | dựng xong v1, chờ duyệt ảnh |
| 7 | Ngân hà + hành tinh mới + Trái Đất già (file riêng `scenes/EP0_space.blend`, script `space.py`) | dựng xong v1, chờ duyệt ảnh |
| 8 | Mặt trời biến dạng (ảnh thật NASA SDO, xoắn méo) — file `scenes/EP0_sun.blend`, script `sun.py` | dựng xong v1, chờ duyệt; mặt trăng nứt + sao chổi làm thử nhưng trông giả nên đang ẩn, cần làm lại bằng ảnh thật |
| 9 | Bóng quái vật, bóng lệch nhịp trên cửa sổ | chưa |
| 10 | 4 cảnh montage "In this world" | chưa |
| 11 | Nhân vật (Milo, mẹ, đám đông) xuất từ Studio + rig + mocap | chưa |
| 12 | Animation + camera theo giọng → render nháp → duyệt | chưa |
| 13 | Render cuối + ghép (giọng, nhạc nền, phụ đề, glitch) | chưa |

## Tài nguyên đã có
- Nhân vật dựng sẵn trong Studio (place "Roblox mapcty", `Workspace.Train.Car319.Cast`): Milo, Mẹ, 38 hành khách; mặt ở `faces/meme-set-02` (Milo) và `faces/meme-set-04` (NPC).
- Con quái đã rig 50 xương: `Downloads\train_creature_rigged.blend` / `.fbx` (chưa dùng trong tập 0).

## Cần chuẩn bị
- Bật thư viện **Poly Haven** trong addon Blender MCP (đang tắt) để lấy HDRI, texture, đồ vật thật.

## Cách dựng bối cảnh 1–3 (script trong `scripts/`)
- `fetch_polyhaven.ps1`: tải CC0 Poly Haven (3 HDRI, 15 texture PBR, 13 đồ vật) → `assets/polyhaven/`.
- `lib.py` (vật liệu PBR chiếu hộp, khối bmesh không cần context, sương volume) · `train.py` (toa + đầu máy, instance 20 toa) · `station.py` (mái vòm kính sắt, tường gạch vòm cuốn, ray, sân đá cuội, đồ vật) · `city.py` (thành phố đổ nát, mỗi tòa 1 mesh, cửa sổ procedural + vết cháy) · `spiral.py` (sàn xoắn quanh tháp, biển mây) · `setup_s1.py` (trời ghép: trời vật lý ở chân trời + mây lửa HDRI phía trên, mặt trời 8°, camera).
- Render nền: `blender -b scenes\EndlessLine_EP0.blend -P scripts\render_still.py -- <Camera> <out.png> <pct> <samples> [CYCLES]`. Camera: `Cam_Station`, `Cam_CityStreet`, `Cam_SpiralSide`.
- ⚠️ Bài học: Blender UI bị crash khi cảnh quá nặng (43k vật thể) → mỗi tòa nhà gộp 1 mesh; render bằng Blender chạy nền, không render qua MCP; lưu file sau mỗi bước. GTX 1650: Cycles phải dùng CUDA (OptiX lỗi). Mây xa trong Eevee bị vỡ khối → dùng mặt biển mây (mesh + SSS) + lớp sương mỏng.

## Nhật ký
- 2026-11-11: tạo dự án, file .blend và thư mục; chuyển kịch bản tập 0 sang nhánh Blender.
- 2026-11-11: bối cảnh 5 — mặt trời dùng ảnh thật NASA SDO/AIA 304Å ngày 31/8/2012 (`assets/sun/GSFC_20171208_Archive_e001953.jpg`, ghi nguồn: *NASA/SDO*), xoắn bằng toạ độ cực + nhiễu, quầng nhật hoa xoắn tím phía sau (noise trên cos/sin để không bị đường nối). Bài học: đồ vật phát sáng dạng "cộng" không che được vật phía sau → đĩa mặt trời phải đặc (Mix với Transparent theo mặt nạ viền). Mặt trăng nứt/sao chổi dựng bằng khối trông giả → ẩn, sẽ làm lại bằng ảnh NASA.
- 2026-11-11: thay ngân hà vẽ công thức bằng **ảnh thật thiên hà Xoáy Nước M51** (NASA PIA10200 "A Classic Beauty", ảnh ghép Hubble/Spitzer/Chandra; ghi nguồn khi đăng: *Image: NASA/JPL-Caltech/ESA/CXC/STScI*) → `assets/galaxy/PIA10200_orig.jpg`, script `galaxy_img.py` + `galaxy_face.py`. Nguồn ảnh vũ trụ thật: images-api.nasa.gov.
- 2026-11-11: dựng bối cảnh 4 (vũ trụ) chạy hoàn toàn bằng Blender nền: `blender -b -P scripts\space.py` → `scenes\EP0_space.blend`; camera `Cam_Galaxy` (đặt lại bằng `space_cam.py`), `Cam_NewPlanet`, `Cam_OldEarth` (render với tham số ẩn `Galaxy`). Ảnh: `render/EP0_04a..04c_*.png`. Ghi chú kỹ thuật: viền khí quyển chỉ sáng phía có nắng nhờ Shader to RGB; đốm lửa phía đêm Trái Đất; ngân hà là đĩa phát sáng đặt rất xa, tắt đổ bóng.
- 2026-11-11: dựng bối cảnh 1–3, ảnh duyệt ở `render/EP0_01_nha_ga.png`, `EP0_02_thanh_pho.png`, `EP0_03_duong_ray_xoan.png`.

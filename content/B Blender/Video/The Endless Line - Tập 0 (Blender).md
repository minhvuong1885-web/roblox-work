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
| 9 | Hình nhá (file `scenes/EP0_teasers.blend`, script `teasers.py` + `win_fix*.py`): 6a quái vật đuổi theo, 6b bóng lệch nhịp ở cửa sổ, 6c đèn tắt dần | v1 đã gửi; 6a đạt, 6b/6c cần làm rõ hơn |
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
- 2026-11-11 (tối): **cảnh Milo giới thiệu (0:22–0:28)** — kịch bản đã thêm vào [[The Endless Line - Tập 0]]. File `scenes/EP0_milo_intro.blend` (tạo từ `EP0_station_v7.blend` bằng `scripts/milo_intro.py`), render bằng `scripts/milo_intro_render.py -- <out> <pct> <samples> <1-152>` (script tự đổi mặt Milo theo `face_track`). Bản nháp: `video/EP0_canh_milo_gioi_thieu_previs.mp4` (chưa có giọng — chờ người dùng tạo câu "Hi! My name is Milo. Me and my mom got on this train… to start a new life on a new planet." bằng giọng Heisenberg). Toa dùng là `Car_303` đổi số thành 307. Shot A f1–30 máy quay ngoài (`Cam_IntroA`), shot B f31–152 trong toa (`Cam_IntroB`, đổi bằng marker). Mẹ tạm dùng thân Roblox + mặt `02_woman_cute` (chưa xuất mẹ thật từ Studio). **Bài học:** ClassicHead của Milo không nằm dưới khớp Head → phải parent lại vào Head, nếu không xoay đầu chỉ có mũ xoay; LowerTorso là cha của chân. Còn phải làm: Milo vươn đầu ra ngoài cửa sổ rõ hơn, đổi mẹ sang mẹ thật, khớp animation với giọng thật.
- 2026-11-11: **chốt cảnh 0:15–0:22 (nhà ga)** — bản nháp `video/EP0_canh4_nha_ga_previs_v2.mp4`, file `scenes/EP0_station_v7.blend` (máy quay `Cam_Train`, script `station_anim.py`; tham số haze 0 = bản nháp nhanh không sương, bước 2 khung). Đoàn **COSMO EXPRESS** (`scenes/CosmoExpress.blend`): 20 toa Car 319 xuất từ Studio (số 300–319, 318/319 cuối đoàn), đầu máy dựng mới `loco_build.py`; mô tả toa chuẩn: `MAU_TOA_TAU_CAR319.md`. Trong ga tàu thu nhỏ 0,47. Đám đông `crowd_v3.py`: 10 kiểu người (mặt meme-set-04 + màu áo), bản đi / xách vali / đứng, ~150 người. Bài học: xuất nhiều nhân vật cùng lúc từ Studio làm treo Studio; cảnh nặng → gộp ~3.900 chi tiết toa thành 89 mesh. Cảnh 1–3 chốt: `video/EP0_canh1-3_nhap_v14.mp4` (phụ đề 1 chữ, từ khoá đỏ, AI xoá chữ gốc cảnh 3, 5 SFX Pixabay).
- 2026-11-11: **Milo 3D đã duyệt** — `scenes/Milo3D.blend`, script `scripts/milo_build2.py -- <gltf> <out.blend> <preview.png> 0 0.74`. Thân xuất từ Studio (chuột phải Milo_Export → Save/Export → Export as glTF; File → Export as glTF là xuất CẢ place → treo Studio). Áo gốc có sẵn hình つみネコ LOVER'S BOOK. Mũ thỏ: Roblox chặn xuất → dùng mẫu 3D người dùng gửi `assets/hat3d/base_basic_pbr.glb`, rộng 1,34× đầu, đáy mũ ở 74% chiều cao đầu. Da mặt phải mờ (roughness 0.75, không coat) để không có vệt bóng giữa mặt. Bản nháp cảnh 1–3: `video/EP0_canh1-3_nhap_v5.mp4` (phụ đề lấy mốc từ Whisper, tắt tiếng gốc cảnh 3).
- 2026-11-11: tạo dự án, file .blend và thư mục; chuyển kịch bản tập 0 sang nhánh Blender.
- 2026-11-11: bối cảnh 6. **Sửa lỗi mẫu toa tàu** (đã sửa trong `train.py` và cả `EndlessLine_EP0.blend` bằng `roof_fix.py`, `shell_fix.py`): (1) mái là khối trụ dẹt, nửa dưới lấp kín trong toa → chỉ giữ nửa trên; (2) thân toa là khối đặc, cửa sổ chỉ là đường hầm → thêm Solidify 8 cm trước Boolean để thành vỏ rỗng. Quái vật: chụp mesh tĩnh từ rig con quái, vật liệu Emission đen (bóng đen tuyệt đối), mắt đỏ phát sáng, ánh sáng ngược đỏ tím xuyên sương. Toa sáng/tối: hai bộ nguồn `TR_Carriage_SRC` / `TR_Carriage_DARK` (thuộc tính instancer không chạy). Ảnh: `render/EP0_06a..06c_*.png`.
- 2026-11-11: **mặt trời v4 – theo ảnh mẫu người dùng** (mặt trời là khối cầu được chiếu từ một bên, như hành tinh có ngày/đêm): ảnh SDO 4096 tô bóng như khối cầu (pháp tuyến dựng từ toạ độ đĩa), đường phân ngày đêm mềm, viền phía sáng rực, phía tối gần đen; quầng lấy độ sáng thật từ ảnh 171Å + tua lửa chỉ ở phía sáng. Bóng tối lan = xoay hướng sáng 0°→84° (giây 1,5→2,75). Script `sun_v4.py` (chạy trên `EP0_sun_v3.blend`) → `EP0_sun_v4.blend`, clip `video/EP0_05_mat_troi_test_v4.mp4` (~4,7 phút render).
- 2026-11-11: **mặt trời v3 (người dùng muốn thật, rõ nét, không méo)**: bỏ xoắn/bóp méo; dùng ảnh SDO **4096px** mới nhất (`sdo.gsfc.nasa.gov/assets/img/latest/latest_4096_0304.jpg` + `_0171.jpg` cộng 38% cho viền vàng), chế độ màu Standard (AgX làm phai đỏ cam thành hồng). Hiệu ứng: bóng tối lan nhanh nuốt nửa mặt trời (giây 1,5→3), mép lửa lởm chởm cam đỏ. Script `sun_v3.py` → `scenes/EP0_sun_v3.blend`, clip `video/EP0_05_mat_troi_test_v3.mp4` (render 120 khung ~3 phút). Bài học: hiệu ứng "nứt" bằng voronoi trông như lưới giả → bỏ.
- 2026-11-11: bối cảnh 5 — mặt trời dùng ảnh thật NASA SDO/AIA 304Å ngày 31/8/2012 (`assets/sun/GSFC_20171208_Archive_e001953.jpg`, ghi nguồn: *NASA/SDO*), xoắn bằng toạ độ cực + nhiễu, quầng nhật hoa xoắn tím phía sau (noise trên cos/sin để không bị đường nối). Bài học: đồ vật phát sáng dạng "cộng" không che được vật phía sau → đĩa mặt trời phải đặc (Mix với Transparent theo mặt nạ viền). Mặt trăng nứt/sao chổi dựng bằng khối trông giả → ẩn, sẽ làm lại bằng ảnh NASA.
- 2026-11-11: thay ngân hà vẽ công thức bằng **ảnh thật thiên hà Xoáy Nước M51** (NASA PIA10200 "A Classic Beauty", ảnh ghép Hubble/Spitzer/Chandra; ghi nguồn khi đăng: *Image: NASA/JPL-Caltech/ESA/CXC/STScI*) → `assets/galaxy/PIA10200_orig.jpg`, script `galaxy_img.py` + `galaxy_face.py`. Nguồn ảnh vũ trụ thật: images-api.nasa.gov.
- 2026-11-11: dựng bối cảnh 4 (vũ trụ) chạy hoàn toàn bằng Blender nền: `blender -b -P scripts\space.py` → `scenes\EP0_space.blend`; camera `Cam_Galaxy` (đặt lại bằng `space_cam.py`), `Cam_NewPlanet`, `Cam_OldEarth` (render với tham số ẩn `Galaxy`). Ảnh: `render/EP0_04a..04c_*.png`. Ghi chú kỹ thuật: viền khí quyển chỉ sáng phía có nắng nhờ Shader to RGB; đốm lửa phía đêm Trái Đất; ngân hà là đĩa phát sáng đặt rất xa, tắt đổ bóng.
- 2026-11-11: dựng bối cảnh 1–3, ảnh duyệt ở `render/EP0_01_nha_ga.png`, `EP0_02_thanh_pho.png`, `EP0_03_duong_ray_xoan.png`.

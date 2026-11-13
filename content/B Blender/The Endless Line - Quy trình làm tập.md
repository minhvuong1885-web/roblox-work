---
trang-thai: dang-dung
tags: [the-endless-line, quy-trinh, skill]
---
> Bản sao của skill Claude `endless-line-episode`. Sửa ở đây thì báo Claude cập nhật skill. Xem [[The Endless Line - Tập 0]], [[00 Quy trình Blender]].

# The Endless Line — làm một tập từ kịch bản tới video hoàn chỉnh

Rút ra từ Tập 0 (11–13/11/2026). Mục tiêu: người dùng chỉ đưa kịch bản → Claude làm **một mạch**, chỉ dừng ở những chỗ thật sự cần người dùng (thu VO, render máy 2, chọn khi có nhiều phương án).

## 0. Trước khi làm
1. Đọc memory: `the-endless-line-ep0-progress`, `pc-lag-limits`, `render-package-autorun`, và note vault `B Blender/Kịch bản/The Endless Line - Tập 0.md` (vault: `C:\Users\admin\Downloads\Vault Roblox\Roblox Work`). Vault thắng memory nếu mâu thuẫn.
2. Gốc dự án: `C:\Users\admin\Roblox script&work\blender\The Endless Line\` (đường dẫn có dấu cách và `&` → **luôn đặt trong ngoặc kép**).
3. Kiểm tra RAM trống trước mọi việc nặng (`FreePhysicalMemory`).
4. Công cụ:
   - Blender: `(Get-ChildItem "C:\Program Files\Blender Foundation" -Recurse -Filter blender.exe | Select -First 1).FullName` (Blender 5.2, EEVEE).
   - ffmpeg: `C:\Users\admin\AppData\Local\demucs-env\Scripts\ffmpeg.exe` (KHÔNG có trong PATH, không có ffprobe → dùng `ffmpeg -i` đọc Duration).
   - Python có `faster_whisper`, numpy, Pillow.

## 1. Kịch bản → bảng cảnh
- Tạo note vault `B Blender/Kịch bản/The Endless Line - Tập N.md` theo mẫu Tập 0: bảng `# | thời gian | lời | hình | trạng thái`. Video **dọc 1080×1920, 24 fps**, tổng ~1:00–1:10.
- Mỗi câu thoại = 1 cảnh, 4–8 giây. Ghi rõ máy quay, ai làm gì, khung bắt đầu/kết thúc từng đoạn (như docstring đầu `scripts/zombie_car_light.py`, `scripts/milo_escape.py`).
- Đưa bảng cho người dùng **một lần**, kèm danh sách câu VO cần thu, rồi làm tiếp ngay (không chờ duyệt từng cảnh trừ khi họ muốn).

## 2. Tài sản có sẵn (dùng lại, không dựng lại)
| Thứ | Ở đâu |
|---|---|
| Ga tàu + đoàn tàu + đám đông, Milo/Mẹ trong toa (đồ mới, mẹ tóc dài) | `scenes/EP0_milo_intro.blend` (84 MB — **nặng, máy này không mở dựng nổi**) |
| Toa đơn nhẹ: Milo, mẹ, 6 zombie | `scenes/EP0_milo_escape.blend` (render nháp được trên máy này tới ~khung có ít người) |
| Toa tàu đơn / đầu máy | `scenes/Car319.blend`, `scenes/CosmoExpress.blend` |
| Milo 3D | `scenes/Milo3D.blend` |
| Vũ trụ, Trái Đất, mặt trời xoắn, tay quái vật, xuyên toa | `EP0_space*`, `EP0_leave_earth`, `EP0_sun_twist`, `EP0_hands`, `EP0_lights_out` |
| Helper dùng chung | `scripts/lib.py`; slim/pack: `slim_textures.py`, `pack_copy.py`; render: `render_frames.py` |
| Mặt | `C:\Users\admin\Roblox script&work\faces\meme-set-0X\` (README trong từng thư mục). Mẹ sợ: `meme-set-05/01_woman_quiet_fear_padded_big.png`; zombie: `meme-set-05/11_zombie_red_eyes_padded.png` |

Quy ước nhân vật đã duyệt: Milo dáng **trẻ con** (scale ~0.78–0.86, tay thon x0.68), đội mũ thỏ; mẹ tóc dài; zombie cao hơn (0.33–0.36), mặt possessed mắt đỏ; người trong toa scale 0.28. Hành khách luôn có tóc + quần áo hoạ tiết.

## 3. Dựng cảnh (Blender headless)
- Mỗi cảnh một script `scripts/<ten_canh>.py` theo mẫu `milo_escape.py` / `zombie_car_light.py`: mở .blend nguồn, **xoá trước** mọi thứ không dùng (delete, không hide), thêm hành động + đèn + camera (marker đổi camera), purge orphans, lưu `scenes/EP{N}_<ten>.blend`, pack + `slim_textures.py`.
- Chạy: `Start-Process` với `-b "<abs.blend>" -t 4 -P "<abs.py>" -- "<out>"`, `PriorityClass = BelowNormal`, **watchdog**: vòng lặp kiểm RAM 2–3 s, `Stop-Process` nếu < 1,5 GB. Không bao giờ chạy 2 Blender cùng lúc.
- Nếu nguồn là `EP0_milo_intro.blend` hoặc file > ~60 MB: **đừng thử dựng trên máy này** (Tập 0 hỏng 3 lần). Đóng gói luôn script + .blend nguồn để máy 2 tự dựng rồi render (mẫu `goi_render_zombie_toa`).
- Ảnh duyệt: 30–40 % độ phân giải, 6–8 samples, 1–5 khung; nháp chuyển động: cứ 2 khung lấy 1, render theo đợt 4–12 khung mỗi process (RAM rò theo thời gian). Cảnh đông người dễ vượt RAM → bỏ nháp, đóng gói.

## 4. Gói render cho máy thứ 2 (định dạng người dùng đã chốt)
Thư mục `goi_render_<ten>/` + zip:
- `.blend` tự chứa (pack_all, `UNPACKED: []`) **hoặc** .blend nguồn + script dựng (đường dẫn ảnh trong script đổi thành tương đối `os.path.dirname(__file__)`).
- `render.bat`: dòng `BLENDER=` / `FFMPEG=` / `PCT` / `SAMPLES` / `FRAMES` ở đầu; nếu cần dựng thì `if exist <blend> goto render`; render bằng `render_frames.py` ra `frames/`, ghép `EP{N}_canh<k>_<ten>_raw.mp4` (không tiếng, không phụ đề).
- `HUONG_DAN.md`: "Gửi Claude ở máy kia", bảng nội dung theo khung, cách chạy, **xem thử 5 khung** trước, tự kiểm tra, lỗi hay gặp, gửi gì về.
- **Không** thêm CHAY.bat / CLAUDE.md / tự tìm Blender (người dùng đã từ chối).

## 5. VO
- Người dùng tự thu trên ElevenLabs giọng **Heisenberg - Polished, and Clear** (file mp3 trong Downloads). Mỗi cảnh một file.
- Xử lý (1.2x + 1 semitone + cắt lặng đầu):
  `ffmpeg -i in.mp3 -af "asetrate=44100*1.059463,atempo=1.132652,aresample=44100,silenceremove=start_periods=1:start_threshold=-45dB" -ar 44100 audio/EP{N}_VO_<ten>.wav`
- Mốc chữ: `python scripts/word_times.py audio/..._words.json audio/....wav 0`.
- Báo lại câu nào rơi vào đoạn hình nào.

## 6. Hoàn thiện từng cảnh (sau khi có raw mp4 từ máy 2)
- **Cắt theo VO: không để đuôi im lặng** — cảnh kết thúc ~0,3 s sau chữ cuối (người dùng bắt bỏ đuôi thừa).
- Phụ đề từng chữ: `python scripts/make_subs_word.py words.json out.ass` → burn `subtitles=...ass`. **Tất cả chữ trắng, chỉ chữ "NO" đỏ + nảy** (người dùng chốt 13/11; đừng thêm từ khoá màu). ASS cũ: `scripts/restyle_ass.py in out`.
- **Luôn giữ bản sạch không phụ đề** của mọi cảnh (raw mp4 / frames) và **gói render không burn phụ đề** — phụ đề chỉ burn ở máy chính, để sửa kiểu chữ không phải render lại.
- **Glitch khi chuyển toa/chuyển cảnh**: tìm chỗ đen bằng `blackdetect=d=0.04:pix_th=0.06`; hiệu ứng (escape dấu phẩy `\,` trong `between`):
  `rgbashift=rh=-24:bh=24:rv=6:enable=E,noise=alls=60:allf=t:enable=E,eq=brightness=0.06:contrast=1.3:enable=E,crop=w=iw-40:h=ih:x=20+20*sin(t*90)*(E):y=0,scale=1080:1920` mỗi lần ~0,25 s. **Không đặt glitch sát cuối cảnh.**
- SFX chuẩn: tiếng glitch đã chọn `audio/sfx_glitch_chosen.wav` (vol 0.6), `audio/sfx_pixabay/whoosh.mp3` (0.35, đặt ~0,2 s trước glitch), tàu ầm nền `anoisesrc=c=brown,lowpass=f=180,volume=0.5`; có thêm riser/boom/whoosh_hit trong `audio/sfx_pixabay/`. Khi người dùng muốn chọn âm, làm 5 phương án đặt cạnh nhau.
- Chuyển cảnh **giữa hai không gian khác nhau** (toa A → toa B): dùng kiểu **nhấp nháy xen kẽ** (người dùng chọn ở Tập 0 scene 9) — chớp A/B 4 lần × 2 khung, tiếng điện chập (aevalsrc vuông 100 Hz + nhiễu, bật/tắt theo nhịp 0,083 s, volume **0.35** — 0.7 bị chê to), bùm trầm `boom.mp3` (0.45) khi vào hẳn cảnh B. Glitch RGB dành cho camera xuyên toa liên tục.
- Cảnh render từ máy 2 thường **rất tối** → grade `eq=gamma=1.7–2.0:contrast=1.15–1.25:saturation=1.2–1.3`. Đặt camera gần/thấp hơn nếu cần thấy mặt.
- Mẫu đầy đủ: lệnh tạo `video/EP0_canh_xuyen_toa_final.mp4` trong memory/progress.
- **Bộ chuyển cảnh người dùng đã duyệt ở Tập 0** (mã đầy đủ trong `scripts/build_60fps.ps1`):
  - ga → cảnh tiếp: **cắt chéo màn hình** — khung cuối cắt theo đường chéo, 2 nửa trượt ra 2 góc (0,45 s), lộ cảnh mới phóng 1.6→1 vào nhân vật ở giữa + whoosh.
  - trong toa → ngoài vũ trụ: **mờ chéo 0,5 s** (giữ khung cuối bằng `tpad` để không lệch thời gian) + tiếng tàu khởi hành thật (còi + xình xịch) ở **-24 dB** (to hơn bị chê) + swell nhẹ -10 dB.
  - ngoài toa → trong toa: **lao vào cửa sổ, tối dần + rè nhẹ** (KHÔNG sáng chói): zoom 0,7 s có rung, tmix, rgbashift nhỏ, noise 28, tối về đen; cảnh sau hiện từ đen, noise tan dần; whoosh lowpass ngắn 0,75 s + rè khẽ 0,8 s (dài hơn bị chê thừa).
  - toa A → toa B: nhấp nháy xen kẽ (trên). Xuyên toa liên tục: glitch RGB, không đặt sát cuối.
- `blend` của ffmpeg phải `format=gbrp` trước, không thì ám tím. `scale` với `eval=frame` + overlay làm ffmpeg crash → logo nảy thì pre-render khung bằng PIL.
- **Logo kết**: `assets/title/endless_line_logo.png` (đỏ, hoạt hình rùng rợn, 1 dòng — người dùng tự tạo bằng ChatGPT). Xuất hiện ngay sau lời cuối: nảy 1.3→1, glitch RGB + noise, tiếng riser + glitch + boom; dưới logo "Follow us to see more!" (Impact 56, trắng viền đen, sát mép logo); cuối fade đen.

## 6b. Mức âm lượng (người dùng chốt 13/11)
- **Voice đỉnh -3 đến -1 dBFS**, **mỗi SFX đỉnh ~-6**, **nhạc nền ~-15**, tiếng nền môi trường (rumble) ~-14. Không duck nhạc, **không hạ SFX dưới voice** (người dùng nhắc lại mức -6). Tiếng dài kiểu môi trường (tàu chạy) đặt riêng thấp hẳn (-24).
- VO thay mới cho cảnh đã burn phụ đề: tách câu thành cụm theo khoảng lặng, dời từng cụm cho khớp mốc chữ cũ; chữ mới thêm (vd "And") thì burn bổ sung 1 dòng ASS trên bản ghép.
- Làm bằng `scripts/build_mix.py`: danh sách sự kiện (file, giây bắt đầu, cắt, fade) cho VO / SFX / bed, chuẩn hoá đỉnh từng cái rồi cộng, trần mềm -1 dB → `audio/EP{N}_mix.wav`, mux với video đã ghép (`-map 0:v -map 1:a -c:v copy`). Video các cảnh ghép không cần tiếng.

## 7. Ghép cả tập
- Theo `scripts/compose_0000-0045_v2.ps1`: concat các cảnh (đã burn phụ đề, fps=30), **bỏ audio riêng** từng cảnh, đặt lại VO bằng `adelay` theo offset, **một** nhạc nền liên tục `C:\Users\admin\Roblox script&work\audio\nhac-nen\nhac-nen-khong-voice.wav` (vol 0.35, sidechaincompress dưới VO), SFX theo offset, `alimiter`. Xuất `video/EP{N}_ghep_*.mp4`, giữ bản cũ khi làm lại (`_v2`, `_v3`).
- Khi thay một cảnh, giữ **đúng số khung** để không phải dời offset; nếu đổi độ dài thì dời mọi offset phía sau.
- Cảnh cuối: tiêu đề **THE ENDLESS LINE** + câu kêu gọi theo dõi.
- **Xuất 60 fps** (bản giao): RIFE `%LOCALAPPDATA%\rife-ncnn\...\rife-ncnn-vulkan.exe -m rife-v4.6 -n <khung×2.5>` nội suy từng cảnh SẠCH (24→60), rồi dựng lại mọi chuyển cảnh/phụ đề/logo ở 60 fps và mux `build_mix.py`; quy trình mẫu `scripts/build_60fps.ps1` (~10 phút/tập). Sau khi dựng, kiểm độ dài từng cảnh khớp bản 30 fps (pad/trim) để voice không lệch. Tốt hơn nữa: render 60 fps từ đầu nếu máy 2 chịu được.

## 8. Một mạch — chỉ dừng ở đây
1. Sau bảng cảnh: đưa danh sách câu VO để thu (làm song song phần dựng).
2. Khi gói render đã sẵn: báo tên các zip cần chạy ở máy 2.
3. Khi có nhiều phương án thẩm mỹ thật sự (âm, glitch, màu): gửi tối đa 5 bản để chọn.
Mọi thứ khác tự quyết theo quy ước trên, cập nhật memory tiến độ `the-endless-line-ep{N}-progress` và cột trạng thái trong note vault sau mỗi mốc. Gửi file kết quả bằng SendUserFile.

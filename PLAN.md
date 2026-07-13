# Skyforge — Kế hoạch thực hiện

Game không chiến 3D arcade chạy trên trình duyệt. Một file `index.html` self-contained, Three.js r160+ qua CDN (importmap + ES modules), toàn bộ hình học procedural, âm thanh Web Audio API, mục tiêu 60fps.

**Quy trình:** mỗi phase là một mốc chạy được, chơi thử được. Xong phase → commit, tag version, push, deploy Vercel → dừng chờ feedback. Không tự nhảy phase tiếp theo.

---

## Phase 0 — Setup hạ tầng ✅
- `git init`, `.gitignore`
- Repo GitHub public `skyforge`
- Deploy Vercel (`vercel --prod`), có link
- `PLAN.md`, `README.md`
- `index.html` placeholder (màn hình tiêu đề tối giản để xác nhận deploy chạy)

**Mốc:** mở link Vercel thấy trang Skyforge.

## Phase 1 — Máy bay bay được (v0.1.0)
- Skeleton kiến trúc: comment đầu file, chia section rõ ràng
- Fixed timestep 60Hz tách khỏi render loop
- Mô hình bay arcade: vận tốc theo vector mũi, ga 40–180 u/s, W/S/A/D/Q/E với input làm mượt, tốc độ <60 thì mũi tự chúc xuống, chạm đất/trần nảy lại
- Máy bay low-poly ghép primitive: thân thuôn, cánh delta, đuôi đứng, kính buồng lái
- Mặt đất phẳng tạm + lưới tham chiếu, skybox gradient + fog
- Camera ngôi thứ ba bám đuôi có độ trễ lò xo

**Mốc:** bay lượn quanh thế giới tạm, cảm giác điều khiển ổn.

## Phase 2 — Thế giới (v0.2.0)
- Terrain 8000×8000, simplex noise, 3 vùng pha trộn: núi (đá xám→tuyết, đỉnh 600), hoang mạc (đụn cát vàng + cột đá), biển + đảo (sóng nhẹ, cát→cây xanh)
- Một BufferGeometry duy nhất, vertex color, flat shading
- Mây low-poly instanced, biên mềm 8000×8000 đẩy người chơi lại
- Camera buồng lái (phím C): khung kính, không thấy thân máy bay
- Afterburner phát sáng theo ga, khói đầu cánh khi quẹo gấp

**Mốc:** bay ngắm cảnh 3 vùng địa hình, đổi 2 góc nhìn, giữ 60fps.

## Phase 3 — Vũ khí + HUD (v0.3.0)
- Pháo: 900 rpm, đạn bay thật (không hitscan), quá nhiệt 3s bắn / 2s nghỉ
- Tên lửa: 4 quả, khóa mục tiêu 1.5s (<1500u), bay bám giới hạn góc lượn, vệt khói, nổ gần
- HUD Canvas2D: thước tốc độ / độ cao / hướng, vòng ngắm + chỉ báo đón đầu, ô khóa mục tiêu, thanh ga / đạn / quá nhiệt / máu, đếm kill, radar mini
- Mục tiêu tĩnh (bia bay lơ lửng) để test bắn
- Âm thanh: động cơ đổi cao độ theo ga, tiếng pháo, tiếng nổ
- Rung màn hình khi bắn, hit marker

**Mốc:** bắn nổ bia, HUD đầy đủ, ngắm đón đầu hoạt động.

## Phase 4 — Địch + AI (v0.4.0)
- Máy bay địch (biến thể màu khác), spawn theo wave (3, +1/wave)
- AI 3 trạng thái: TRUY ĐUỔI (slerp về player, giới hạn tốc độ xoay < player), TẤN CÔNG (nón 12°, <800u), TÁCH LY (bị bám >2s hoặc máu <30%)
- Địch bắn cùng hệ thống vũ khí; player 100 máu, rung khi trúng đạn
- Địch nổ: cầu lửa phình + hạt + mảnh vỡ
- Radar hiện địch, khóa tên lửa vào địch

**Mốc:** dogfight thật sự với 1 wave địch.

## Phase 5 — Game loop + polish (v1.0.0)
- Thắng: qua 3 wave. Thua: hết máu → game over, hiện kill, R chơi lại
- Màn hình tiêu đề / wave transition
- Vệt tốc độ + mở FOV khi ga cao
- Cân bằng độ khó, tối ưu hiệu năng, rà soát 60fps
- Hoàn thiện README

**Mốc:** chơi từ đầu tới thắng/thua hoàn chỉnh.

---

## Điều khiển (tham chiếu)
W/S chúc-ngóc · A/D roll · Q/E yaw · Shift/Ctrl ga · Space pháo · F tên lửa · C đổi camera · R chơi lại

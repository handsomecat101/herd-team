# DECISIONS.md — Nhật Ký Quyết Định

> **Chỉ Coordinator ghi file này.**
> Ghi **quyết gì** và **vì sao** — không ghi quyết định hiển nhiên.
> Luật: 1–3 dòng / quyết định. Mới nhất ở trên.

---

## Quyết định

### 2026-10-19 — Smoke test do agent chạy bằng browser thật, không giao cho Human

- **Chọn:** cài `agent-browser` để agent tự mở trình duyệt, tự bấm, tự đọc console.
- **Vì:** ban đầu định giao Human bấm tay. Sai — agent điều khiển trình duyệt được, và
  tiêu chí "console không có cảnh báo hydration" chỉ kiểm được khi chạy thật.
- **Đánh đổi:** thêm một công cụ phải cài (Chrome tải về ~197 MB). Xem lại nếu môi trường
  không cho phép cài công cụ ngoài.

### 2026-10-19 — Chốt `amount >= 0` thay vì `> 0`

- **Chọn:** cho phép giao dịch 0 đồng; sửa `CONTRACT.md` cho khớp với phần kiểm tra dữ liệu.
- **Vì:** hai chỗ trong contract mâu thuẫn nhau (`> 0` ở phần kiểu dữ liệu, `>= 0` ở phần
  kiểm tra nhập liệu). Bên code đã theo `>= 0`.
- **Đánh đổi:** giao dịch 0 đồng lọt vào thống kê. Vô hại, và giao diện đã chặn ở tầng nhập.

### 2026-10-19 — Thêm `CONTRACT.md` trước khi cho 2 agent code song song

- **Chọn:** khoá toàn bộ chữ ký hàm / kiểu dữ liệu / props trước khi giao việc.
- **Vì:** một agent làm UI, một agent làm lớp dữ liệu, hai bên **không nói chuyện được với
  nhau**. Không có contract thì bên dữ liệu đổi tên hàm là bên UI gãy ngay.
- **Đánh đổi:** tốn ~15 phút chốt trước. Đổi lại hai bên chạy độc lập được, không phải chờ nhau.

### 2026-10-19 — Chia việc theo tầng, không chia theo tính năng

- **Chọn:** Track A = `app/**` + `components/**`, Track B = `lib/**`.
- **Vì:** chia theo đường dẫn file thì hai agent không bao giờ sửa cùng một file.
  Chia theo tính năng ("người làm form, người làm biểu đồ") thì cả hai đều phải đụng
  vào lớp dữ liệu và sẽ ghi đè nhau.
- **Đánh đổi:** phải chốt contract chặt hơn. Chấp nhận được.

### 2026-10-19 — Không dùng thư viện biểu đồ, vẽ bằng CSS

- **Chọn:** tự dựng biểu đồ cột bằng phần tử HTML và CSS, không thêm recharts/chart.js.
- **Vì:** nhu cầu chỉ là 6 cột so sánh thu/chi — một thư viện là quá nhiều cho việc đó.
- **Đánh đổi:** không có tooltip, không có animation mặc định. Xem lại nếu cần biểu đồ
  tương tác phức tạp.

### 2026-10-19 — Lưu dữ liệu bằng `localStorage`, không dựng backend

- **Chọn:** toàn bộ giao dịch nằm trong `localStorage`, kèm xuất/nhập JSON để sao lưu.
- **Vì:** một người dùng, một thiết bị, không muốn vận hành server.
- **Đánh đổi:** dữ liệu gắn với một trình duyệt; xoá dữ liệu duyệt web là mất. Xem lại
  nếu cần đồng bộ nhiều thiết bị.

---

*DECISIONS.md — Rẻ nhất để viết, đắt nhất khi thiếu.*

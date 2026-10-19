# Ví dụ: Expense Tracker

> Đây là **bộ file thật** của một dự án đã chạy xong — không phải file mẫu viết cho đẹp.
> Dùng để tham chiếu cách điền, đặc biệt là `CONTRACT.md` và `DECISIONS.md`.

---

## Bối cảnh dự án

App ghi chép thu chi cá nhân. Next.js + TypeScript + Tailwind, dữ liệu trong `localStorage`,
không backend. 15 file source. Làm xong trong một buổi với **3 agent + 1 coordinator**
trên cùng một herdr workspace.

## Đội hình đã dùng

| Vai | Ai | Mảng |
|-----|----|------|
| Coordinator | Command Code | điều phối, ghi file kit, không code |
| Track A | Antigravity | khung dự án + toàn bộ UI (`app/**`, `components/**`) |
| Track B | OpenCode | lớp dữ liệu (`lib/**`) |
| Track C | Command Code (MiniMax) | review, verify, test — **không sửa gì** |

## Đọc file nào trước

1. `CONTRACT.md` — xem cách khoá giao diện để 2 agent code song song
2. `DECISIONS.md` — xem cách ghi lý do quyết định
3. `ISSUE_LOG.md` — xem entry lỗi được viết thế nào
4. `SOUL.md`, `STATE.md`, `GOVERNANCE.md` — bộ khung còn lại

## Điều đáng chú ý nhất

**Track C tìm ra 2 lỗi mà Track A và Track B không thấy:**

- Một hàm định dạng nhãn tháng trả `"T0"` / `"TNaN"` với đầu vào sai
- Rủi ro hydration mismatch khi định dạng tiền bằng `Intl.NumberFormat`
  (Node và Chrome sinh ký tự trắng khác nhau)

Cả hai đều nằm trong code mà **hai agent đã tự chạy thử và báo là ổn**.
Đây là lý do phải có người kiểm tra độc lập — người viết không tự thấy lỗi của mình.

## Chi phí thật của việc chia đội

Với dự án cỡ này, chia 3 agent là **hơi thừa**. Phần lớn thời gian không mất ở chỗ code
mà ở chỗ phối hợp: chốt contract, bật quyền, brief lại khi mất context.

Chia đội đáng giá khi: dự án đủ lớn, hoặc bạn **bắt buộc** phải có người kiểm tra độc lập.

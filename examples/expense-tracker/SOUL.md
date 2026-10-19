# SOUL.md - Identity & Purpose

> "Tâm hồn" của dự án. Mọi agent đọc file này ĐẦU TIÊN để hiểu đang làm gì.

---

## Project Identity

| Field | Value |
|-------|-------|
| **Project Name** | Expense Tracker (Quản lý chi tiêu) |
| **Type** | Web App (single page, client-side) |
| **Description** | App ghi chép & thống kê thu chi cá nhân: thêm/sửa/xóa giao dịch, xem tổng thu – chi – số dư, biểu đồ 6 tháng, phân tích theo danh mục, backup bằng JSON |
| **Tech Stack** | Next.js (App Router) + TypeScript + Tailwind CSS |
| **Storage** | localStorage (không backend, không database) |
| **Trạng thái** | 📝 Đã có tài liệu khởi điểm — chưa có code |

---

## Goals & Objectives

### Primary Goal
Cho một người dùng cá nhân ghi lại thu/chi hằng ngày trong vài giây và nhìn ra ngay tháng này tiêu vào đâu — toàn bộ chạy offline, không cần đăng ký tài khoản.

### Success Criteria
- [ ] Thêm / sửa / xóa giao dịch hoạt động đúng, dữ liệu còn nguyên sau khi F5
- [ ] Số dư = tổng thu − tổng chi, tính đúng theo bộ lọc đang áp dụng
- [ ] Biểu đồ 6 tháng + bảng phân tích danh mục hiển thị đúng tỉ lệ
- [ ] Xuất JSON → nhập lại khôi phục được đúng dữ liệu cũ
- [ ] `npx tsc --noEmit` và `npm run build` pass, console không có cảnh báo hydration

---

## Target Users

- [ ] Người đi làm muốn kiểm soát chi tiêu cá nhân hằng tháng
- [ ] Sinh viên / freelancer cần biết tiền vào – tiền ra mà không muốn dùng app có tài khoản

---

## Key Features

| Feature | Priority | Status |
|---------|----------|--------|
| Thêm / sửa / xóa giao dịch (thu & chi) | 🔴 High | ⏳ TODO |
| Tổng thu – tổng chi – số dư | 🔴 High | ⏳ TODO |
| Lọc theo tháng / loại / tìm kiếm ghi chú | 🔴 High | ⏳ TODO |
| Lưu trữ localStorage (tự khôi phục sau reload) | 🔴 High | ⏳ TODO |
| Biểu đồ cột thu–chi 6 tháng gần nhất | 🟡 Medium | ⏳ TODO |
| Chi tiêu theo danh mục (thanh ngang + %) | 🟡 Medium | ⏳ TODO |
| Xuất / nhập JSON backup | 🟡 Medium | ⏳ TODO |
| Xóa toàn bộ dữ liệu (có xác nhận) | ⚪ Low | ⏳ TODO |

---

## Constraints & Limitations

- **Không có backend.** Không API, không DB, không auth, không đồng bộ nhiều thiết bị.
- **Không thêm thư viện ngoài** cho chart / date / UI kit (recharts, chart.js, date-fns, moment, MUI...). Biểu đồ vẽ bằng CSS/div.
- **Dữ liệu là của người dùng:** chỉ nằm trong `localStorage` key `expense-tracker:v1`, dạng `{ transactions: Transaction[] }`. Không gửi đi đâu cả.
- **Không có deadline cứng.** Human quyết định khi nào bắt đầu code.

---

## Quy Tắc Bất Biến (Invariants — KHÔNG được phá)

1. **Chống hydration mismatch:** KHÔNG đọc `localStorage` trong thân component hay trong initializer của `useState`. Chỉ đọc trong `useEffect`, kèm cờ `hydrated` trước khi render số liệu.
2. **Tiền tệ:** `amount` lưu là **số nguyên VND**. Hiển thị bằng `Intl.NumberFormat('vi-VN', { style: 'currency', currency: 'VND', maximumFractionDigits: 0 })`.
3. **Ngày:** lưu dạng chuỗi `'YYYY-MM-DD'` (giờ local, không dùng `toISOString()` vì lệch múi giờ).
4. **Ranh giới server/client:** `app/page.tsx` và `app/layout.tsx` giữ **server component**. Mọi component có state/handler phải có `'use client'` ở đầu file.
5. **Không hard-code màu rải rác:** màu theme (thu = xanh, chi = đỏ/cam, nền tối) khai báo một chỗ trong `app/globals.css`.
6. **ID giao dịch:** dùng `crypto.randomUUID()`, có hàm fallback cho môi trường không hỗ trợ.

---

*SOUL.md — Đọc đầu tiên khi bắt đầu phiên làm việc.*

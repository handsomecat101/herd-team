# ISSUE_LOG.md - Sổ Tay Miễn Dịch Lỗi Kỹ Thuật

> **QUY TẮC GHI LỖI TIẾT KIỆM TOKEN (BẮT BUỘC):**
> 1. **Khi khởi động (Bước 0):** chỉ đọc **Bảng Tra Cứu Nhanh (Quick Index)** ngay dưới đây. Chỉ đọc phần chi tiết khi làm việc đụng tới file/lỗi liên quan.
> 2. **Chỉ ghi lỗi có bài học kỹ thuật**, mỗi lỗi **5–8 dòng** theo cấu trúc: Triệu chứng → Nguyên nhân gốc → Cách fix & quy tắc tránh lặp lại.
> 3. **Không chép lặp sang `AGENT_CHAT.md`.** Chi tiết lỗi chỉ nằm duy nhất ở file này.

---

## ⚡ Bảng Tra Cứu Nhanh (Quick Index)

| ID | Lỗi / Triệu chứng ngắn | File liên quan | Quy tắc tránh lặp lại (1 câu) | Status |
|----|------------------------|----------------|-------------------------------|--------|
| `BUG-001` | `monthLabel('')` → `"T0"`, `monthLabel('abc')` → `"TNaN"` | `lib/format.ts` | Hàm format nhận input ngoài luôn phải validate bằng regex **trước** khi `Number()` | ✅ Fixed |
| `BUG-002` | Nguy cơ hydration mismatch khi render tiền bằng `Intl.NumberFormat` | `lib/format.ts`, `components/ExpenseApp.tsx` | Không render số liệu ở lần render đầu; chỉ render sau cờ `hydrated` | ✅ Prevented |
| `BUG-003` | Đóng pane agent giữa chừng → agent mất sạch context, phải brief lại | — | **Không đóng pane** của agent đang làm việc | ⚠️ Quy trình |
| `BUG-004` | herdr `revision` không đổi với pane agent `unknown` → monitor báo "settled" sai | — | Không dùng pane `revision` làm tín hiệu hoàn thành; dùng tín hiệu nghiệp vụ (port tắt, file đủ) | ⚠️ Quy trình |
| `BUG-005` | Bấm nút tải file bằng `agent-browser click` → download bị cancel | — | Dùng `agent-browser download <@ref> <path>` (hoặc `--download-path`) thay vì `click` | ✅ Fixed |

---

## 🔍 Chi Tiết Các Lỗi (Mới nhất ở trên)

### [BUG-001] `monthLabel` trả về `T0` / `TNaN` với input sai định dạng
- **Ngày:** 2026-10-19 | **Phát hiện:** MiniMax (Track C) | **Fix bởi:** MiniMax | **Status:** ✅ Fixed
- **File:** `lib/format.ts` | **Screenshot:** —
- **Nguyên nhân gốc:** `Number(key.slice(5, 7))` — với chuỗi rỗng `Number('')` = `0` → `"T0"`; với chuỗi rác → `NaN` → `"TNaN"`. Hàm không kiểm tra định dạng đầu vào.
- **Cách fix & Bài học:**
  - Thêm regex `/^(\d{4})-(\d{2})$/` + guard tháng `01–12`, sai thì trả `''`.
  - **Quy tắc:** mọi hàm format nhận input từ ngoài (localStorage, file nhập, URL) phải validate trước khi ép kiểu số.

### [BUG-002] Rủi ro hydration mismatch khi render tiền tệ
- **Ngày:** 2026-10-19 | **Phát hiện:** MiniMax (Track C) | **Status:** ✅ Prevented (đã xác nhận 0 warning khi test thật)
- **File:** `lib/format.ts`, `components/ExpenseApp.tsx` | **Screenshot:** —
- **Nguyên nhân gốc:** `Intl.NumberFormat('vi-VN')` có thể sinh ký tự trắng khác nhau giữa Node và Chrome (`U+00A0` vs `U+202F`) trước `₫`. Next.js render sẵn HTML ở server → nếu chuỗi khác nhau, React cảnh báo hydration.
- **Cách fix & Bài học:**
  - Thêm luật vào `CONTRACT.md` §7: render số liệu **chỉ sau** cờ `hydrated` (`useEffect`), lần render đầu hiển thị `—`.
  - Không gọi `new Date()` / `lastMonthsBuckets` / `categoryBuckets` trong thân render.
  - **Quy tắc:** mọi thứ phụ thuộc môi trường (locale, timezone, `Date`) phải nằm sau mốc hydrate.

### [BUG-003] Mất context khi pane bị đóng giữa chừng
- **Ngày:** 2026-10-19 | **Status:** ⚠️ Quy trình
- **Triệu chứng:** `agy` đang làm Track A thì pane `w3:p7` biến mất; agent mới ở `w3:p9` không nhớ gì.
- **Nguyên nhân gốc:** context của agent nằm trong tiến trình của pane, không nằm trên đĩa.
- **Bài học:** không đóng pane của agent đang làm việc. Nếu buộc phải đổi pane, phải **brief lại từ đầu** (đọc kit + nêu rõ file nào còn thiếu) — nói "làm tiếp" là vô nghĩa.

### [BUG-004] Monitor báo hoàn thành sai
- **Ngày:** 2026-10-19 | **Status:** ⚠️ Quy trình
- **Triệu chứng:** monitor báo `PANE_SETTLED` trong khi agent vẫn đang chạy.
- **Nguyên nhân gốc:** dùng `revision` của pane làm tín hiệu "output đã đứng yên". Với agent herdr không nhận diện được (trạng thái `unknown`), `revision` **không được cập nhật** → đứng yên giả.
- **Bài học:** chỉ dùng tín hiệu **nghiệp vụ** để biết xong: tiến trình con đã tắt, file đích đã đủ, hoặc trạng thái `idle`/`done` do herdr phân loại được.

### [BUG-005] Tải file bằng `agent-browser click` bị cancel
- **Ngày:** 2026-10-19 | **Status:** ✅ Fixed
- **Triệu chứng:** bấm nút "Xuất JSON" bằng `click` → download bị cancel, không có file.
- **Nguyên nhân gốc:** `click` không chờ/không nhận luồng download của trình duyệt.
- **Bài học:** dùng `agent-browser download <@ref> <path>` (hoặc đặt `--download-path`) cho mọi thao tác tải file; dùng `upload <@ref> <file>` cho nhập file.

---

*ISSUE_LOG.md — Luôn cập nhật Quick Index ở đầu file mỗi khi thêm lỗi mới.*

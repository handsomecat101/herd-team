# ISSUE_LOG.md — Sổ Tay Miễn Dịch Lỗi

**🧩 BẢN MẪU** — đây là template rỗng, không phải dự án thật. Điền theo hướng dẫn trong file, rồi xoá dòng này.

> **Chỉ Coordinator ghi file này.**
> Chỉ ghi lỗi **có bài học kỹ thuật**. 5–8 dòng / lỗi.
> Không chép lại nội dung này sang file khác.

---

## Cách dùng

- **Lúc khởi động:** chỉ đọc **bảng Quick Index**. Mất 5 giây là biết toàn bộ cái bẫy của dự án.
- **Khi làm việc:** đụng tới file nào thì tra dòng tương ứng rồi mới đọc phần chi tiết.

---

## Quick Index

| ID | Triệu chứng ngắn | File liên quan | Quy tắc tránh lặp lại (1 câu) | Status |
|----|------------------|----------------|-------------------------------|--------|
| `BUG-001` | [triệu chứng, 1 dòng] | `[file]` | [luật 1 câu] | ✅ Fixed |
| `BUG-002` | | | | 🔴 Open |

Trạng thái: `🔴 Open` · `🟡 Fixing` · `✅ Fixed` · `⚠️ Quy trình`

> `⚠️ Quy trình` dùng cho lỗi **cách làm việc**, không phải lỗi code — ví dụ đóng pane
> giữa chừng làm mất context. Cũng đáng ghi: nó tốn thời gian như bug thật.

---

## Chi tiết

### [BUG-001] [Tên lỗi ngắn]

- **Ngày:** [YYYY-MM-DD] · **Phát hiện:** [ai] · **Sửa:** [ai] · **Status:** ✅ Fixed
- **File:** `[đường dẫn]`
- **Nguyên nhân gốc:** [1–2 câu, chỉ thẳng vào cơ chế — không mô tả lại triệu chứng]
- **Cách sửa & bài học:**
  - [đã sửa thế nào]
  - **Quy tắc:** [điều cấm làm / phải làm để không tái phát]

---

## Mẫu một entry tốt

### [BUG-001] Hàm ép kiểu số nhận chuỗi rỗng trả `0` thay vì báo lỗi

- **Ngày:** [YYYY-MM-DD] · **Phát hiện:** [người kiểm tra] · **Sửa:** [người kiểm tra] · **Status:** ✅ Fixed
- **File:** `[đường dẫn]`
- **Nguyên nhân gốc:** ép kiểu trên chuỗi rỗng trả `0` thay vì báo lỗi, nên giá trị sai lọt vào
  hệ thống mà trông như giá trị hợp lệ. Hàm không kiểm tra định dạng trước khi ép kiểu.
- **Cách sửa & bài học:**
  - Thêm kiểm tra định dạng trước khi ép kiểu; sai thì trả giá trị rỗng và báo lỗi.
  - **Quy tắc:** mọi hàm nhận dữ liệu từ ngoài (bộ nhớ trình duyệt, file nhập, tham số dòng lệnh,
    tham số URL) đều phải kiểm tra định dạng trước khi ép sang kiểu số.

---

> Điểm mấu chốt của một entry tốt: **nguyên nhân gốc chỉ thẳng vào cơ chế**, và
> **bài học viết thành luật** để lần sau tra ra là làm được ngay.

---

*ISSUE_LOG.md — Càng để lâu càng đắt giá. Cập nhật Quick Index mỗi lần thêm lỗi.*

# SOUL.md — Dự Án Là Gì + Quy Tắc Bất Biến

> Đọc **đầu tiên**. File này cố định, ít khi sửa.
> Ngân sách: ~60 dòng. Dài hơn là agent mới phải trả thuế context mỗi phiên.

---

## Identity

| Field | Value |
|-------|-------|
| **Tên dự án** | [tên] |
| **Loại** | Web app / CLI / API / Thư viện |
| **Mô tả 1 câu** | [làm được gì, cho ai] |
| **Tech stack** | [ngôn ngữ, framework, thư viện chính] |
| **Lưu trữ** | [localStorage / SQLite / Postgres / không có] |
| **Chạy local** | [lệnh + địa chỉ] |

---

## Mục tiêu

**Mục tiêu chính:** [1–2 câu]

**Tiêu chí thành công** — kiểm chứng được, không cảm tính:

- [ ] [ví dụ: build pass, không cảnh báo ở console]
- [ ] [ví dụ: nhập 3 bản ghi rồi tải lại trang vẫn còn dữ liệu]
- [ ] [ví dụ: lọc theo tháng ra đúng tập kết quả]

---

## Ràng buộc

- **Thời gian:** [mốc, nếu có]
- **Phụ thuộc:** [API ngoài, dịch vụ, giấy phép]
- **Không được dùng:** [thư viện, dịch vụ bị cấm và lý do]

---

## Quy Tắc Bất Biến

> Phần quan trọng nhất của file. Đây là những điều **không được phá**, kể cả khi
> có vẻ tiện hơn. Agent mới đọc 30 giây là biết mình không được làm gì.

Viết theo dạng: **luật** + **vì sao** + **cách kiểm**.

```markdown
1. [Luật] — vì [lý do]. Kiểm bằng [cách].
```

Ví dụ thật:

1. **Không đọc `localStorage` trong initializer của `useState`** — vì server không có
   `localStorage`, React sẽ cảnh báo hydration mismatch. Đọc trong `useEffect`, kèm cờ `hydrated`.
2. **Lưu ngày dạng `'YYYY-MM-DD'` theo giờ local** — vì `toISOString()` đổi sang UTC và
   giao dịch sẽ nhảy sang ngày khác. Kiểm bằng cách nhập giao dịch lúc 23:00.
3. **Không thêm thư viện ngoài** — vì [lý do]. Muốn thêm thì hỏi Human.
4. **`amount` là số nguyên, đơn vị nhỏ nhất của tiền tệ** — vì số thực gây sai số làm tròn.

---

*SOUL.md — Sửa file này là thay đổi hợp đồng với mọi agent. Báo Coordinator.*

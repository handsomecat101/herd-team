# SOUL.md — Dự Án Là Gì + Quy Tắc Bất Biến

**🧩 BẢN MẪU** — đây là template rỗng, không phải dự án thật. Điền theo hướng dẫn trong file, rồi xoá dòng này.

> Đọc **đầu tiên**. File này cố định, ít khi sửa.
> Ngân sách: ~60 dòng. Dài hơn là agent mới phải trả thuế context mỗi phiên.

---

## Identity

| Field | Value |
|-------|-------|
| **Tên dự án** | [tên] |
| **Loại** | [web app / CLI / API / thư viện] |
| **Mô tả 1 câu** | [làm được gì, cho ai] |
| **Tech stack** | [ngôn ngữ, framework, thư viện chính] |
| **Lưu trữ** | [dữ liệu nằm ở đâu — hoặc "không có"] |
| **Chạy local** | [lệnh + địa chỉ] |

---

## Mục tiêu

**Mục tiêu chính:** [1–2 câu]

**Tiêu chí thành công** — kiểm chứng được, không cảm tính:

- [ ] [tiêu chí 1 — viết sao cho có lệnh hoặc thao tác kiểm được]
- [ ] [tiêu chí 2]
- [ ] [tiêu chí 3]

---

## Ràng buộc

- **Thời gian:** [mốc, nếu có]
- **Phụ thuộc:** [API ngoài, dịch vụ, giấy phép]
- **Không được dùng:** [thứ bị cấm và lý do]

---

## Quy Tắc Bất Biến

> Phần quan trọng nhất của file. Đây là những điều **không được phá**, kể cả khi
> có vẻ tiện hơn. Agent mới đọc 30 giây là biết mình không được làm gì.

Viết mỗi luật theo dạng: **[luật] — vì [lý do]. Kiểm bằng [cách]**.

1. **[luật 1]** — vì [lý do]. Kiểm bằng [cách].
2. **[luật 2]** — vì [lý do]. Kiểm bằng [cách].
3. **[luật 3]** — vì [lý do]. Kiểm bằng [cách].

### Gợi ý dạng luật hay gặp

Chọn cái nào đúng với dự án của bạn:

- **Môi trường chạy** — chỗ nào có thứ gì, chỗ nào không có
  (server không có API của trình duyệt, CLI không có biến môi trường của web…)
- **Định dạng dữ liệu lưu trữ** — kiểu ngày, múi giờ, đơn vị tiền, cách mã hoá
- **Phụ thuộc bị cấm** — thư viện không được thêm, và lý do
- **Ranh giới giữa các tầng** — tầng nào được gọi tầng nào

> Luật không có "vì" thì vài tháng sau sẽ bị phá. Luật không có "kiểm bằng" thì không ai
> biết mình đã phá hay chưa.

---

*SOUL.md — Sửa file này là thay đổi hợp đồng với mọi agent. Báo Coordinator.*

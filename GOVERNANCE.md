# GOVERNANCE.md — Phân Quyền & Luật Phối Hợp

> Không có phần này thì hai agent sẽ sửa cùng một file và ghi đè nhau.

---

## 1. Luật Một Người Ghi

**Mỗi file chỉ có đúng một người được ghi.**

| File | Người ghi duy nhất |
|------|--------------------|
| `STATE.md`, `DECISIONS.md`, `ISSUE_LOG.md`, `CONTRACT.md`, `GOVERNANCE.md` | **Coordinator** |
| `SOUL.md` | Coordinator (đổi là đổi hợp đồng với mọi agent) |
| Code — `src/**`, `app/**`, `lib/**` | Worker theo phân mảng |

Worker **không ghi file kit**. Worker báo cáo bằng lời ở tin nhắn cuối; Coordinator ghi sổ.

> Lý do: nhiều agent ghi cùng file thì thao tác nào tới sau sẽ thắng, và nội dung
> của người trước biến mất im lặng. Đây là lỗi đã xảy ra thật, không phải phòng xa.

---

## 2. Phân Mảng Theo Đường Dẫn

Chia việc theo **file**, không chia theo "chức năng".

| Mảng | Được sửa | KHÔNG được sửa |
|------|----------|-----------------|
| Coordinator | file kit `*.md` | **toàn bộ code** |
| Worker A | `[đường dẫn A]` | ngoài đường dẫn A |
| Worker B | `[đường dẫn B]` | ngoài đường dẫn B |
| Người kiểm tra | **không sửa gì** — chỉ đọc và chạy lệnh | mọi file |

Hai mảng giao nhau tại `CONTRACT.md`. Không ai được sửa contract mà không báo Coordinator.

---

## 3. Ranh Giới Cứng

- **Coordinator không viết code.** Viết code rồi thì không còn ai kiểm tra được.
- **Không tự mở rộng quyền.** Cần thêm quyền thì hỏi Human.
- **Không tin báo cáo suông.** Xong việc phải có lệnh verify + kết quả thật.
- **Không đóng pane** của agent đang làm việc.
- **Tuần tự hoá bước cài đặt.** Hai agent cùng chạy cài đặt/build sẽ tranh chấp thư mục phụ thuộc.

---

## 4. Mức Phê Duyệt

| Mức | Khi nào | Hành động |
|-----|---------|-----------|
| **INFO** | Sửa nhỏ trong phạm vi mình | Tự làm, ghi `STATE.md` |
| **CONFIRM** | Đổi cấu trúc, thêm dependency, đổi tech stack | Hỏi Human trước |
| **APPROVE** | Xoá dữ liệu người dùng, đổi khoá lưu trữ, thay đổi phá tương thích | **Bắt buộc** chờ Human |
| **ESCALATE** | Nguy cơ mất dữ liệu, lỗi không rõ nguyên nhân | Dừng ngay, báo Human |

---

## 5. Git

Commit khi: bug fix xong, feature xong, tài liệu kit đổi.

```
[PREFIX]: [mô tả ngắn]

Feat: thêm form nhập giao dịch
Fix: chặn hydration mismatch khi đọc localStorage
Docs: khoá T-005 và ghi bàn giao
```

Không commit code chưa chạy được. Không đẩy thẳng lên nhánh chính nếu dự án có yêu cầu review.

---

*GOVERNANCE.md — Luật chơi. Vi phạm thì lỗi không phải của agent, mà của người đặt luật.*

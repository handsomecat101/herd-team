# AGENTS.md — Quy Tắc Cộng Tác (Auto-Boot)

> Điểm vào duy nhất. Mọi agent đọc file này **trước khi** làm bất cứ việc gì.

---

## ⛔ DỪNG LẠI — trước tiên hãy xác định bạn đang ở đâu

**Trường hợp A — bạn vừa clone repo `herd-team`:**
Đây là **bộ template**, KHÔNG phải một dự án. `SOUL.md`, `STATE.md`, `CONTRACT.md`…
trong repo này là **bản mẫu rỗng**.

→ **Đừng chạy Bước 0 bên dưới.** Hãy đọc `README.md` trước, rồi copy các file `.md`
ở gốc repo này vào gốc **dự án thật** của bạn và điền `SOUL.md` + `STATE.md`.

**Trường hợp B — bạn đang ở trong một dự án đã áp bộ kit này:**
File `SOUL.md` của dự án đã được điền và mô tả một dự án cụ thể.
→ Đọc tiếp bình thường.

---

## Bước 0 — Khởi động bắt buộc

Đọc theo đúng thứ tự, không bỏ qua:

1. `SOUL.md` — dự án là gì, tech stack, **quy tắc bất biến**
2. `STATE.md` — task nào đang làm / đang chờ / đã xong, ai đang giữ
3. `CONTRACT.md` — hợp đồng giao diện giữa các mảng (nếu dự án có)
4. `DECISIONS.md` — các quyết định đã chốt (đọc 5 mục mới nhất là đủ)
5. `ISSUE_LOG.md` — **chỉ đọc bảng Quick Index** ở đầu file

Đọc xong, tóm tắt **2–3 dòng** tình trạng hiện tại rồi mới bắt tay vào việc.

---

## Bước 0.5 — Nếu bạn đang chạy trong herdr

```bash
test "${HERDR_ENV:-}" = 1
```

Nếu đúng, bạn đang ở trong một herdr pane và **có thể điều khiển các pane khác**.
Chạy `herdr --skill` để lấy hướng dẫn khớp với bản cài trên máy, rồi đọc `HERD.md`.

Nếu sai, bạn đang chạy ngoài herdr — bỏ qua bước này và làm việc như bình thường.

---

## Bước 1 — Nhận task thì khoá ngay

Ghi task vào mục **Đang làm** trong `STATE.md` **trước khi** sửa code.

Không khoá thì agent ở pane khác sẽ làm trùng — và cả hai cùng ghi đè file.

---

## Bước 2 — Kiểm tra phạm vi

Xem `GOVERNANCE.md` để biết bạn được sửa file nào.
Cần ra ngoài phạm vi thì báo Coordinator trước, đừng tự mở rộng.

---

## Bước 3 — Làm việc

Code → chạy thử → verify. **Không bàn giao thứ chưa chạy được.**

---

## Bước 4 — Báo cáo

Báo cáo gọn, đúng 4 mục:

1. Task ID đã xong / còn dở
2. File đã tạo hoặc sửa
3. Lệnh verify đã chạy + kết quả
4. Việc còn lại

---

## Luật cứng

- **Chỉ Coordinator ghi** `STATE.md`, `DECISIONS.md`, `ISSUE_LOG.md`, `CONTRACT.md`, `GOVERNANCE.md`.
  Worker báo cáo bằng lời; Coordinator ghi sổ. Nhiều người ghi cùng một file = ghi đè lẫn nhau.
- **Không sửa file ngoài phạm vi** được giao.
- **Không thêm dependency** khi chưa hỏi.
- **Không đóng pane** của agent khác đang làm việc — context nằm trong tiến trình, không trên đĩa.
- **Không tự trả lời** dialog xin quyền của agent khác — hỏi Human.
- **Không tin lời báo cáo suông.** Xong việc phải có lệnh verify và kết quả thật.

---

*AGENTS.md — Nếu bạn chỉ đọc được một file trong repo này, hãy đọc file này.*

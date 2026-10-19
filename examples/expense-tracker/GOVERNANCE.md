# GOVERNANCE.md - Luật & Quy Tắc Phối Hợp

> Quy tắc chung mà TẤT CẢ agent trên mọi IDE phải tuân theo.

---

## 1. Quy Tắc Chung

### 1.1 Trước Khi Bắt Đầu (Bước 0 — Mỗi phiên mới / Đổi IDE)

| Thứ tự | Hành động | Lưu ý tiết kiệm token |
|--------|-----------|------------------------|
| 1 | Đọc `SOUL.md` | Nắm tech stack + quy tắc bất biến |
| 2 | Đọc `PROJECT_STATE.md` | Xem task nào đang khóa, task nào chờ |
| 3 | Đọc `AGENT_CHAT.md` | Đọc 5–8 tin gần nhất |
| 4 | Đọc Quick Index trong `ISSUE_LOG.md` | Chỉ đọc phần chi tiết khi cần |

### 1.2 Trong Khi Làm Việc

- ✅ **Khóa task ngay (Bước 1):** ghi `🟡 In Progress` vào `PROJECT_STATE.md` **trước khi** sửa code.
- ✅ **Chỉ sửa file thuộc task được giao** (theo bảng phân quyền mục 2).
- ✅ **Test trước khi bàn giao** — không bàn giao code chưa chạy được.
- ✅ **Giữ đúng invariants trong `SOUL.md`.**

- ❌ **Không sửa đè file agent khác đang khóa (`🟡 In Progress`).**
- ❌ **Không bàn giao code chưa test.**
- ❌ **Không thêm thư viện ngoài** (chart/date/UI kit) khi chưa xin phép Human.
- ❌ **Không bắt đầu code khi Human chưa xác nhận.**

### 1.3 Sau Khi Xong

- ✅ `PROJECT_STATE.md`: chuyển task sang `✅ Task Hoàn Thành` — **1 dòng**.
- ✅ `AGENT_CHAT.md`: thêm tin `@done` / `@handoff` — **3–7 dòng**, giữ ≤ 8 tin.
- ✅ `ISSUE_LOG.md`: nếu có bug có bài học → thêm entry + cập nhật Quick Index (**không copy sang `AGENT_CHAT.md`**).
- ✅ Git commit khi được yêu cầu.

---

## 2. Phân Quyền File (Agent Roles & Constraints)

| Agent | Pane (herdr) | Vai trò | Được phép sửa | KHÔNG được sửa |
|-------|--------------|---------|---------------|-----------------|
| **Command Code** (`cmd`) | `w3:p3` | **Coordinator** — điều phối, giao việc, khóa task | Chỉ file kit `*.md` (`AGENTS`, `SOUL`, `PROJECT_STATE`, `AGENT_CHAT`, `ISSUE_LOG`, `GOVERNANCE`, `QUY_TRINH`) + `CONTRACT.md` | **Toàn bộ code**: `app/**`, `components/**`, `lib/**` |
| **Antigravity** (`agy`) | `w3:p7` | **Track A** — khung dự án + UI | `app/**`, `components/**`, `package.json`, `tsconfig.json` | `lib/**`, file kit |
| **OpenCode** (`opencode`) | `w3:p2` | **Track B** — lớp dữ liệu | `lib/**` | `app/**`, `components/**`, file kit |
| **Codex / Cursor** | *chưa có trong herd* | Review, kiểm thử, tối ưu | Toàn bộ code **ở chế độ đề xuất**; chỉ sửa khi được Coordinator assign | File agent khác đang lock |
| **Human** | — | Product Owner, duyệt | Tất cả | — |

> 🎯 **Ba nguyên tắc tách vai:**
> 1. Coordinator **không tự code**. Mọi thay đổi code do Agent Dev thực hiện.
> 2. **Chỉ Coordinator ghi file kit** (`PROJECT_STATE.md`, `AGENT_CHAT.md`, `ISSUE_LOG.md`, `GOVERNANCE.md`, `CONTRACT.md`). Agent Dev báo cáo kết quả ở tin nhắn cuối, Coordinator ghi sổ — tránh 2 agent ghi đè nhau.
> 3. Hai track gặp nhau tại `CONTRACT.md`. **Không ai được sửa `CONTRACT.md`** — muốn đổi phải báo Coordinator.
>
> 📡 **Giao việc qua herdr, không qua file:** `herdr agent prompt w3:p7 "…"` và `herdr agent prompt w3:p2 "…"`. Luôn gọi bằng **pane ID** (có 2 pane opencode).

### Khi cần sửa file ngoài phạm vi
1. Báo trước trong `AGENT_CHAT.md` (`@help` / `@assign`) hoặc hỏi Human.
2. Chờ đồng ý rồi mới thực hiện.

---

## 3. Git Rules

### 3.1 Khi nào commit

| Hành động | Thời điểm commit |
|-----------|------------------|
| Fix bug / Feature | Sau khi test pass, `tsc --noEmit` sạch |
| Cập nhật tài liệu kit | Ngay sau khi sửa `*.md` của kit |
| Deploy | Ngay sau khi verify thành công |

### 3.2 Commit message

```
[PREFIX]: [Mô tả ngắn gọn]

Ví dụ:
- Feat: add transaction form with income/expense toggle
- Fix: prevent hydration mismatch when reading localStorage
- UI: style summary cards for dark theme
- Docs: lock T-005 and log handoff
```

---

## 4. Quy Tắc Giao Tiếp Giữa Các IDE

### 4.1 Phân vai file (Zero-Duplication)

| File | Mục đích duy nhất | Giới hạn |
|------|-------------------|----------|
| `SOUL.md` | Kiến trúc, tech stack, ràng buộc cố định | ~50–80 dòng |
| `PROJECT_STATE.md` | Dashboard trạng thái task | 1 dòng / task, < 100 dòng |
| `AGENT_CHAT.md` | Tin bàn giao ca trực | 3–7 dòng / tin, 5–8 tin |
| `ISSUE_LOG.md` | Quick Index + nguyên nhân gốc & cách fix | 5–8 dòng / lỗi |

### 4.2 Các tag dùng trong `AGENT_CHAT.md`

- **`@done`** — báo hoàn thành task trong ca.
- **`@handoff`** — làm dở, dừng phiên hoặc đổi IDE, cần ghi rõ đang ở đâu.
- **`@assign → [Agent]`** — giao việc cụ thể cho agent tiếp theo.
- **`@help`** — vướng mắc, cần agent khác hoặc Human hỗ trợ.
- **`@approve`** — xin duyệt thay đổi quan trọng.

---

## 5. Approval Levels

| Level | Khi nào | Hành động |
|-------|---------|-----------|
| **INFO** | Sửa nhỏ, cập nhật tài liệu kit | Tự làm & ghi bàn giao |
| **CONFIRM** | Đổi cấu trúc thư mục, thêm dependency, đổi tech stack | Hỏi Human trước |
| **APPROVE** | Xóa dữ liệu người dùng, đổi key localStorage, thay đổi phá vỡ tương thích | **BẮT BUỘC** chờ Human duyệt |
| **ESCALATE** | Nguy cơ mất dữ liệu, lỗi nghiêm trọng không rõ nguyên nhân | Dừng ngay, báo Human |

> ⚠️ **Trạng thái hiện tại (2026-10-19):** Human đã bật đèn xanh. `T-001`, `T-003` → `T-011` đang chạy song song trên 2 pane. `T-012` **chưa có agent nhận** — vẫn ở mức CONFIRM, cần Human quyết định ai chạy.

---

*GOVERNANCE.md — Giữ kỷ luật chung để mọi agent từ mọi IDE phối hợp mượt mà, không trùng lặp.*

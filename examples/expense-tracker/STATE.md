# PROJECT_STATE.md - Bảng Trạng Thái Dự Án (Dashboard)

> **QUY TẮC TIẾT KIỆM TOKEN (BẮT BUỘC):**
> 1. File này **CHỈ chứa bảng trạng thái** — mỗi task 1 dòng ngắn gọn.
> 2. **CẤM** viết nhật ký chi tiết theo giờ hay phân tích lỗi ở đây (trùng với `AGENT_CHAT.md` / `ISSUE_LOG.md`).
> 3. **Khóa task ngay ở Bước 1.** Chỉ **Coordinator** được ghi file này.

---

## Project Info

| Field | Value |
|-------|-------|
| **Project Name** | Expense Tracker (Quản lý chi tiêu) |
| **Ngày bắt đầu** | 2026-10-19 |
| **Cập nhật gần nhất** | 2026-10-19 |
| **Trạng thái** | ✅ HOÀN THÀNH — smoke test bằng browser thật PASS toàn bộ |

## URLs & Endpoints

| Service | URL / Path |
|---------|------------|
| **Local dev** | `http://localhost:3000` (chạy `npm run dev`; hiện port trống) |
| **Storage** | Browser `localStorage` — key `expense-tracker:v1` |
| **Backup** | File JSON do người dùng tự xuất/nhập |

---

## Thành Viên (Herd thật — workspace `w3`, tab `w3:t2`)

| Agent | Pane | Vai trò | Status | Ghi chú |
|-------|------|---------|--------|---------|
| **Command Code** (`cmd`) | `w3:p3` | **Coordinator** — điều phối, ghi dashboard. **Không viết code.** | 🟡 Active | Đầu mối duy nhất ghi file kit |
| **Antigravity** (`agy`) | `w3:p9` | **Track A — UI** | ⚪ Idle | Xong toàn bộ UI (`T-001`, `T-005`→`T-011`) |
| **OpenCode** (`opencode`) | `w3:p2` | **Track B — `lib/**`** | ⚪ Idle | `T-003`, `T-004` ✅ |
| **Command Code / MiniMax** (`M3.1-Flash`) | `w3:pA` | **Track C — Reviewer + Tester** | ⚪ Idle | `T-012`, `T-012b`, `T-014` ✅ |
| **Human** | — | Product Owner, duyệt | 👤 Active | |

> ⚠️ `w3:p7` (agy cũ) **đã đóng** — agy hiện ở `w3:p9`.
> ⚠️ Có **2 pane opencode** (`w3:p2` = dự án này, `w3:p4` = `ai vision`). Luôn gọi bằng **pane ID**.
> ✅ `agent-browser 0.39.1` + Chrome `155.0.8059.84` đã cài.
> ⚠️ Skill `herdr` **chưa được cài** ở đâu — `herdr --skill` in ra bản bundled nếu cần.

---

## 🟡 Task Đang Làm (In Progress — Lock)

| ID | Task | Agent | Priority | Status | Files đang sửa |
|----|------|-------|----------|--------|----------------|
| T-013 | Bàn giao cuối + tổng kết | Coordinator | ⚪ LOW | 🟡 In Progress | — |

---

## ⏳ Task Chờ (Backlog / TODO)

| ID | Task | Agent dự kiến | Priority | Ghi chú ngắn |
|----|------|---------------|----------|--------------|
| — | *Không còn task nào* | — | — | — |

> **Ranh giới:** Track A chỉ `app/**` + `components/**`; Track B chỉ `lib/**`; Track C **chỉ đọc + chạy lệnh verify/test**, không sửa code. Gặp nhau tại `CONTRACT.md`.

---

## ✅ Task Hoàn Thành (1 dòng / task)

| ID | Task | Agent | Ngày xong | Kết quả (1 câu ngắn) |
|----|------|-------|-----------|----------------------|
| T-000 | Dựng 7 file kit agent-team | Coordinator | 2026-10-19 | Đủ 7 file tài liệu khởi điểm |
| T-002 | Chốt vai trò + `CONTRACT.md` + chia track | Coordinator | 2026-10-19 | Contract freeze; chia Track A/B/C |
| T-001 | Khởi tạo Next.js + TS + Tailwind + alias | agy | 2026-10-19 | `next build` pass 4/4 static pages |
| T-003 | `lib/types.ts` + `categories.ts` + `format.ts` | opencode | 2026-10-19 | 3 file, export khớp contract |
| T-004 | `lib/storage.ts` + `lib/stats.ts` | opencode | 2026-10-19 | 2 file |
| T-005 | `components/TransactionForm.tsx` | agy | 2026-10-19 | Form thêm/sửa giao dịch |
| T-006 | `components/TransactionList.tsx` + `Filters.tsx` | agy | 2026-10-19 | Danh sách + lọc tháng/loại/tìm kiếm |
| T-007 | `components/SummaryCards.tsx` | agy | 2026-10-19 | Thẻ tổng thu/chi/số dư |
| T-008 | `components/BarChart.tsx` | agy | 2026-10-19 | Biểu đồ 6 tháng, vẽ bằng CSS |
| T-009 | `components/CategoryBreakdown.tsx` | agy | 2026-10-19 | Thanh ngang + % |
| T-010 | `components/ExpenseApp.tsx` + `app/page.tsx` + `app/layout.tsx` | agy | 2026-10-19 | State gốc + localStorage + xuất/nhập JSON |
| T-011 | Theme + responsive + trạng thái rỗng | agy | 2026-10-19 | `app/globals.css` |
| T-012 | Review `lib/**` vs `CONTRACT.md` | MiniMax | 2026-10-19 | Tìm ra lỗi `monthLabel` + rủi ro hydration `formatVND` |
| T-012b | `tsc --noEmit` + `npm run build` + soi `ExpenseApp.tsx` theo §7 | MiniMax | 2026-10-19 | Cả 2 lệnh pass; `ExpenseApp` đúng §7 |
| T-014 | Smoke test bằng browser thật | MiniMax | 2026-10-19 | 7/7 mục PASS; **0 cảnh báo hydration**; round-trip JSON khớp |

---

## 🔴 Blockers

| Vấn đề đang chặn | Agent | Cần ai hỗ trợ | Tham chiếu |
|------------------|-------|---------------|------------|
| — | — | — | — |

---

*PROJECT_STATE.md — Giữ dưới 100 dòng để tối ưu context.*

# STATE.md — Bảng Trạng Thái Dự Án

**🧩 BẢN MẪU** — đây là template rỗng, không phải dự án thật. Điền theo hướng dẫn trong file, rồi xoá dòng này.

> **Chỉ Coordinator ghi file này.**
> Luật: **1 dòng / task**. Không nhật ký theo giờ, không phân tích lỗi ở đây.
> Ngân sách: dưới 100 dòng. Vượt là mỗi phiên khởi động đều phải trả giá.

---

## Thông tin

| Field | Value |
|-------|-------|
| **Cập nhật gần nhất** | [YYYY-MM-DD] |
| **Trạng thái** | 🟢 Đang chạy / 🟡 Chờ duyệt / 🔴 Kẹt / ✅ Xong |

---

## Đội hình

| Agent | Pane | Vai trò | Trạng thái | Được sửa |
|-------|------|---------|------------|----------|
| [Coordinator] | `w1:p1` | Điều phối — **không viết code** | 🟡 Active | file kit |
| [Worker A] | `w1:p2` | [mảng] | ⚪ Idle | `[đường dẫn]` |
| [Human] | — | Duyệt, bật đèn xanh | 👤 Active | tất cả |

> Ghi **pane ID**, không chỉ ghi tên — tên có thể trùng giữa các dự án.
>
> Cột **Được sửa** là phạm vi được **GHI**, không phải phạm vi được **BIẾT**.
> Mọi agent đọc **toàn bộ** file kit — không ai có sổ riêng. Chia đường dẫn là để hai người
> không sửa trùng một file, không phải để che thông tin của nhau.

---

## Đang làm

| ID | Task | Agent | Ưu tiên | File đang sửa |
|----|------|-------|---------|----------------|
| — | *(chưa có task nào được khoá)* | — | — | — |

---

## Chờ

| ID | Task | Agent dự kiến | Ưu tiên | Ghi chú |
|----|------|---------------|---------|---------|
| T-001 | [tên task ngắn] | [agent] | 🔴 HIGH | [phụ thuộc gì] |

> **Đừng chia việc theo "chức năng" mơ hồ.** Chia theo **đường dẫn file** —
> hai agent không được sửa cùng một file.

---

## Xong

| ID | Task | Agent | Ngày | Kết quả (1 câu) |
|----|------|-------|------|------------------|
| — | — | — | — | — |

---

## Kẹt

| Vấn đề | Agent | Cần ai | Tham chiếu |
|--------|-------|--------|------------|
| — | — | — | — |

---

*STATE.md — Trạng thái sống của dự án. Coordinator là người duy nhất ghi.*

# Herd Team Kit

> Bộ khung Markdown để chạy một **đội agent** trong cùng một workspace terminal
> ([herdr](https://herdr.dev)) — với bộ nhớ ngoài nằm trên đĩa.

---

## Vì sao cần kit này

Agent mất context theo ba đường, và cả ba đều là chuyện thật, không phải giả định:

| Đường mất context | Hậu quả thực tế |
|---|---|
| **Pane bị đóng** | Agent mới ở pane khác không nhớ gì; nói "làm tiếp" là vô nghĩa |
| **Context window đầy → nén** | Chi tiết bị nén mất; agent vẫn tự tin như đang nhớ |
| **Đổi phiên / đổi máy** | Không ai biết dự án đang ở đâu, cái gì đã xong |

Thứ duy nhất sống sót qua cả ba đường là **file trên đĩa**. Kit này chính là bộ file đó —
không phải thủ tục hành chính, mà là bộ nhớ duy nhất của dự án.

---

## Kit này khác bộ `agent-team` cũ ở đâu

| | Bộ cũ (`agent-team`) | Kit này (`herd-team`) |
|---|---|---|
| Kênh giao việc | Ghi file, agent sau đọc | **Prompt trực tiếp qua herdr** |
| Vai trò của file | Vừa là kênh giao việc, vừa là bộ nhớ | **Chỉ còn là bộ nhớ** |
| Dùng khi | Agent ở các IDE rời, không liên lạc được | Agent chung một herdr workspace |
| Ai ghi file kit | Nhiều agent cùng ghi | **Duy nhất Coordinator** (chống ghi đè) |

Điểm mấu chốt: khi đã có herdr, handoff qua file là **thừa**. Giữ lại file để làm bộ nhớ,
bỏ đi vai trò "kênh giao việc" của nó.

---

## Bản đồ file

| File | Vai trò duy nhất | Ngân sách |
|---|---|---|
| `AGENTS.md` | Auto-boot — điểm vào duy nhất cho mọi agent | ~60 dòng |
| `SOUL.md` | Dự án là gì + **quy tắc bất biến** | ~60 dòng, cố định |
| `STATE.md` | Dashboard task: đang làm / chờ / xong | **1 dòng / task**, < 100 dòng |
| `CONTRACT.md` | Hợp đồng giao diện giữa các mảng code | Tùy dự án |
| `DECISIONS.md` | Nhật ký quyết định: **quyết gì, vì sao, đánh đổi gì** | 1–3 dòng / quyết định |
| `ISSUE_LOG.md` | Sổ lỗi: Quick Index + nguyên nhân gốc | 5–8 dòng / lỗi |
| `GOVERNANCE.md` | Phân quyền file, luật một-người-ghi, mức phê duyệt | ~80 dòng |
| `HERD.md` | Playbook điều phối bằng herdr | ~120 dòng |

**Ngân sách độ dài không phải hình thức.** Mỗi phiên mới đều phải đọc lại bộ kit.
File 500 dòng nghĩa là mỗi lần mở dự án, agent mất một khúc context chỉ để "nắm tình hình".

---

## Bắt đầu trong 5 phút

1. Copy bộ file này vào gốc repo dự án.
2. Điền `SOUL.md` (dự án là gì, quy tắc bất biến) và `STATE.md` (danh sách task).
3. Mở herdr, chạy các agent trong từng pane.
4. Đọc `HERD.md` nếu bạn là người điều phối.
5. Agent đọc `AGENTS.md` đầu tiên — mọi thứ khác chảy ra từ đó.

Xem `examples/expense-tracker/` để thấy bộ file đã điền thật trông như thế nào.

---

## Mô hình vận hành

```
    ┌──────────────────────────────────────────────┐
    │  herdr workspace                             │
    │                                              │
    │  ┌────────────┐   prompt    ┌─────────────┐  │
    │  │Coordinator │ ──────────► │  Worker A   │  │
    │  │            │ ◄────────── │  Worker B   │  │
    │  │  không code│   báo cáo   │  Worker C   │  │
    │  └─────┬──────┘             └─────────────┘  │
    └────────┼─────────────────────────────────────┘
             │ ghi
             ▼
    ┌──────────────────────────────────────────────┐
    │  SOUL · STATE · CONTRACT · DECISIONS ·       │
    │  ISSUE_LOG · GOVERNANCE      (trên đĩa)      │
    └──────────────────────────────────────────────┘
```

- **Coordinator** — người duy nhất ghi file kit; giao việc bằng prompt; không tự viết code.
- **Worker** — chỉ sửa file thuộc mảng của mình; báo cáo lại cho Coordinator.
- **Đĩa** — bộ nhớ thật. Coordinator chết thì worker mới đọc file vẫn tiếp tục được.

---

## Nguyên tắc

1. **Mỗi thông tin nằm ở đúng một nơi.** Không chép lỗi từ `ISSUE_LOG` sang `STATE`.
2. **Một người ghi một file.** Nhiều agent ghi cùng file = ghi đè lẫn nhau.
3. **Không đóng pane đang làm việc.** Context nằm trong tiến trình, không nằm trên đĩa.
4. **Prompt phải tự chứa.** Worker mới không có context của bạn — đừng nói "làm tiếp".
5. **Có người kiểm tra độc lập.** Người viết không tự thấy lỗi của mình.
6. **Chốt giao diện trước khi chạy song song.** Không có `CONTRACT.md` thì hai mảng sẽ vỡ nhau.

---

## Nguồn

Ý tưởng "bộ nhớ chung bằng Markdown" lấy từ
[handsomecat101/agent-team](https://github.com/handsomecat101/agent-team).
Kit này giữ mục đích đó, thay kênh giao việc bằng herdr, và thêm `CONTRACT.md` + `DECISIONS.md`.

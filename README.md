# Herd Team Kit

> Bộ khung Markdown để chạy một **đội agent** trong cùng một workspace terminal
> ([herdr](https://herdr.dev)) — với bộ nhớ ngoài nằm trên đĩa.

**Đây là bộ TEMPLATE, không phải một dự án.** Repo này chỉ chứa file Markdown quy ước.
Không có mã nguồn. Không chứa mã nguồn của herdr. herdr là công cụ bên ngoài, cài riêng.

---

## ⛔ Bạn là agent vừa clone repo này?

**Dừng lại.** Các file trong repo này là **bản mẫu rỗng**, không phải dự án đang chạy.
Đừng boot theo `AGENTS.md` ở đây.

Việc của bạn:

1. Đọc file này để biết kit gồm những gì
2. Copy các file `.md` ở gốc repo vào gốc **dự án thật**
3. Điền `SOUL.md` (dự án là gì, quy tắc bất biến) và `STATE.md` (danh sách task)
4. Từ đó mới bootstrap theo `AGENTS.md`

---

## Trong repo có gì / không có gì

| Có | Không có |
|---|---|
| File `.md` quy ước và template | Mã nguồn của bất kỳ dự án nào |
| Hướng dẫn dùng herdr (`HERD.md`) | Mã nguồn của herdr |
| | Script, công cụ, binary |
| | Danh sách phụ thuộc (`package.json`…) |

Kit này **không chạy được một mình**. Nó là giấy tờ, không phải chương trình.

Điều kiện để dùng: máy đã cài [herdr](https://herdr.dev) và các agent chạy trong pane của nó.
Không có herdr thì `SOUL` / `STATE` / `CONTRACT` / `DECISIONS` / `ISSUE_LOG` vẫn dùng được
(bộ nhớ ngoài vẫn có giá trị); riêng `HERD.md` thì vô dụng.

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
| `AGENTS.md` | Auto-boot — điểm vào duy nhất cho mọi agent | ~70 dòng |
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

## Áp kit vào một dự án — 5 phút

1. Copy **các file `.md` ở gốc repo này** vào gốc dự án thật.
2. Điền `SOUL.md` — dự án là gì, tech stack, **quy tắc bất biến**.
3. Điền `STATE.md` — danh sách task ban đầu.
4. Mở herdr, chạy các agent trong từng pane.
5. Người điều phối đọc `HERD.md`. Agent đọc `AGENTS.md` đầu tiên.

`CONTRACT.md` chưa cần điền ngay — chỉ cần khi sắp cho **từ hai agent trở lên** code song song.

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

Các quy tắc trong `HERD.md` được đúc kết từ việc dùng thật, không phải viết ra trên lý thuyết.

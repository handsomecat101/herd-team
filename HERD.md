# HERD.md — Playbook Điều Phối Bằng herdr

> Dành cho **Coordinator**. Worker không cần đọc file này.
> Mục tiêu: giao việc, phát hiện kẹt, thu kết quả — mà không cần Human ngồi canh.

---

## 1. Mô hình

Một **Coordinator** (không viết code) + N **Worker**, cùng một herdr workspace.

- Coordinator giao việc bằng **prompt trực tiếp**.
- Worker trả kết quả ở **tin nhắn cuối**.
- File trên đĩa là **bộ nhớ**, không phải kênh giao việc.

### Ai biết gì — đọc chung, ghi riêng

Đây là chỗ dễ hiểu nhầm nhất: **chia mảng là chia quyền GHI, không chia quyền BIẾT.**

| | Được GHI | Được ĐỌC |
|---|---|---|
| Bộ file kit | chỉ Coordinator | **mọi agent, toàn bộ** |
| Code | worker theo đường dẫn của mình | mọi agent |

**Không có sổ riêng cho từng agent.** Cả đội đọc chung một bộ file ở một chỗ, để mọi người
nhìn ra cùng một sự thật. Nếu mỗi agent có ghi chú riêng thì không ai còn biết tình trạng chung.

**Giới hạn thật của mô hình này:** worker chỉ thấy ảnh chụp tại lúc nó boot. Trong lúc nó làm,
`STATE.md` có thể đã đổi mà nó không biết — và nó không thấy worker khác đang làm gì.

Bù lại bằng ba cách:

1. **`CONTRACT.md` chốt trước** — worker không cần biết bên kia đang làm gì, chỉ cần biết giao diện.
2. **Prompt tự chứa** — Coordinator chịu trách nhiệm đưa context cần thiết vào prompt.
3. **Coordinator là kênh đồng bộ duy nhất** — có gì đổi thì phải chủ động nói, không chờ ai hỏi.

> **Nghĩa vụ của Coordinator:** thu báo cáo xong thì cập nhật `STATE.md` **ngay trong cùng lượt**.
> Để trễ một lượt là worker kế tiếp boot lên đọc thông tin cũ rồi làm sai — mà thông tin cũ
> nguy hiểm hơn không có thông tin, vì nó trông đáng tin.

---

## 2. Khảo sát đội hình

```bash
test "${HERDR_ENV:-}" = 1      # bắt buộc: phải chạy trong pane herdr

herdr workspace list
herdr pane list --workspace "$HERDR_WORKSPACE_ID"
herdr agent list
```

ID có dạng `w1` (workspace), `w1:t1` (tab), `w1:p1` (pane). Đọc ID từ JSON trả về,
**đừng đoán**.

### Luôn gọi bằng pane ID, không gọi bằng tên

Tên agent chỉ duy nhất trong một server. Có thể có **hai agent cùng tên** ở hai thư mục khác nhau
(ví dụ hai phiên `opencode` cho hai dự án). Gọi bằng tên lúc đó là gọi nhầm người.

> Quy tắc: **mọi lệnh điều khiển dùng pane ID.**

---

## 3. Giao việc

### Cách chuẩn

```bash
herdr agent prompt w1:p7 "$(cat /đường/dẫn/prompt.txt)" --wait --timeout 90000
```

Ghi prompt ra file rồi truyền vào — tránh mọi vấn đề quote và encoding.

- Task ngắn: dùng `--wait` để biết ngay nó bắt đầu chạy hay kẹt.
- Task dài: **bỏ `--wait`**, gửi rồi tự kiểm tra sau. `timeout` không có nghĩa là thất bại —
  task vẫn chạy tiếp trong pane đó.

### Khi herdr không nhận diện được agent

`herdr agent list` có thể báo agent đó là `unknown`. Lúc đó `agent prompt` sẽ trả
`agent_not_found`. Dùng đường vòng:

```bash
herdr pane run w1:pA "<prompt một dòng, không xuống dòng>"
```

`pane run` gõ văn bản rồi Enter. Xuống dòng có thể làm prompt gửi sai — viết thành một dòng dài.

### Prompt phải tự chứa

Worker **không có context của bạn**. Prompt phải nói rõ:

1. Đọc file nào, theo thứ tự nào
2. Task ID + phạm vi file được sửa
3. Ràng buộc cứng (không sửa gì, không thêm gì)
4. Báo cáo theo mẫu nào
5. Việc gì **không** được làm

Tuyệt đối không viết "làm tiếp việc đang dở" — worker mới không biết việc đó là gì.

---

## 4. Chờ và phát hiện kẹt

Năm trạng thái: `idle` · `working` · `blocked` · `done` · `unknown`.

| Trạng thái | Nghĩa |
|---|---|
| `working` | đang chạy |
| `idle` / `done` | xong lượt, sẵn sàng nhận input |
| `blocked` | **có dialog đang chờ ai đó trả lời** |
| `unknown` | có agent nhưng herdr không phân loại được — không đồng nghĩa đã xong |

```bash
herdr agent wait w1:p7 --until blocked --timeout 120000
```

**Khi thấy `blocked`:**
1. Đọc pane để xem dialog hỏi gì: `herdr pane read w1:p7 --source visible --lines 25`
2. Việc thường ngày (duyệt lệnh, duyệt sửa file) → Coordinator có thể tự trả lời
3. Việc **mở rộng quyền** → hỏi Human trước, đừng tự mở

---

## 5. Chế độ quyền — cái bẫy tốn thời gian nhất

`shift+tab` **xoay vòng** qua các chế độ, không phải bật/tắt:

```
default  →  accept-edits  →  plan  →  default  →  ...
```

- `accept-edits`: tự duyệt sửa file → chạy liền mạch
- `plan`: **chỉ đọc, không ghi được file.** Bấm nhầm vào đây là agent đứng hình giữa việc

**Sau mỗi lần bấm, đọc footer để xác nhận chế độ hiện tại.** Đừng bấm hai lần liên tiếp.

Hai điều dễ tưởng nhầm:
- `accept-edits` chỉ áp cho **sửa file**, không áp cho **lệnh shell**. Lệnh vẫn hỏi từng lần.
- Mỗi **tiền tố lệnh khác nhau** là một lần hỏi mới. Cho phép `Get-ChildItem -Path lib`
  không có nghĩa là `Get-ChildItem -Path app` cũng được phép.

> Nếu agent kẹt ở dialog liên tục: để Human bật toàn quyền cho pane đó.
> Coordinator **không tự mở rộng quyền** — đây là ranh giới cứng.

---

## 6. Theo dõi tiến độ

### Đừng dùng `revision` của pane

`herdr pane list` có trường `revision`. **Không dùng nó làm tín hiệu hoàn thành.**
Với agent mà herdr phân loại là `unknown`, trường này không được cập nhật — nó đứng yên
trong khi agent vẫn đang chạy, và bạn sẽ kết luận sai rằng đã xong.

### Dùng tín hiệu nghiệp vụ

| Việc đang chạy | Tín hiệu xong |
|---|---|
| Dev server | Cổng đã tắt / không còn tiến trình con |
| Sinh file | Đủ bộ file đích trên đĩa |
| Agent thường | `idle` / `done` **do herdr phân loại được** |

Kết hợp với `monitor_command` (tự đánh thức khi tiến trình kết thúc) thay vì tự poll.

---

## 7. Chống mất context

- **Không đóng pane của agent đang làm việc.** Đổi pane là mất trắng context của agent đó.
- Pane bị đóng rồi thì agent mới **phải được brief lại từ đầu**, đọc lại bộ kit.
- Theo dõi **phần trăm context** ở footer. Gần đầy thì bảo agent `/compact` trước khi giao việc nặng.
- Bộ kit phải **ngắn** — mỗi phiên mới đều đọc lại. File dài là thuế đánh vào mọi lần khởi động.

---

## 8. Sai lầm đã trả giá

Ghi lại để không lặp. Mỗi dòng là một lần mất thời gian thật.

| Sai lầm | Hậu quả | Cách tránh |
|---|---|---|
| Gọi agent bằng **tên** khi có 2 pane cùng tên | Gửi việc nhầm người | Luôn dùng pane ID |
| Bấm `shift+tab` hai lần liên tiếp | Nhảy sang `plan`, agent không ghi được file | Đọc footer sau mỗi lần bấm |
| Dùng pane `revision` làm tín hiệu xong | Báo "xong" khi agent còn đang chạy | Dùng tín hiệu nghiệp vụ |
| Nói "làm tiếp" với agent ở pane mới | Agent không biết việc gì | Brief lại từ đầu |
| Nhiều agent cùng ghi một file kit | Ghi đè nhau, mất nội dung | Chỉ Coordinator ghi |
| Cho 2 agent `npm install` / build song song | Tranh chấp thư mục phụ thuộc | Tuần tự hoá bước cài đặt |
| Bấm nút tải file bằng `click` thường | Download bị cancel | Dùng lệnh `download` của công cụ browser |
| Tin lời báo cáo "đã xong" | Build thực ra đang đỏ | Bắt buộc có lệnh verify + kết quả |

---

## 9. Nhịp làm việc

```
1. Đọc kit                → biết đang ở đâu
2. Chốt CONTRACT          → nếu sắp cho ≥2 agent code song song
3. Chia mảng theo FILE    → không chồng lấn, không chia theo "chức năng" mơ hồ
4. Giao việc              → prompt tự chứa, ghi rõ phạm vi
5. Treo monitor           → tín hiệu nghiệp vụ
6. Thu báo cáo & verify   → người kiểm tra độc lập
7. Ghi STATE / DECISIONS  → để phiên sau không phải suy lại
```

Bước 2 là bước hay bị bỏ nhất — và là bước đắt nhất khi bỏ.

---

*HERD.md — Đọc khi bạn là người điều phối.*

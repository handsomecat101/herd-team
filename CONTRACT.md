# CONTRACT.md — Hợp Đồng Giao Diện

**🧩 BẢN MẪU** — đây là template rỗng, không phải dự án thật. Điền theo hướng dẫn trong file, rồi xoá dòng này.

> **Chỉ Coordinator ghi file này.**
> Mục đích: cho phép nhiều agent code **song song** mà không vỡ nhau.
> Đổi contract = phải báo Coordinator. Không ai tự đổi.

---

## Khi nào cần file này

Khi sắp cho **từ hai agent trở lên** cùng code. Không có file này thì:

- Agent A đổi tên hàm → agent B gãy ngay, mà hai người không nói chuyện được với nhau
- Hai bên hiểu khác nhau về cùng một kiểu dữ liệu
- Phải chờ nhau từng bước, mất hết lợi ích của việc chạy song song

Có file này thì hai bên code độc lập dựa trên một điểm tựa chung.

---

## Cách viết chữ ký

Ghi đủ để bên dùng **không phải đoán**. Quan trọng nhất là **ngữ nghĩa biên** —
đầu vào sai thì trả về cái gì.

```ts
// Đủ: tên, tham số, kiểu trả về, và ngữ nghĩa khi đầu vào sai
export function parsePort(input: string): number;
// '8080' -> 8080; chuỗi không hợp lệ -> 0
```

```ts
// Thiếu: không biết trường hợp sai thì trả gì, bên dùng sẽ tự đoán
export function parsePort(input: string): number;
```

Với kiểu dữ liệu, ghi rõ **ngữ nghĩa từng trường**, đặc biệt là đơn vị và định dạng:

```ts
export interface [TênKiểu] {
  id: string;         // [khoá — duy nhất hay không, sinh thế nào]
  [trường]: number;   // [đơn vị là gì, có cho phép âm không]
  [trường]: string;   // [định dạng chính xác, múi giờ nếu là ngày]
  [trường]?: string;  // [tuỳ chọn — vắng mặt nghĩa là gì]
}
```

---

## Mẫu

### 1. Kiểu dữ liệu dùng chung

```ts
// [dán định nghĩa type / interface]
```

### 2. Hàm lớp dữ liệu

```ts
// [chữ ký + ngữ nghĩa biên cho từng hàm]
```

### 3. Props / giao diện thành phần

```ts
// [chữ ký props]
```

### 4. Quy ước chung

- **Đường dẫn import:** [`@/...`]
- **Ngôn ngữ giao diện:** [tiếng Việt]
- **Xử lý lỗi:** [hàm nào ném lỗi, hàm nào trả giá trị rỗng — chốt rõ từng hàm]
- **Được dùng:** [danh sách thư viện]

---

## Lịch sử sửa

> Ghi mỗi lần đổi contract. Đây là thứ chống việc tranh luận lại từ đầu.

| Ngày | Đổi gì | Vì sao |
|------|--------|--------|
| [YYYY-MM-DD] | [mô tả thay đổi] | [lý do — thường là hai chỗ trong file mâu thuẫn nhau] |

---

*CONTRACT.md — Điểm gặp duy nhất của các mảng code. Sửa là phải báo.*

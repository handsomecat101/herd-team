# CONTRACT.md — Hợp Đồng Giao Diện

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

## Cách viết

Chốt **trước khi** giao việc. Ghi đủ chữ ký để bên dùng không phải đoán:

```ts
// Đủ: tên, tham số, kiểu trả về, ngữ nghĩa biên
export function parseAmount(input: string): number;
// '1.000.000' -> 1000000; chuỗi sai -> 0
```

```ts
// Thiếu: không biết trường hợp sai thì trả gì
export function parseAmount(input: string): number;
```

Với kiểu dữ liệu, ghi rõ **ngữ nghĩa của từng trường**, đặc biệt là định dạng và đơn vị:

```ts
export interface Transaction {
  id: string;        // UUID, duy nhất
  amount: number;    // số nguyên, đơn vị nhỏ nhất, >= 0
  date: string;      // 'YYYY-MM-DD' giờ LOCAL, không phải UTC
  note?: string;     // tuỳ chọn, tối đa 80 ký tự
}
```

---

## Mẫu

### 1. Kiểu dữ liệu dùng chung

```ts
// [dán định nghĩa type/interface]
```

### 2. Hàm lớp dữ liệu

```ts
// [chữ ký + ngữ nghĩa biên]
```

### 3. Props / giao diện thành phần

```ts
// [chữ ký props]
```

### 4. Quy ước chung

- Đường dẫn import: [`@/lib/...`]
- Ngôn ngữ giao diện: [tiếng Việt]
- Xử lý lỗi: [ném lỗi / trả giá trị rỗng — chốt rõ từng hàm]
- Thư viện được dùng: [danh sách trắng]

---

## Lịch sử sửa

> Ghi mỗi lần đổi contract. Đây là thứ chống việc tranh luận lại từ đầu.

| Ngày | Đổi gì | Vì sao |
|------|--------|--------|
| [YYYY-MM-DD] | [ví dụ: `amount` từ `> 0` thành `>= 0`] | [hai chỗ trong file đang mâu thuẫn nhau] |

---

*CONTRACT.md — Điểm gặp duy nhất của các mảng code. Sửa là phải báo.*

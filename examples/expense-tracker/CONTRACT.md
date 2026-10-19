# CONTRACT.md — Hợp Đồng Giao Diện (Interface Freeze)

> **Do Coordinator quản lý. KHÔNG agent nào được sửa file này.**
> Mục đích: cho phép 2 track code **song song** mà không dẫm chân nhau.
> - Track B (`lib/**`) implement đúng chữ ký dưới đây.
> - Track A (`components/**`, `app/**`) import và dùng đúng chữ ký dưới đây.
> Đổi contract = phải báo Coordinator.
>
> **Lịch sử sửa (Coordinator):**
> - 2026-10-19: `amount` chốt là `≥ 0` (trước đây §1 ghi `> 0`, mâu thuẫn với validate §4 — nay thống nhất theo §4).
> - 2026-10-19: thêm luật chống hydration mismatch cho tiền tệ (§7) — phát hiện bởi review của Track C.

---

## 1. `lib/types.ts` (Track B)

```ts
export type TxType = 'income' | 'expense';
export type TypeFilter = 'all' | TxType;

export interface Transaction {
  id: string;          // createId()
  type: TxType;
  amount: number;      // số nguyên VND, ≥ 0
  category: string;
  date: string;        // 'YYYY-MM-DD' giờ LOCAL
  note?: string;
  createdAt: string;   // ISO 8601
}

export interface Filters {
  month: string;       // 'YYYY-MM' hoặc '' = tất cả thời gian
  type: TypeFilter;
  search: string;      // khớp note hoặc category, không phân biệt hoa thường
}

export interface Summary {
  income: number;
  expense: number;
  balance: number;     // income - expense
}

export interface MonthBucket {
  key: string;         // 'YYYY-MM'
  label: string;       // 'T10'
  income: number;
  expense: number;
}

export interface CategoryBucket {
  category: string;
  amount: number;
  percent: number;     // 0..100, làm tròn 1 chữ số
}
```

---

## 2. `lib/categories.ts` (Track B)

```ts
export const CATEGORIES: Record<TxType, readonly string[]>;
// income:  ['Lương', 'Thưởng', 'Đầu tư', 'Thu khác']
// expense: ['Ăn uống', 'Đi lại', 'Nhà ở', 'Mua sắm', 'Giải trí',
//           'Sức khỏe', 'Giáo dục', 'Hóa đơn', 'Chi khác']

export function categoriesFor(type: TxType): readonly string[];
export function defaultCategory(type: TxType): string; // phần tử đầu
```

---

## 3. `lib/format.ts` (Track B)

```ts
export function formatVND(n: number): string;       // 1000000 -> "1.000.000 ₫"
export function formatVNDShort(n: number): string;  // 1500000 -> "1,5 tr"; 0 -> "0"
export function formatDate(date: string): string;   // '2026-10-19' -> "19/10/2026"
export function parseAmount(input: string): number; // '1.000.000' -> 1000000; sai -> 0
export function monthKey(date: string): string;     // '2026-10-19' -> '2026-10'
export function monthLabel(key: string): string;    // '2026-10' -> 'T10'; sai định dạng -> ''
export function todayISO(): string;                 // ngày local 'YYYY-MM-DD'
```

---

## 4. `lib/storage.ts` (Track B)

```ts
export const STORAGE_KEY = 'expense-tracker:v1';

export function createId(): string;                        // randomUUID() + fallback
export function loadTransactions(): Transaction[];          // [] nếu chưa có / JSON hỏng
export function saveTransactions(txs: Transaction[]): void; // bọc try/catch, không throw
export function exportJSON(txs: Transaction[]): void;       // tải file expense-tracker-YYYY-MM-DD.json
export function parseImported(text: string): Transaction[]; // throw Error nếu sai định dạng
```

`parseImported` chấp nhận **cả 2 dạng**: `Transaction[]` hoặc `{ transactions: Transaction[] }`.
Phải validate từng item: `type` ∈ {income, expense}, `amount` là số hữu hạn ≥ 0, `date` khớp `^\d{4}-\d{2}-\d{2}$`. Item sai → bỏ qua, không throw.

---

## 5. `lib/stats.ts` (Track B)

```ts
export function filterTransactions(txs: Transaction[], f: Filters): Transaction[];
// month '' => bỏ qua lọc tháng; type 'all' => bỏ qua lọc loại; search '' => bỏ qua
export function summarize(txs: Transaction[]): Summary;
export function sortByDateDesc(txs: Transaction[]): Transaction[];
export function lastMonthsBuckets(txs: Transaction[], count?: number): MonthBucket[];
// count mặc định 6. Luôn trả đủ `count` bucket, tính cả tháng không có giao dịch,
// tháng cuối = tháng hiện tại.
export function categoryBuckets(txs: Transaction[], type?: TxType): CategoryBucket[];
// type mặc định 'expense'. Chỉ tính giao dịch đúng loại đó.
// Sắp xếp amount giảm dần. percent = amount / tổng * 100.
```

---

## 6. Component props (Track A)

```ts
// components/SummaryCards.tsx  — nhận thẳng Summary
export function SummaryCards(props: Summary): JSX.Element;

// components/TransactionForm.tsx
export function TransactionForm(props: {
  editing: Transaction | null;
  onSubmit(input: Omit<Transaction, 'id' | 'createdAt'>): void;
  onCancelEdit(): void;
}): JSX.Element;

// components/TransactionList.tsx
export function TransactionList(props: {
  items: Transaction[];
  onEdit(tx: Transaction): void;
  onDelete(id: string): void;
}): JSX.Element;

// components/Filters.tsx  — không cần truyền danh sách, chỉ báo lên
export function Filters(props: {
  value: Filters;
  onChange(next: Filters): void;
  count: number;      // số giao dịch khớp, để hiển thị "12 giao dịch"
}): JSX.Element;

// components/BarChart.tsx
export function BarChart(props: { data: MonthBucket[] }): JSX.Element;

// components/CategoryBreakdown.tsx
export function CategoryBreakdown(props: {
  data: CategoryBucket[];
  scopeLabel: string;  // vd "Tháng 10/2026" hoặc "Tất cả thời gian"
}): JSX.Element;

// components/ExpenseApp.tsx  — 'use client', default export
export default function ExpenseApp(): JSX.Element;
```

`app/page.tsx` là server component, chỉ `import ExpenseApp from '@/components/ExpenseApp'` và render.
`app/layout.tsx` đặt `<html lang="vi">`, metadata title `Quản lý chi tiêu`.

---

## 7. Quy Ước Chung

- Alias: `@/lib/...`, `@/components/...`.
- Ngôn ngữ UI: **tiếng Việt**.
- Không thêm dependency ngoài. Chỉ `next`, `react`, `react-dom`, `typescript`, `tailwindcss` (+ eslint).
- Toàn bộ component dùng hook/handler phải có `'use client'` ở dòng đầu.
- Tiền hiển thị qua `formatVND`, không tự nối chuỗi `₫`.
- **Chống hydration mismatch cho tiền tệ (BẮT BUỘC):** `Intl.NumberFormat('vi-VN')` có thể cho ký tự trắng khác nhau giữa Node và Chrome (U+00A0 vs U+202F) trước `₫`. `ExpenseApp` phải giữ cờ `hydrated` (mặc định `false`), **chỉ render số liệu sau `useEffect`**. Trong lần render đầu, hiển thị `—` hoặc skeleton. Không gọi `lastMonthsBuckets` / `categoryBuckets` / `new Date()` trong thân render.
- **Dữ liệu chỉ đọc từ `localStorage` trong `useEffect`** — không đọc trong initializer của `useState`.

# DECISIONS.md — Nhật Ký Quyết Định

**🧩 BẢN MẪU** — đây là template rỗng, không phải dự án thật. Điền theo hướng dẫn trong file, rồi xoá dòng này.

> **Chỉ Coordinator ghi file này.**
> Ghi **quyết gì** và **vì sao** — không ghi quyết định hiển nhiên.
> Luật: 1–3 dòng / quyết định. Mới nhất ở trên.

---

## Vì sao cần file này

`ISSUE_LOG.md` ghi **lỗi**. `CONTRACT.md` ghi **giao diện**.

Không file nào ghi **lý do**. Ba tháng sau sẽ có người hỏi "sao lại chọn cách này?" —
và không ai trả lời được, vì lúc quyết định thì lý do chỉ nằm trong context của một agent,
mà context thì mất.

Mỗi lần phải suy lại một quyết định cũ là một lần trả giá lần hai cho cùng một việc.

---

## Khi nào ghi

Ghi khi quyết định đó **sẽ bị hỏi lại**:

- Chọn công nghệ / thư viện / cách lưu trữ
- Từ chối một phương án mà người khác có thể muốn
- Chấp nhận một đánh đổi có hại đã biết
- Đặt ra một quy ước không hiển nhiên

Không ghi: chọn tên biến, chia thư mục theo lẽ thường.

---

## Mẫu

```markdown
### [YYYY-MM-DD] — [quyết định trong 1 dòng]

- **Chọn:** [làm gì]
- **Vì:** [lý do thật — ràng buộc, chi phí, hay thời gian]
- **Đánh đổi:** [mất gì; khi nào nên xem lại]
```

Bốn dòng. Nếu dài hơn thì chưa hiểu rõ quyết định.

Điểm đáng viết nhất ở dòng **Đánh đổi**: nó nói cho người sau biết **khi nào được phép
đổi ý**. Một quyết định không ghi đánh đổi thì sẽ bị tranh luận lại vô ích.

---

## Quyết định

### [YYYY-MM-DD] — [quyết định đầu tiên]

- **Chọn:** [làm gì]
- **Vì:** [lý do]
- **Đánh đổi:** [mất gì; xem lại khi nào]

---

*DECISIONS.md — Rẻ nhất để viết, đắt nhất khi thiếu.*

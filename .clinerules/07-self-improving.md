---
description: Hướng dẫn Cline tự cải thiện qua từng phiên bằng cách ghi lại patterns, gotchas và insights vào .clinerules và memory-bank.
author: TaiVo-AI
version: 1.0
tags: ["self-improving", "learning", "meta", "workflow"]
---

# Self-Improving Cline

Cuối mỗi task quan trọng, Cline chủ động hỏi:

> "Bạn có muốn tôi cập nhật `.clinerules` hoặc `memory-bank/` dựa trên những gì chúng ta vừa học được không?"

## Những gì nên ghi lại

### Vào `.clinerules/` (patterns lâu dài)
- Pattern code mới phát hiện hiệu quả hơn cách cũ
- Lỗi hay gặp và cách tránh
- Tool/command hữu ích không có trong docs
- Convention đặc thù của project này

### Vào `memory-bank/activeContext.md` (insights phiên hiện tại)
- Quyết định kỹ thuật vừa được đưa ra
- Context quan trọng cho phiên tiếp theo
- Vấn đề chưa giải quyết

### Vào `memory-bank/systemPatterns.md` (kiến trúc mới)
- Pattern mới được adopt
- Anti-pattern vừa được refactor ra

## Quy trình học

```
Phát hiện insight mới
  → Xác nhận với user: "Tôi thấy [pattern X]. Tôi có nên ghi lại không?"
  → Nếu user đồng ý → cập nhật file phù hợp
  → Giải thích ngắn gọn lý do ghi lại
```

## Ví dụ insight đáng ghi lại

```markdown
## Tool Usage Patterns (thêm vào techContext.md)

- **Prisma migrations**: Luôn chạy `npx prisma generate` sau khi sửa schema
  trước `npx prisma migrate dev` — nếu không type sẽ không sync
- **Vitest watch mode**: Dùng `--reporter=verbose` khi debug test phức tạp
```

## Không nên ghi lại

- Thông tin một lần, không tái sử dụng
- Quyết định đã được ghi trong git commit message
- Context quá cụ thể cho một task không lặp lại

---
description: Quy tắc tài liệu: README, JSDoc, ADR. Khi nào và cách viết tài liệu hiệu quả.
author: TaiVo-AI
version: 1.0
tags: ["documentation", "readme", "jsdoc"]
---

# Documentation Standards

Tài liệu là một phần của code — không phải afterthought.

## Khi nào phải viết tài liệu

| Tình huống | Loại tài liệu |
|-----------|--------------|
| Tạo public API / exported function | JSDoc inline |
| Tạo component mới | Props table + usage example |
| Quyết định kiến trúc quan trọng | ADR (Architecture Decision Record) |
| Setup môi trường thay đổi | Cập nhật README hoặc `memory-bank/techContext.md` |
| Bug fix phức tạp | Comment giải thích root cause |

## README.md — Bắt buộc có

Mỗi project phải có README với các mục:

```markdown
# Project Name
> Mô tả 1 câu

## Quick Start
## Development Setup
## Environment Variables
## Project Structure
## Scripts
## Contributing
```

## JSDoc — API công khai

```typescript
/**
 * Tính tổng tiền đơn hàng sau khi áp dụng discount.
 *
 * @param items - Danh sách sản phẩm trong giỏ hàng
 * @param discountCode - Mã giảm giá (tùy chọn)
 * @returns Tổng tiền sau giảm giá (VND)
 * @throws {InvalidDiscountError} Nếu mã giảm giá không hợp lệ
 *
 * @example
 * const total = calculateTotal(cartItems, 'SUMMER2024');
 */
export function calculateTotal(items: CartItem[], discountCode?: string): number
```

## ADR (Architecture Decision Record)

Lưu tại `docs/decisions/` với format:

```markdown
# ADR-001: [Tên quyết định]

## Status: [Proposed | Accepted | Deprecated]
## Date: YYYY-MM-DD

## Context
[Tại sao cần đưa ra quyết định này?]

## Decision
[Quyết định là gì?]

## Consequences
[Hệ quả tích cực và tiêu cực]
```

## Cập nhật Memory Bank

Sau mỗi thay đổi lớn, cập nhật:
- `memory-bank/activeContext.md` — những gì vừa thay đổi
- `memory-bank/progress.md` — cập nhật trạng thái & changelog
- `memory-bank/systemPatterns.md` — nếu thêm pattern/architecture mới

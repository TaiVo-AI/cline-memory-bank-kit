# System Patterns

> Kiến trúc, quyết định kỹ thuật, patterns đang dùng, và các đường đi implement quan trọng.

## Kiến Trúc Tổng Quan

[Mô tả: monolith / microservices / serverless / v.v.]

```
[Sơ đồ hoặc mô tả luồng data chính]
Client → API Gateway → Services → Database
```

## Các Layer & Trách Nhiệm

| Layer | Thư mục | Trách nhiệm |
|-------|---------|------------|
| API/Routes | `src/api/` | Nhận request, validate, trả response |
| Services | `src/services/` | Business logic |
| Repository | `src/repositories/` | Database access |
| Models | `src/models/` | Data structures |

## Design Patterns Đang Dùng

| Pattern | Vị trí | Lý do |
|---------|--------|-------|
| Repository | DB layer | Tách business logic khỏi data access |
| ... | ... | ... |

## Critical Implementation Paths

> Những đường đi quan trọng nhất trong code — phải hiểu rõ trước khi thay đổi.

- **[Tên flow]**: `EntryPoint → Service → Repository → DB` — [Mô tả tại sao quan trọng]
- **[Auth flow]**: `Middleware → JWT verify → UserContext → Handler`
- **[Tên flow khác]**: ...

## Quyết Định Kỹ Thuật Quan Trọng

| Quyết định | Lý do | Thay thế đã xem xét |
|-----------|-------|---------------------|
| Dùng PostgreSQL | ACID, JSON support | MongoDB — không đủ consistency |
| ... | ... | ... |

## Conventions Đặc Thù Dự Án

[Những convention không rõ ràng từ code, cần ghi lại rõ]

## Anti-Patterns Cần Tránh

[Những gì KHÔNG làm trong dự án này và lý do cụ thể]

---
*Cập nhật: YYYY-MM-DD*

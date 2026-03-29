---
description: Chiến lược testing: unit, integration, coverage targets, mock rules.
author: TaiVo-AI
version: 1.0
tags: ["testing", "tdd", "quality"]
---

# Testing Standards

Test là bảo hiểm cho code — không phải gánh nặng.

## Chiến lược Testing (Testing Pyramid)

```
        /\
       /E2E\        ← Ít nhất, chậm nhất, tốn nhất
      /------\
     /Integra-\     ← Vừa phải
    /  tion    \
   /------------\
  /  Unit Tests  \  ← Nhiều nhất, nhanh nhất, rẻ nhất
 /______________  \
```

## Quy tắc viết test

### Unit Tests — Business Logic

```typescript
// ✅ Đúng — test behavior, không test implementation
describe('calculateDiscount', () => {
  it('applies 10% for SUMMER code', () => {
    expect(calculateDiscount(100_000, 'SUMMER')).toBe(90_000);
  });

  it('returns original price for invalid code', () => {
    expect(calculateDiscount(100_000, 'INVALID')).toBe(100_000);
  });

  it('throws for negative price', () => {
    expect(() => calculateDiscount(-1, 'SUMMER')).toThrow(InvalidPriceError);
  });
});
```

### Integration Tests — Route Handlers & Repositories

```typescript
// Test route handlers với database thật hoặc in-memory DB
describe('POST /api/orders', () => {
  it('creates order and returns 201', async () => {
    const response = await request(app)
      .post('/api/orders')
      .send({ items: [{ productId: '1', qty: 2 }] })
      .set('Authorization', `Bearer ${testToken}`);

    expect(response.status).toBe(201);
    expect(response.body).toMatchObject({ id: expect.any(String) });
  });
});
```

## Quy tắc Mock

- **Mock external services** (APIs bên thứ 3, email, SMS)
- **Không mock internal modules** — test chúng trực tiếp
- **Mock database** chỉ trong unit test, dùng DB thật trong integration test

## Coverage

| Loại | Mục tiêu tối thiểu |
|------|-------------------|
| Business logic (services) | 80%+ |
| API route handlers | 70%+ |
| Utility functions | 90%+ |
| UI Components | 60%+ |

> Coverage cao không đồng nghĩa test tốt. Ưu tiên test meaningful cases.

## Naming Convention cho test

```
describe('[Tên module/function]', () => {
  it('[should + hành vi mong đợi]', ...)
  it('[returns/throws + kết quả] when [điều kiện]', ...)
})
```

## Không được làm

- Không skip test mà không có lý do (`it.skip`, `xit`)
- Không test implementation details (private methods, internal state)
- Không để test phụ thuộc vào thứ tự chạy
- Không hardcode date/time — dùng mock

---
description: Tiêu chuẩn viết code: naming conventions, async, imports, error handling.
author: TaiVo-AI
version: 1.0
tags: ["coding-standards", "typescript", "quality"]
---

# Coding Standards

Tiêu chuẩn viết code áp dụng xuyên suốt toàn dự án.

## Nguyên tắc chung

- Ưu tiên **rõ ràng** hơn clever — code phải tự giải thích được
- Mỗi function/module làm **một việc** duy nhất (Single Responsibility)
- Không lặp code — trừu tượng hóa khi logic xuất hiện lần thứ 3 (Rule of Three)
- Xử lý lỗi rõ ràng — không bao giờ nuốt exception mà không log

## Đặt tên (Naming Conventions)

| Loại | Convention | Ví dụ |
|------|-----------|-------|
| Variables, functions | camelCase | `getUserData`, `isLoading` |
| Classes, Types | PascalCase | `UserService`, `ApiResponse` |
| Constants | UPPER_SNAKE_CASE | `MAX_RETRY`, `API_BASE_URL` |
| Files | kebab-case | `user-service.ts`, `api-client.ts` |
| Test files | `*.test.ts` hoặc `*.spec.ts` | `user.service.test.ts` |

## Async / Error Handling

```typescript
// ✅ Đúng — async/await với typed error
async function fetchUser(id: string): Promise<User> {
  try {
    const data = await userRepository.findById(id);
    if (!data) throw new NotFoundError(`User ${id} not found`);
    return data;
  } catch (error) {
    logger.error('fetchUser failed', { id, error });
    throw error;
  }
}

// ❌ Sai — callback style, nuốt lỗi
function fetchUser(id, callback) {
  userRepository.findById(id, (err, data) => {
    if (err) return; // nuốt lỗi!
    callback(data);
  });
}
```

## Import & Module

- Sắp xếp import theo thứ tự: external libs → internal modules → types
- Sử dụng absolute imports, không dùng `../../../`
- Không import wildcard (`import * as`)

## Comments & Documentation

- Comment giải thích **tại sao**, không phải **cái gì**
- JSDoc cho public API và exported functions
- TODO phải kèm ticket/issue number: `// TODO(#123): Fix race condition`
- Xóa code đã comment out trước khi merge

## Không được làm

- Không commit code có `console.log` debug
- Không hardcode secrets, URLs, magic numbers vào source
- Không bỏ qua TypeScript errors bằng `// @ts-ignore` (trừ có lý do ghi rõ)
- Không viết function dài hơn 50 dòng mà không refactor

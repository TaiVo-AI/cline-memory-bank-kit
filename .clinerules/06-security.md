---
description: Bảo mật: secrets, input validation, auth, security headers.
author: TaiVo-AI
version: 1.0
tags: ["security", "owasp", "auth"]
---

# Security Guidelines

Bảo mật là yêu cầu phi chức năng — không phải tùy chọn.

## Secrets & Credentials

```bash
# ✅ Đúng — dùng biến môi trường
const dbUrl = process.env.DATABASE_URL;

# ❌ Sai — hardcode credential
const dbUrl = 'postgresql://admin:password123@localhost/mydb';
```

**Quy tắc:**
- Toàn bộ secrets trong `.env` (không commit lên git)
- Cung cấp `.env.example` với placeholder values
- Rotate secrets ngay nếu bị lộ

## Input Validation

- Validate **tất cả** input từ người dùng ở server side
- Dùng schema validation (Zod, Joi, Yup) — không validate thủ công
- Sanitize input trước khi render HTML (chống XSS)
- Parameterized queries — không concatenate SQL string (chống SQL Injection)

```typescript
// ✅ Đúng — Zod schema validation
const createUserSchema = z.object({
  email: z.string().email(),
  password: z.string().min(8).max(100),
  role: z.enum(['user', 'admin']),
});

// ✅ Đúng — parameterized query
const user = await db.query(
  'SELECT * FROM users WHERE email = $1',
  [email] // KHÔNG dùng template string với input
);
```

## Authentication & Authorization

- Không tự implement crypto — dùng thư viện đã được audit (bcrypt, argon2)
- JWT: dùng `RS256` hoặc `ES256`, không dùng `none` algorithm
- Kiểm tra authorization trước khi trả dữ liệu:
  ```typescript
  if (resource.ownerId !== currentUser.id) {
    throw new ForbiddenError();
  }
  ```
- Implement rate limiting cho auth endpoints

## Dependencies

- Chạy `npm audit` định kỳ
- Không dùng package với 0 downloads hoặc không maintainer
- Pin versions trong production (`"react": "18.2.0"`, không phải `"^18"`)

## API Security Headers

```typescript
// Dùng helmet.js hoặc cấu hình tương đương
app.use(helmet({
  contentSecurityPolicy: true,
  hsts: { maxAge: 31536000 },
  noSniff: true,
}));
```

## Không được làm

- Không log sensitive data (password, token, card number)
- Không expose stack trace trong production response
- Không dùng `eval()`, `new Function()`, `innerHTML` với user input
- Không disable CORS cho tất cả origins trong production (`*`)

# Tech Context

> Stack, môi trường dev, dependencies, constraints kỹ thuật, và cách dùng tool hiệu quả.

## Tech Stack

| Layer | Công nghệ | Phiên bản |
|-------|-----------|-----------|
| Language | | |
| Framework | | |
| Database | | |
| Cache | | |
| Auth | | |
| Testing | | |
| CI/CD | | |
| Hosting | | |

## Môi Trường Dev — Setup

```bash
# Prerequisites
node >= 20.x
pnpm >= 9.x

# Clone & install
git clone <repo>
cd <project>
pnpm install

# Env setup
cp .env.example .env
# Điền các giá trị cần thiết vào .env

# Run dev
pnpm dev
```

## Biến Môi Trường Quan Trọng

| Biến | Bắt buộc | Mô tả |
|------|---------|-------|
| `DATABASE_URL` | ✅ | PostgreSQL connection string |
| `JWT_SECRET` | ✅ | Secret key cho JWT |
| `API_KEY_XXX` | ✅ | API key service X |

## Scripts

```bash
pnpm dev        # Dev server
pnpm build      # Production build
pnpm test       # Run tests
pnpm test:e2e   # E2E tests
pnpm lint       # Lint
pnpm db:migrate # Run migrations
```

## External Integrations

| Service | Mục đích | Docs |
|---------|---------|------|
| ... | ... | ... |

## Tool Usage Patterns

> Cách dùng tool/CLI hiệu quả đã được phát hiện trong quá trình làm việc.

- **[Tên tool]**: Dùng flag `--xxx` khi cần [tình huống], tránh `--yyy` vì [lý do]
- **Database migrations**: Luôn chạy `pnpm db:generate` trước `pnpm db:migrate`
- **Testing**: Dùng `pnpm test:watch` khi develop, `pnpm test` cho CI
- [Thêm patterns khi phát hiện...]

## Constraints Kỹ Thuật

- [Constraint 1 — ví dụ: phải chạy được trên Node 20 LTS]
- [Constraint 2 — ví dụ: bundle size < 200KB]

---
*Cập nhật: YYYY-MM-DD*

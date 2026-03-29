---
description: Git workflow: branch strategy, conventional commits, PR checklist.
author: TaiVo-AI
version: 1.0
tags: ["git", "workflow", "commits"]
---

# Git Workflow

Quy trình làm việc với Git đảm bảo lịch sử commit rõ ràng và hợp tác nhất quán.

## Branch Strategy

```
main          ← Production-ready, được bảo vệ
develop       ← Integration branch
feature/*     ← Tính năng mới:    feature/user-authentication
fix/*         ← Bug fix:          fix/login-redirect-loop
chore/*       ← Maintenance:      chore/update-dependencies
docs/*        ← Chỉ tài liệu:    docs/api-reference
```

**Quy tắc:**
- Không commit thẳng vào `main` hoặc `develop`
- Mỗi branch giải quyết 1 vấn đề/tính năng duy nhất
- Xóa branch sau khi merge

## Commit Message — Conventional Commits

Format: `<type>(<scope>): <mô tả ngắn>`

| Type | Dùng khi |
|------|---------|
| `feat` | Thêm tính năng mới |
| `fix` | Sửa bug |
| `docs` | Chỉ thay đổi tài liệu |
| `style` | Format, whitespace (không đổi logic) |
| `refactor` | Refactor không thêm feature/fix bug |
| `test` | Thêm hoặc sửa test |
| `chore` | Build, dependencies, config |
| `perf` | Cải thiện performance |

```bash
# ✅ Đúng
feat(auth): add Google OAuth login
fix(cart): resolve quantity overflow on checkout
docs(api): update endpoint documentation for v2
chore(deps): upgrade to React 19

# ❌ Sai
fix bug
update code
WIP
```

## Pull Request

**Checklist trước khi tạo PR:**
- [ ] Code tự review lại lần cuối
- [ ] Tests đã pass (`npm test`)
- [ ] Không còn `console.log` debug
- [ ] PR description mô tả rõ **what** và **why**
- [ ] Link đến issue liên quan (`Closes #123`)

**PR Description template:**
```markdown
## Thay đổi gì?
[Mô tả ngắn gọn]

## Tại sao?
[Context / vấn đề giải quyết]

## Cách test?
[Bước reproduce hoặc verify]

Closes #[issue number]
```

## Không được commit

File `.gitignore` phải có:
- `node_modules/`, `.env`, `*.local`
- Build artifacts: `dist/`, `build/`, `.next/`
- IDE files: `.vscode/settings.json`, `.idea/`
- OS files: `.DS_Store`, `Thumbs.db`
- Secrets và credentials mọi loại

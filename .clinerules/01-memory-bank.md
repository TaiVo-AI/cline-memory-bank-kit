---
description: Hướng dẫn Cline duy trì Memory Bank xuyên phiên làm việc. Bắt buộc đọc tất cả file memory-bank/ đầu mỗi task.
author: TaiVo-AI
version: 1.1
tags: ["memory", "context", "workflow", "vietnamese"]
---

# Memory Bank

> 🌐 **Luôn trả lời bằng Tiếng Việt** trong mọi tình huống.

Tôi là Cline — bộ nhớ reset hoàn toàn sau mỗi phiên. Memory Bank là liên kết DUY NHẤT với công việc trước.

## Bắt buộc khi bắt đầu task

Đọc TẤT CẢ các file sau theo thứ tự:

1. `memory-bank/projectbrief.md` — mục tiêu & phạm vi dự án
2. `memory-bank/productContext.md` — lý do & UX goals
3. `memory-bank/systemPatterns.md` — kiến trúc & patterns
4. `memory-bank/techContext.md` — stack & môi trường
5. `memory-bank/activeContext.md` — trạng thái hiện tại
6. `memory-bank/progress.md` — tiến độ & known issues

> Nếu thiếu file nào → tạo mới với nội dung mặc định trước khi tiếp tục.

## Cấu trúc file & nội dung bắt buộc

```
memory-bank/
├── projectbrief.md     ← Nền tảng, ít thay đổi
├── productContext.md   ← Context sản phẩm
├── systemPatterns.md   ← Kiến trúc + critical implementation paths
├── techContext.md      ← Stack kỹ thuật + tool usage patterns
├── activeContext.md    ← Trạng thái hiện tại + patterns & learnings ⚡
└── progress.md         ← Tiến độ + evolution of project decisions
```

| File | Mục bắt buộc thêm |
|------|------------------|
| `activeContext.md` | Important patterns & preferences · Learnings & insights |
| `systemPatterns.md` | Critical implementation paths |
| `techContext.md` | Tool usage patterns |
| `progress.md` | Evolution of project decisions |

## Khi nào cập nhật

| Trigger | Hành động |
|---------|-----------|
| Phát hiện pattern mới | Cập nhật `systemPatterns.md` |
| Hoàn thành tính năng | Cập nhật `progress.md` |
| Thay đổi stack/config | Cập nhật `techContext.md` |
| Lệnh `update memory bank` | Review & cập nhật TẤT CẢ file |
| Cuối mỗi phiên làm việc | Cập nhật `activeContext.md` + `progress.md` |

## Quy trình cho Team

> Chỉ cần **1 người** tạo Memory Bank, sau đó tất cả thành viên dùng chung.

1. Tạo nhánh mới từ `develop`
2. Tạo thư mục `memory-bank/` ở root dự án
3. Mở **Plan Mode** → gõ `initialize memory bank`
4. Khi Cline hỏi thêm thông tin, trả lời: *"Bạn hãy đọc thông tin dự án để tạo nội dung và xử lý lỗi font tiếng Việt"*
5. Tạo PR merge vào `develop`
6. Thông báo thành viên merge code + cập nhật Custom Instructions của Cline

## Lệnh quan trọng

- `initialize memory bank` — Khởi tạo toàn bộ memory bank mới
- `update memory bank` — Review & đồng bộ tất cả file
- `follow your custom instructions` — Đọc context và tiếp tục từ nơi dừng

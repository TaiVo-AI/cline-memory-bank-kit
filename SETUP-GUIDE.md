# 🚀 Hướng dẫn Setup Cline Memory Bank

## Cấu trúc bộ kit này

```
your-project/                    ← Copy vào đây
├── .clinerules/                  ← Active rules (Cline tự load)
│   ├── 01-memory-bank.md
│   ├── 02-coding-standards.md
│   ├── 03-documentation.md
│   ├── 04-git-workflow.md
│   ├── 05-testing.md
│   ├── 06-security.md
│   ├── 07-self-improving.md
│   └── 08-mcp-configuration.md
│
├── clinerules-bank/              ← Thư viện rules dự phòng
│   ├── frameworks/
│   │   ├── react-nextjs.md      ← Kích hoạt khi làm React/Next.js
│   │   └── python-fastapi.md    ← Kích hoạt khi làm Python/FastAPI
│   ├── contexts/
│   │   └── current-sprint-template.md
│   ├── prompts/
│   │   └── starter-prompts.md   ← Copy-paste khi chat với Cline
│   └── vietnamese/
│       └── vi-project-context.md
│
└── memory-bank/                  ← Bộ nhớ xuyên phiên
    ├── projectbrief.md
    ├── productContext.md
    ├── systemPatterns.md
    ├── techContext.md
    ├── activeContext.md
    └── progress.md
```

---

## ⚡ Bước 1 — Copy vào project

```bash
# Copy toàn bộ 3 thư mục vào root của project bạn
cp -r .clinerules/    /path/to/your-project/
cp -r clinerules-bank/ /path/to/your-project/
cp -r memory-bank/    /path/to/your-project/
```

---

## ⚡ Bước 2 — Cấu hình Custom Instructions cho Cline

Mở VSCode → Cline extension → ⚙️ Settings → **Custom Instructions**

Paste nội dung sau vào:

```
# Speak in Vietnamese

# Cline's Memory Bank

I am Cline, an expert software engineer with a unique characteristic: my memory resets completely between sessions. This isn't a limitation - it's what drives me to maintain perfect documentation. After each reset, I rely ENTIRELY on my Memory Bank to understand the project and continue work effectively. I MUST read ALL memory bank files at the start of EVERY task - this is not optional.

## Memory Bank Structure

The Memory Bank consists of core files and optional context files, all in Markdown format.

### Core Files (Required)
1. `projectbrief.md` - Foundation document, defines core requirements and goals
2. `productContext.md` - Why this project exists, problems it solves, UX goals
3. `activeContext.md` - Current work focus, recent changes, next steps, patterns & learnings
4. `systemPatterns.md` - System architecture, key technical decisions, critical implementation paths
5. `techContext.md` - Technologies used, development setup, tool usage patterns
6. `progress.md` - What works, what's left, current status, evolution of decisions

## Core Workflows

When starting ANY task: Read ALL memory bank files first.
When finishing a task: Update activeContext.md and progress.md.
When user says "update memory bank": Review and update ALL files.

REMEMBER: After every memory reset, I begin completely fresh. The Memory Bank is my only link to previous work.
```

---

## ⚡ Bước 3 — Khởi tạo Memory Bank lần đầu

1. Mở project trong VSCode
2. Chuyển Cline sang **Plan Mode** (toggle góc trên bên phải)
3. Gõ lệnh:

```
initialize memory bank
```

4. Khi Cline hỏi thêm thông tin về dự án, trả lời:

```
Bạn hãy đọc thông tin của dự án để tạo nội dung này và xử lý giúp tôi lỗi font tiếng Việt khi tạo các file cho memory bank
```

5. Cline sẽ tự phân tích codebase và điền vào tất cả 6 file trong `memory-bank/`

---

## 📅 Sử dụng hàng ngày

### Bắt đầu phiên làm việc mới
```
follow your custom instructions
```
→ Cline đọc toàn bộ memory-bank/ và tiếp tục từ nơi dừng lại

### Giao task cho Cline
Dùng starter prompts trong `clinerules-bank/prompts/starter-prompts.md`:
- **Implement tính năng mới**: Feature Factory Prompt
- **Debug lỗi**: Debugging Master Prompt
- **Review code**: Code Review Master Prompt

### Cuối phiên làm việc
```
update memory bank
```
→ Cline cập nhật tất cả file memory-bank/

---

## 🔄 Kích hoạt rule theo framework

```bash
# Đang làm React/Next.js?
cp clinerules-bank/frameworks/react-nextjs.md .clinerules/09-framework.md

# Đang làm Python/FastAPI?
cp clinerules-bank/frameworks/python-fastapi.md .clinerules/09-framework.md

# Đang làm dự án Việt Nam?
cp clinerules-bank/vietnamese/vi-project-context.md .clinerules/10-vietnamese.md

# Đầu sprint mới?
cp clinerules-bank/contexts/current-sprint-template.md .clinerules/10-sprint.md
# → Điền thông tin sprint vào file vừa copy
```

---

## 👥 Quy trình cho Team

> Chỉ cần **1 người** setup, các thành viên còn lại chỉ cần bước 2 + 7

1. **Người setup** (làm 1 lần):
   - Copy 3 thư mục vào project
   - Chạy `initialize memory bank`
   - Tạo nhánh mới từ `develop`, commit toàn bộ
   - Tạo PR merge vào `develop`

2. **Tất cả thành viên**:
   - Merge nhánh `develop` về local
   - Cấu hình Custom Instructions (Bước 2)
   - Bắt đầu dùng ngay!

---

## 💡 Tips

| Muốn làm gì | Lệnh |
|------------|------|
| Cline nhớ lại context | `follow your custom instructions` |
| Cập nhật memory sau task lớn | `update memory bank` |
| Reset và tạo lại memory bank | `initialize memory bank` |
| Cline giải thích vừa làm gì | `explain your last changes` |

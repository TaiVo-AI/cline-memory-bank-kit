---
description: Hướng dẫn cấu hình và sử dụng MCP (Model Context Protocol) servers trong dự án.
author: TaiVo-AI
version: 1.0
tags: ["mcp", "tools", "integration", "configuration"]
---

# MCP Configuration

Hướng dẫn sử dụng MCP servers để mở rộng khả năng của Cline.

## MCP Servers phổ biến

### GitHub Integration
```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "${GITHUB_TOKEN}"
      }
    }
  }
}
```

### Filesystem (truy cập thư mục cụ thể)
```json
{
  "filesystem": {
    "command": "npx",
    "args": ["-y", "@modelcontextprotocol/server-filesystem", "/path/to/project"]
  }
}
```

### Brave Search (web search)
```json
{
  "brave-search": {
    "command": "npx",
    "args": ["-y", "@modelcontextprotocol/server-brave-search"],
    "env": { "BRAVE_API_KEY": "${BRAVE_API_KEY}" }
  }
}
```

### PostgreSQL Database
```json
{
  "postgres": {
    "command": "npx",
    "args": ["-y", "@modelcontextprotocol/server-postgres", "${DATABASE_URL}"]
  }
}
```

## Vị trí file cấu hình

| OS | Đường dẫn |
|----|-----------|
| Windows | `%APPDATA%\Code\User\globalStorage\saoudrizwan.claude-dev\settings\cline_mcp_settings.json` |
| macOS | `~/Library/Application Support/Code/User/globalStorage/saoudrizwan.claude-dev/settings/cline_mcp_settings.json` |
| Linux | `~/.config/Code/User/globalStorage/saoudrizwan.claude-dev/settings/cline_mcp_settings.json` |

## Quy tắc sử dụng MCP

- **Ưu tiên MCP** hơn web search khi có tool phù hợp (GitHub MCP > tìm kiếm thủ công)
- **Secrets**: KHÔNG hardcode API keys — dùng biến môi trường hoặc `${ENV_VAR}`
- **Phạm vi tối thiểu**: Chỉ cấp quyền filesystem cho thư mục cần thiết
- **Ghi lại vào techContext.md**: MCP servers nào đang dùng và mục đích

## Cách kích hoạt MCP trong task

Khi cần dùng MCP tool, nói rõ:
```
Hãy dùng GitHub MCP để tạo issue mới với tiêu đề "[BUG] Login redirect loop"
```

Cline sẽ tự động sử dụng tool phù hợp nếu MCP server đã được cấu hình.

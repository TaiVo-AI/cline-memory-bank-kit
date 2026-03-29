---
description: Rules cho dự án Electron + React — cấu trúc main/renderer process, IPC communication, bảo mật, packaging.
author: TaiVo-AI
version: 1.0
tags: ["electron", "react", "typescript", "desktop", "ipc"]
globs: ["**/*.tsx", "**/*.ts", "electron/**/*", "src/main/**/*", "src/renderer/**/*"]
---

# Electron + React Guidelines

## Cấu trúc Project

```
src/
├── main/                   # Main process (Node.js)
│   ├── index.ts            # Entry point
│   ├── windows/            # BrowserWindow management
│   ├── ipc/                # IPC handlers
│   └── services/           # Native services (file, tray, menu)
├── renderer/               # Renderer process (React)
│   ├── App.tsx
│   ├── components/
│   ├── hooks/
│   ├── pages/
│   └── store/
├── preload/                # Preload scripts
│   └── index.ts
└── shared/                 # Types dùng chung cả 2 process
    └── types.ts
```

## IPC Communication — Quy tắc bắt buộc

```typescript
// ✅ ĐÚNG — Định nghĩa IPC channels trong shared/types.ts
export interface IpcChannels {
  'app:get-version': { request: void; response: string };
  'file:open-dialog': { request: OpenDialogOptions; response: string[] };
  'settings:save': { request: AppSettings; response: boolean };
}

// ✅ ĐÚNG — Preload script expose API an toàn
// preload/index.ts
contextBridge.exposeInMainWorld('electronAPI', {
  getVersion: () => ipcRenderer.invoke('app:get-version'),
  openFile: (opts: OpenDialogOptions) => ipcRenderer.invoke('file:open-dialog', opts),
  saveSettings: (s: AppSettings) => ipcRenderer.invoke('settings:save', s),
});

// ✅ ĐÚNG — Renderer dùng qua window.electronAPI
const version = await window.electronAPI.getVersion();

// ❌ SAI — KHÔNG dùng remote module
const { app } = require('@electron/remote'); // BANNED

// ❌ SAI — KHÔNG require node modules trực tiếp trong renderer
const fs = require('fs'); // BANNED trong renderer
```

## Bảo mật — KHÔNG được bỏ qua

```typescript
// ✅ ĐÚNG — BrowserWindow config an toàn
new BrowserWindow({
  webPreferences: {
    preload: path.join(__dirname, '../preload/index.js'),
    contextIsolation: true,   // BẮT BUỘC true
    nodeIntegration: false,   // BẮT BUỘC false
    sandbox: true,            // Bật nếu có thể
    webSecurity: true,        // KHÔNG tắt trong production
  },
});

// ✅ Validate tất cả IPC input ở main process
ipcMain.handle('file:open-dialog', async (event, opts: OpenDialogOptions) => {
  // Validate opts trước khi dùng
  if (!isValidOpenDialogOptions(opts)) throw new Error('Invalid options');
  return dialog.showOpenDialog(opts);
});
```

## React trong Renderer

- Dùng **React 18+** với functional components + hooks
- State management: **Zustand** cho global state (nhẹ hơn Redux, phù hợp desktop)
- Không fetch HTTP trực tiếp từ renderer — đi qua IPC → main process
- Dùng `useEffect` cleanup để hủy IPC listeners:

```typescript
useEffect(() => {
  const handler = (_: IpcRendererEvent, data: UpdateInfo) => setUpdate(data);
  window.electronAPI.onUpdateAvailable(handler);
  return () => window.electronAPI.removeUpdateListener(handler);
}, []);
```

## Window Management

```typescript
// main/windows/main-window.ts
export function createMainWindow(): BrowserWindow {
  const win = new BrowserWindow({ /* config */ });

  // Lưu/restore window bounds
  const bounds = store.get('windowBounds');
  if (bounds) win.setBounds(bounds);

  win.on('close', () => store.set('windowBounds', win.getBounds()));
  return win;
}
```

## Build & Packaging

- Dùng **electron-builder** hoặc **electron-forge**
- Code signing bắt buộc cho production (macOS notarization, Windows Authenticode)
- Auto-update: **electron-updater** với staged rollouts
- `.env` production: dùng `electron-store` cho user settings, KHÔNG dùng `.env` file

## Scripts chuẩn

```bash
pnpm dev          # Dev mode (main + renderer hot reload)
pnpm build        # Production build
pnpm dist         # Package thành installer
pnpm dist:mac     # macOS .dmg
pnpm dist:win     # Windows .exe
pnpm dist:linux   # Linux .AppImage
```

## Không được làm

- KHÔNG `nodeIntegration: true` trong production
- KHÔNG tắt `contextIsolation`
- KHÔNG dùng `eval()` trong renderer
- KHÔNG hardcode path separator (`/` hoặc `\`) — dùng `path.join()`
- KHÔNG block main process với sync operations — dùng async

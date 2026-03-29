# React & Next.js Guidelines

> **Cách dùng:** Copy file này vào `.clinerules/` khi làm việc với React/Next.js project.

## Component Design

- Dùng **functional components** với hooks — không dùng class components
- Mỗi component một file, export default
- Props interface đặt ngay trên component:
  ```typescript
  interface ButtonProps {
    label: string;
    onClick: () => void;
    variant?: 'primary' | 'secondary';
    disabled?: boolean;
  }

  export default function Button({ label, onClick, variant = 'primary', disabled }: ButtonProps) { ... }
  ```

## Hooks Rules

- Tách logic phức tạp ra custom hook (`useXxx`)
- Custom hooks sống trong `src/hooks/`
- Không gọi hook trong condition, loop, hoặc nested function

## State Management

- Local state: `useState` / `useReducer`
- Server state: React Query hoặc SWR (không fetch trong useEffect)
- Global client state: Zustand hoặc Context (tránh prop drilling > 2 cấp)

```typescript
// ✅ Đúng — React Query
const { data: user, isLoading } = useQuery({
  queryKey: ['user', userId],
  queryFn: () => fetchUser(userId),
});

// ❌ Sai — fetch trong useEffect
useEffect(() => {
  fetch(`/api/user/${userId}`).then(r => r.json()).then(setUser);
}, [userId]);
```

## Next.js Specific

- Dùng **App Router** (Next.js 13+), không dùng Pages Router cho project mới
- Server Components mặc định — thêm `'use client'` chỉ khi cần
- Data fetching trong Server Components:
  ```typescript
  // app/users/page.tsx — Server Component
  export default async function UsersPage() {
    const users = await fetchUsers(); // fetch trực tiếp, không cần useEffect
    return <UserList users={users} />;
  }
  ```
- Image: luôn dùng `next/image`, không dùng `<img>` thẳng
- Link: luôn dùng `next/link`, không dùng `<a>` cho internal routes

## File Structure (App Router)

```
src/
├── app/                    # Routes
│   ├── (auth)/             # Route group
│   │   ├── login/page.tsx
│   │   └── register/page.tsx
│   ├── dashboard/
│   │   ├── layout.tsx
│   │   └── page.tsx
│   └── api/                # Route handlers
├── components/
│   ├── ui/                 # Atomic components
│   └── features/           # Feature-specific components
├── hooks/                  # Custom hooks
├── lib/                    # Utilities, API clients
└── types/                  # TypeScript types
```

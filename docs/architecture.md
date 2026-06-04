# Architecture

## Stack

- **React 19**, TypeScript
- **Vite 8** + `@tailwindcss/vite` (Tailwind v4)
- **Zustand 5** for state management
- **Axios** with request/response interceptors for JWT attachment and 401 redirect
- **React Router v7** for routing
- **date-fns** for date formatting
- **lucide-react** for icons

## Project Layout

```
src/
  api/          — Axios wrappers: client.ts, auth.ts, tasks.ts, projects.ts
  store/        — Zustand stores: auth.ts, tasks.ts, theme.ts
  types/        — index.ts: all shared TypeScript types
  pages/        — Today, Upcoming, Inbox, ProjectView, Login, Register
  components/
    Layout/     — AppLayout (route shell), Sidebar
    Task/       — TaskRow, TaskList, TaskDetailModal, AddTaskForm, ProgressRing
```

## State Management

`useTasksStore` (Zustand) holds the task tree as `Task[]`, where each `Task` has a nested `children: Task[]`. The full subtree is always present — the API always returns tasks with all descendants loaded.

Tree mutations (`replaceTask`, `removeTask`, `insertChild`) walk the nested structure recursively and return a new tree — no in-place mutation.

Modal state (`modalTaskId`) lives in the tasks store so it can be triggered from any component in the tree.

## Auth

JWT is stored in `localStorage` under `openplan_token`. The axios interceptor in `client.ts` attaches it to every request and redirects to `/login` on 401. The user object is stored under `openplan_user` and rehydrated into the auth store on page load.

## Routing

React Router v7. All authenticated views are children of `AppLayout`, which renders the sidebar and an `<Outlet>`. Login/Register are standalone routes. `AppLayout` also renders `TaskDetailModal` when `modalTaskId` is set.

## Theme

Theme preference (`system` / `light` / `dark`) is stored in `localStorage` under `openplan_theme`. The `useThemeStore` store manages it. On load, `main.tsx` applies the initial theme by adding or removing the `.dark` class from `document.documentElement`. Tailwind v4 uses `@custom-variant dark` for class-based dark mode.

## Task Model Rules (Frontend Invariants)

These match the server-side rules and must be preserved in any UI changes:

1. **Sequential checkbox** — ticking a sequential parent completes its next uncompleted child, not the parent itself
2. **Parallel checkbox** — ticking a parallel parent with children completes all children recursively
3. **Sequential progress** — children after the first uncompleted one count toward the denominator but contribute 0 to the numerator
4. **`effectivePriority`** — minimum P-number across all uncompleted descendants (including self)
5. **Cascading cancel** — cancelling a task recursively cancels all descendants
6. **`completedAt`** — set automatically by the server; never set from the client

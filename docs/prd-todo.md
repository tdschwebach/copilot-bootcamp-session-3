# Product Requirements Document (PRD) - Todo App Upgrade

## 1. Overview

We are upgrading the basic Todo app so users can better organize and track work without adding unnecessary complexity. The MVP focuses on teachable, frontend-only improvements: optional due dates, simple task priorities, and date-based filters, while keeping storage local and avoiding backend changes.

---

## 2. MVP Scope

- Add an optional `dueDate` field to each task.
- Store `dueDate` in ISO `YYYY-MM-DD` format.
- Add a `priority` field with allowed values `P1`, `P2`, and `P3`.
- Default `priority` to `P3` when not provided.
- Add filter views for `All`, `Today`, and `Overdue`.
- In the `All` view, include completed tasks.
- In the `Today` and `Overdue` views, show only incomplete tasks.
- Keep data storage local only, with no backend or external storage changes.
- Require `title` for every task.
- Treat invalid `dueDate` values as absent rather than failing or storing bad data.

---

## 3. Post-MVP Scope

- Visually highlight overdue tasks so they stand out in the UI.
- Add sorting in this order: overdue tasks first, then priority from `P1` to `P3`, then due date ascending, then tasks without a due date last.
- Add visual priority badges or color-coding for `P1`, `P2`, and `P3`.

---

## 4. Out of Scope

- Notifications
- Recurring tasks
- Multi-user functionality
- Keyboard navigation and additional accessibility enhancements
- Backend changes
- External storage or persistence beyond local storage
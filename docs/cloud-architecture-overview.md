# Cloud Architecture Overview

This monorepo currently runs as a simple local application stack with a React frontend, an Express API, and an in-memory SQLite-backed store inside the backend process.

## System Context

```mermaid
flowchart LR
    User[User in Browser]
    Frontend[React Frontend\npackages/frontend]
    API[Express API\npackages/backend]
    Store[(In-Memory Store\nSQLite in backend process)]

    User -->|Uses UI| Frontend
    Frontend -->|HTTP requests to /api/tasks| API
    API -->|Reads and writes tasks| Store
```

## Create TODO Sequence

```mermaid
sequenceDiagram
    actor User
    participant Frontend as React Frontend
    participant API as Express API
    participant Store as In-Memory SQLite Store

    User->>Frontend: Enter title and submit task form
    Frontend->>Frontend: Validate required title
    Frontend->>API: POST /api/tasks
    Note over Frontend,API: Request body includes title and optional task fields
    API->>API: Validate request payload
    API->>Store: Insert task record
    Store-->>API: Return created task
    API-->>Frontend: 201 Created with task JSON
    Frontend->>API: GET /api/tasks
    API->>Store: Query current tasks
    Store-->>API: Return task list
    API-->>Frontend: 200 OK with tasks
    Frontend-->>User: Render updated TODO list
```

## Notes

- The frontend is a React application in packages/frontend.
- The backend is an Express application in packages/backend.
- Task data is stored in an in-memory SQLite database inside the backend process.
- The current architecture is local and ephemeral rather than persistent cloud infrastructure.
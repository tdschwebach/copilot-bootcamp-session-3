Based on the requirements defined in docs/prd-todo.md.

Technical requirements should reference the current frontend and backend implementation in this codebase, especially the task form and list flow in packages/frontend/src and the task API and SQLite data model in packages/backend/src/app.js.

## MVP

- Epic: Task Data Model Enhancements
  - Story: Add optional due date field to tasks
    - Acceptance Criteria: Tasks can be created and saved without a due date.
    - Acceptance Criteria: A task may include a due date value when one is provided.
    - Technical Requirements: Extend the shared task shape used by the frontend and backend to consistently support a due date field.
    - Technical Requirements: Preserve compatibility with the backend `due_date` column and frontend date input handling already present in the current implementation.
  - Story: Add priority field with P1, P2, and P3 values
    - Acceptance Criteria: Each task supports a priority value of P1, P2, or P3.
    - Acceptance Criteria: Values outside P1, P2, and P3 are not treated as valid priorities.
    - Technical Requirements: Add a priority field to the backend task data model and include it in API create, update, and fetch responses.
    - Technical Requirements: Update frontend task create and edit payloads so priority is submitted and rendered consistently.
    - Technical Requirements: Constrain accepted priority values to P1, P2, and P3 in backend request validation.
  - Story: Default priority to P3 for new tasks
    - Acceptance Criteria: New tasks are assigned priority P3 when no priority is provided.
    - Technical Requirements: Apply the default priority in backend create logic so all clients receive consistent task data.
    - Technical Requirements: Initialize the frontend task form to P3 when no existing task priority is present.
  - Story: Ignore invalid due date values
    - Acceptance Criteria: Invalid due date values are treated as absent.
    - Acceptance Criteria: Invalid due date values do not prevent a task from being saved.
    - Technical Requirements: Add backend date validation for incoming task payloads and normalize invalid due dates to null.
    - Technical Requirements: Keep frontend submission behavior compatible with backend normalization so invalid dates do not break create or edit flows.

- Epic: Task Creation and Editing
  - Story: Require title when saving a task
    - Acceptance Criteria: A task cannot be saved without a title.
    - Technical Requirements: Preserve the existing frontend title validation in the task form and ensure the backend remains the source of truth for rejecting missing titles.
    - Technical Requirements: Keep create and update endpoints aligned so title validation behaves the same for new and edited tasks.
  - Story: Capture due date when creating a task
    - Acceptance Criteria: Users can provide an optional due date when creating a task.
    - Acceptance Criteria: When a due date is saved, it uses ISO YYYY-MM-DD format.
    - Technical Requirements: Reuse the existing date input in the frontend task form and keep it bound to ISO YYYY-MM-DD values.
    - Technical Requirements: Ensure POST and PUT task payloads continue to serialize the due date in a format accepted by the backend SQLite layer.
  - Story: Capture priority when creating a task
    - Acceptance Criteria: Users can assign P1, P2, or P3 priority when creating a task.
    - Acceptance Criteria: If no priority is selected, the saved task uses P3.
    - Technical Requirements: Add a priority control to the existing frontend task form for both create and edit modes.
    - Technical Requirements: Update the save handler in the frontend app container so priority is included in task requests.

- Epic: Task Filtering
  - Story: Add All tasks filter
    - Acceptance Criteria: Users can switch to an All view.
    - Acceptance Criteria: The All view displays all tasks regardless of due date status.
    - Technical Requirements: Add filter state to the frontend task list flow and default it to All.
    - Technical Requirements: Ensure the All view can be fulfilled from the current task fetch flow without excluding completed tasks.
  - Story: Add Today tasks filter
    - Acceptance Criteria: Users can switch to a Today view.
    - Acceptance Criteria: The Today view only includes tasks due today.
    - Technical Requirements: Implement Today filtering against the current task list using the task due date field returned by the backend.
    - Technical Requirements: Compare dates in a consistent local-date-safe way to avoid timezone drift with YYYY-MM-DD values.
  - Story: Add Overdue tasks filter
    - Acceptance Criteria: Users can switch to an Overdue view.
    - Acceptance Criteria: The Overdue view only includes tasks with due dates earlier than today.
    - Technical Requirements: Implement Overdue filtering using the current task fetch results and task due date field.
    - Technical Requirements: Define overdue logic so tasks are considered overdue only when the due date is before today.
  - Story: Show completed tasks in All view
    - Acceptance Criteria: Completed tasks remain visible in the All view.
    - Technical Requirements: Keep the All filter logic inclusive of both completed and incomplete tasks in the frontend list rendering.
  - Story: Hide completed tasks in Today view
    - Acceptance Criteria: Completed tasks are excluded from the Today view.
    - Technical Requirements: Combine Today filtering with task completion state so only incomplete tasks render in that view.
  - Story: Hide completed tasks in Overdue view
    - Acceptance Criteria: Completed tasks are excluded from the Overdue view.
    - Technical Requirements: Combine Overdue filtering with task completion state so only incomplete tasks render in that view.

- Epic: Local Task Persistence
  - Story: Keep task data stored locally
    - Acceptance Criteria: Task data is stored locally.
    - Acceptance Criteria: No backend or external storage is required for task persistence.
    - Technical Requirements: Do not introduce any remote persistence dependency beyond the current local application stack.
    - Technical Requirements: Keep persistence within the existing local backend process and SQLite task store used by the codebase.

## Post-MVP

- Epic: Overdue Task Visibility
  - Story: Highlight overdue tasks in the task list
    - Acceptance Criteria: Overdue tasks are visually distinct from non-overdue tasks.
    - Technical Requirements: Extend the existing task list item styling to derive an overdue visual state from due date and completion status.
    - Technical Requirements: Do not apply overdue highlighting to completed tasks.

- Epic: Task Sorting
  - Story: Sort overdue tasks before other tasks
    - Acceptance Criteria: Overdue tasks appear before non-overdue tasks in the sorted list.
    - Technical Requirements: Replace the current due-date-first backend ordering with sorting that prioritizes overdue status before other criteria.
  - Story: Sort tasks by priority from P1 to P3
    - Acceptance Criteria: Higher priority tasks appear before lower priority tasks.
    - Acceptance Criteria: Priority order is P1, then P2, then P3.
    - Technical Requirements: Define a deterministic priority sort order that maps P1 above P2 above P3.
    - Technical Requirements: Ensure the selected sorting layer has access to the new priority field for all returned tasks.
  - Story: Sort tasks by due date in ascending order
    - Acceptance Criteria: Tasks with due dates are ordered from earliest date to latest date.
    - Technical Requirements: Preserve ascending comparison of valid due dates after overdue and priority ordering are applied.
  - Story: Place tasks without due dates last
    - Acceptance Criteria: Tasks without due dates appear after tasks that have due dates.
    - Technical Requirements: Maintain explicit ordering logic that places null or absent due dates after dated tasks.

- Epic: Priority Visualization
  - Story: Show priority badges on tasks
    - Acceptance Criteria: Each task displays a visible priority indicator.
    - Technical Requirements: Extend the current task list item UI to render a priority indicator alongside the existing due date metadata.
    - Technical Requirements: Ensure the indicator is available in both default and filtered task views.
  - Story: Apply distinct colors for P1, P2, and P3
    - Acceptance Criteria: P1, P2, and P3 each use a distinct visual color treatment.
    - Technical Requirements: Define stable color mappings for each priority value and apply them consistently in the task list UI.
    - Technical Requirements: Keep priority color rendering driven by task data rather than hard-coded per-item exceptions.
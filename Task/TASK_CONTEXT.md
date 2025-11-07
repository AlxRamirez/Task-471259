# Task Context - Task 471259: Archive Action (Frontend)

## Task Information

- **Task ID:** 471259
- **Sprint:** Sprint 6 - Reliability Page Development
- **Type:** Feature - Frontend Implementation
- **Priority:** High
- **Assigned To:** Alex

---

## Objective

Implement the **Archive** functionality for selected tasks in the Tasks table on the Reliability page.

---

## Current State

### Reliability Page (Existing)
- **Location:** `src/components/reliability/Reliability.tsx`
- **Status:** Already exists with basic structure
- **Features:**
  - 4 tabs: Tasks, Plans, Platform, Load to System
  - 3 cascading dropdowns: System → Facility → Project
  - Basic HTML table for tasks (NOT Kendo Grid yet)
  - Mock data in local state (NOT loading from backend yet)

### Current Tasks Table (HTML)
```tsx
<table className="table">
  <thead>
    <tr>
      <th>Task Name</th>
      <th>Task Status</th>
    </tr>
  </thead>
  <tbody>
    {tasks.map((task) => (
      <tr key={task.taskId}>
        <td>{task.taskName}</td>
        <td>
          <span className={`badge ${task.status === TaskStatus.APPROVED ? "badge-completed" : ...}`}>
            {task.status}
          </span>
        </td>
      </tr>
    ))}
  </tbody>
</table>
```

---

## Functional Requirements

### Must Have (Core Functionality)

1. **Replace HTML Table with Kendo Grid**
   - Use `@progress/kendo-react-grid` component
   - Maintain current visual styling
   - Add columns as needed

2. **Checkbox Selection**
   - Enable multi-row selection via checkboxes
   - Track selected tasks in state
   - Display count of selected tasks

3. **Archive Button**
   - Visible only when tasks are selected
   - Disabled when no tasks selected
   - Shows count: "Archive (3 tasks)"

4. **Confirmation Dialog**
   - Display before archiving
   - Show list of selected task names
   - "Cancel" and "Confirm" buttons

5. **API Integration**
   - Call PUT `/api/tasks/archive` endpoint
   - Pass selected task IDs and user email
   - Handle success and error responses

6. **Loading State**
   - Show spinner/loading indicator during API call
   - Disable UI interactions while loading

7. **Success Handling**
   - Show success message: "X task(s) archived successfully"
   - Automatically refresh tasks table (remove archived tasks)
   - Clear selection state

8. **Error Handling**
   - Show error message if API call fails
   - Maintain selection state on error
   - Allow user to retry

9. **Task Visibility**
   - Display only tasks with status: APPROVED or PENDING
   - Filter out ARCHIVED tasks from the view

---

## User Flow

1. User selects System, Facility, and Project from dropdowns
2. Tasks table loads (GET `/api/tasks/tasksbyproject`)
3. User selects one or more tasks via checkboxes
4. "Archive" button appears/enables
5. User clicks "Archive" button
6. Confirmation dialog appears showing selected tasks
7. User clicks "Confirm"
8. Loading state activates
9. API call executes (PUT `/api/tasks/archive`)
10. On success:
    - Success message displays
    - Table refreshes (archived tasks disappear)
    - Selection clears
11. On error:
    - Error message displays
    - Selection remains
    - User can retry

---

## Edge Cases to Handle

### No Selection
- Archive button should be disabled
- Should not be possible to trigger archive action

### Network Error
- Display user-friendly error message
- Keep tasks selected so user can retry
- Log error to console for debugging

### Partial Success (if backend supports)
- Show message: "X of Y tasks archived successfully"
- Refresh table to show current state
- Display which tasks failed (if info available)

### User Email Missing
- Backend requires email for archive operation
- Frontend must extract email from JWT token in localStorage
- Handle case where token is invalid/missing

### Empty Tasks List
- Display message: "No tasks available"
- Archive button should not be visible

---

## Implementation Notes

### Task Loading from Backend
- Currently tasks are mocked in local state
- Must implement: `fetchTasksByProject(projectId)` in a service file
- Use the existing GET `/api/tasks/tasksbyproject` endpoint
- Transform API response to match frontend `Task` interface

### User Email Extraction
- JWT token is stored in localStorage as `id_token`
- Need to decode JWT to extract email claim
- Consider creating a utility function: `getUserEmailFromToken()`

### State Management
- `useState` for selected tasks (array of task IDs)
- `useState` for loading state (boolean)
- `useState` for error/success messages (string or null)
- No external state management library

### Kendo Grid Configuration
- Enable selection: `selectedField` pattern
- Define columns: Task Name, Status, other fields as needed
- Apply existing CSS styling to match current design

---

## Files to Create/Modify

### Files to Modify
1. **`src/components/reliability/Reliability.tsx`**
   - Replace HTML table with Kendo Grid
   - Add selection state management
   - Add Archive button and dialog
   - Integrate with archive API

2. **`src/components/reliability/Reliability.css`**
   - Update styles for Kendo Grid
   - Add styles for Archive button
   - Add styles for confirmation dialog

3. **`types/tasks.types.ts`**
   - Expand Task interface with additional fields from API
   - Add request/response types for archive operation

### Files to Create
1. **`src/services/taskService.ts`** (or similar)
   - `fetchTasksByProject(projectId): Promise<Task[]>`
   - `archiveTasks(taskIds: number[], email: string): Promise<ArchiveResponse>`

2. **`src/utils/jwtUtils.ts`** (optional but recommended)
   - `getUserEmailFromToken(): string | null`
   - Helper to decode and extract claims from JWT

---

## Testing Checklist

- [ ] Table loads tasks correctly from API
- [ ] Tasks with ARCHIVED status are filtered out
- [ ] Checkbox selection works for single task
- [ ] Checkbox selection works for multiple tasks
- [ ] Archive button appears when tasks selected
- [ ] Archive button shows correct count
- [ ] Archive button disabled when no selection
- [ ] Confirmation dialog shows selected tasks
- [ ] Cancel button in dialog works (closes dialog, maintains selection)
- [ ] Confirm button triggers API call
- [ ] Loading state displays during API call
- [ ] Success: Tasks disappear from table
- [ ] Success: Success message displays
- [ ] Success: Selection clears
- [ ] Error: Error message displays
- [ ] Error: Selection maintained
- [ ] Error: User can retry
- [ ] Invalid email: Appropriate error handling

---

## Success Criteria

The task is complete when:
1. User can select multiple tasks via Kendo Grid checkboxes
2. Archive button appears and functions correctly
3. Confirmation dialog prevents accidental archives
4. API integration works (tasks are archived in backend)
5. UI updates correctly (archived tasks disappear)
6. All error cases are handled gracefully
7. Code follows project standards (naming, structure, patterns)

---

## Next Tasks (After 471259)

- **Task 471260:** Copy Tasks - Similar pattern with PUT `/api/tasks/copy`
- **Task 471261:** Unlink Tasks - Similar pattern with PUT `/api/tasks/unlink`

These will follow the same architectural pattern established in this task.

---

**Last Updated:** 2025-11-07  
**Status:** Volatile - Specific to Task 471259  
**Usage:** Include when working on Task 471259 only

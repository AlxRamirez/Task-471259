# Code Standards - mfg-asset-strategy

## Overview

This document defines coding standards and patterns specific to the mfg-asset-strategy project.

---

## Naming Conventions

### Backend (C#)

#### Classes and Interfaces
- **Classes:** PascalCase (e.g., `ProjectRepository`, `TaskController`)
- **Interfaces:** Start with `I` + PascalCase (e.g., `IProjectRepository`, `ITaskService`)
- **Abstract Classes:** PascalCase, consider `Base` prefix if applicable

#### Methods and Properties
- **Methods:** PascalCase (e.g., `GetProjectById`, `ArchiveTasks`)
- **Properties:** PascalCase (e.g., `ProjectId`, `TaskName`)
- **Private Fields:** `_camelCase` with underscore prefix (e.g., `_dbContext`, `_logger`)

#### Constants and Enums
- **Constants:** PascalCase (e.g., `MaxRetryAttempts`)
- **Enums:** PascalCase for type and values (e.g., `TaskStatus.Approved`)

### Frontend (React/TypeScript)

#### Components and Files
- **Components:** PascalCase (e.g., `Reliability.tsx`, `ReusableDataWidget.tsx`)
- **CSS Files:** Same name as component (e.g., `Reliability.css`)
- **Utility Files:** kebab-case (e.g., `auth-util.ts`, `api.ts`)

#### Variables and Functions
- **Variables:** camelCase (e.g., `selectedProject`, `isLoading`)
- **Functions:** camelCase (e.g., `fetchProjects`, `handleArchive`)
- **React Components:** PascalCase function (e.g., `const Reliability: React.FC = () => {}`)

#### Constants and Enums
- **Constants:** UPPER_SNAKE_CASE for true constants (e.g., `API_BASE_URL`)
- **Enums:** PascalCase for type, UPPER_CASE for values (e.g., `TaskStatus.APPROVED`)

#### CSS Classes
- **Pattern:** kebab-case (e.g., `.page-header`, `.content-widget`, `.dropdown-label`)
- **Scope:** Prefix with component name for specificity (e.g., `.reliability-content`)
- **Avoid:** camelCase or PascalCase in CSS

---

## React Component Structure

### Standard Component Template

```typescript
import React, { useState, useEffect } from "react";
import "./ComponentName.css";

const ComponentName: React.FC = () => {
  // 1. State declarations
  const [state, setState] = useState<Type>(initialValue);
  
  // 2. useEffect hooks
  useEffect(() => {
    // side effects
  }, [dependencies]);
  
  // 3. Event handlers and helper functions
  const handleAction = () => {
    // logic
  };
  
  // 4. Render
  return (
    <div className="component-name">
      {/* JSX */}
    </div>
  );
};

export default ComponentName;
```

### Component Organization
1. Imports (React, external libraries, local components, styles)
2. Type definitions (if not in separate file)
3. Component definition
4. State and hooks
5. Helper functions and event handlers
6. Return/JSX
7. Export

---

## TypeScript Patterns

### Type Definitions
- **Location:** Separate files in `types/` directory
- **Naming:** `[entity].types.ts` (e.g., `tasks.types.ts`, `projects.types.ts`)
- **Enums:** Separate files with `.enum.ts` suffix (e.g., `taskstatus.enum.ts`)

### Example Type File
```typescript
import { TaskStatus } from './taskstatus.enum';

export interface Task {
  taskId: number;
  taskName: string;
  status: TaskStatus;
  // ... other fields
}
```

### Type vs Interface
- **Prefer interfaces** for object shapes
- **Use types** for unions, intersections, or primitives

---

## API Communication Pattern

### makeAuthenticatedRequest Helper

**Location:** `src/utils/api.ts`

**Pattern:**
```typescript
export const fetchDataFromApi = async () => {
  const apiBaseUrl = getApiBaseUrl();
  const response = await makeAuthenticatedRequest(
    `${apiBaseUrl}/API/endpoint`
  );
  
  if (!response.ok) {
    throw new Error(`HTTP error! status: ${response.status}`);
  }
  
  const apiData = await response.json();
  
  // Transform API data to match frontend types
  const transformedData: FrontendType[] = apiData.map((item: any) => ({
    id: item.apiFieldName,
    name: item.otherApiField,
    // ... map API fields to frontend fields
  }));
  
  return transformedData;
};
```

### Key Points
- Always use `makeAuthenticatedRequest()` for authenticated API calls
- Always check `response.ok` before parsing JSON
- Transform API data to match frontend types (don't use API structure directly)
- Handle errors with try-catch in calling code

---

## Error Handling

### Backend (C#)
```csharp
try
{
    // Operation
    return Ok(result);
}
catch (NotFoundException ex)
{
    return NotFound(ex.Message);
}
catch (ValidationException ex)
{
    return BadRequest(ex.Message);
}
catch (Exception ex)
{
    _logger.LogError(ex, "Unexpected error");
    return StatusCode(500, "Internal server error");
}
```

### Frontend (React)
```typescript
try {
  const response = await makeAuthenticatedRequest(url, options);
  if (!response.ok) {
    throw new Error(`HTTP error! status: ${response.status}`);
  }
  const data = await response.json();
  // Handle success
} catch (error) {
  console.error("Error:", error);
  // Show user-friendly error message
  // Maintain UI state appropriately
}
```

---

## CSS Standards

### Class Naming
- **Use kebab-case:** `.page-header`, `.content-widget`
- **Component scope:** `.reliability-content`, `.dashboard-widget`
- **State classes:** `.is-active`, `.is-loading`, `.is-disabled`

### Organization
```css
/* 1. Layout */
.component-name {
  display: flex;
  padding: 1rem;
}

/* 2. Typography */
.component-title {
  font-size: 1.5rem;
  font-weight: 600;
}

/* 3. States */
.component-name.is-active {
  background-color: blue;
}

/* 4. Responsive */
@media (max-width: 768px) {
  .component-name {
    padding: 0.5rem;
  }
}
```

### Avoid
- Inline styles (use CSS classes)
- !important (unless absolutely necessary)
- Deep nesting (keep specificity low)

---

## State Management Patterns

### useState
```typescript
// Simple state
const [isLoading, setIsLoading] = useState<boolean>(false);

// Object state
const [user, setUser] = useState<User | null>(null);

// Array state
const [tasks, setTasks] = useState<Task[]>([]);
```

### useEffect
```typescript
// Run once on mount
useEffect(() => {
  fetchData();
}, []);

// Run when dependencies change
useEffect(() => {
  if (projectId) {
    fetchProjectData(projectId);
  }
}, [projectId]);

// Cleanup
useEffect(() => {
  const subscription = subscribeToData();
  return () => subscription.unsubscribe();
}, []);
```

---

## Comments and Documentation

### When to Comment
- Complex business logic
- Non-obvious workarounds
- TODOs with task numbers
- Public API methods (backend)

### When NOT to Comment
- Obvious code ("increment counter")
- Restating variable names
- Code that should be refactored instead

### Format
```typescript
// Good: Explains WHY
// Using raw SQL here because LINQ doesn't support this PostgreSQL-specific syntax
const result = await connection.QueryAsync(sql);

// Bad: Explains WHAT (obvious)
// Set isLoading to true
setIsLoading(true);
```

---

## Git Commit Messages

### Format
```
<type>: <short description>

<optional longer description>

<optional footer>
```

### Types
- `feat:` New feature
- `fix:` Bug fix
- `refactor:` Code refactoring
- `style:` Formatting, whitespace
- `docs:` Documentation
- `test:` Tests
- `chore:` Build, dependencies

### Examples
```
feat: implement archive action for tasks

fix: resolve null reference in ProjectRepository

refactor: extract API call logic to service layer
```

---

## Testing Standards

### Backend (xUnit)
- Test class naming: `[ClassName]Tests`
- Test method naming: `[Method]_[Scenario]_[ExpectedResult]`
- Arrange-Act-Assert pattern

### Frontend (Playwright)
- Located in `playwright.mcp/` directory
- Focus on integration and E2E tests
- API testing in `src/tests/api-*.spec.ts`

---

**Last Updated:** 2025-11-07  
**Status:** Semi-Stable - Changes when project standards evolve  
**Usage:** Reference when writing new code for mfg-asset-strategy project

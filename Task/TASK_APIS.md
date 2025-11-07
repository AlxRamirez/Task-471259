# Task APIs - Task 471259: Archive Action

## Overview

This document describes the backend APIs required for implementing the Archive functionality in Task 471259.

---

## API Endpoints

### 1. GET /api/tasks/tasksbyproject

**Purpose:** Load tasks for a specific project

**Status:** Already implemented in backend (ready to use)

#### Request

**Method:** GET  
**URL:** `{API_BASE_URL}/api/tasks/tasksbyproject`

**Query Parameters:**
- `projectId` (required): ID of the project
- `pageNumber` (optional): Page number (default: 1)
- `pageSize` (optional): Items per page (default: 25)

**Headers:**
- `Authorization: Bearer {id_token}`
- `Content-Type: application/json`

**Example Request:**
```
GET https://localhost:56802/api/tasks/tasksbyproject?projectId=123&pageNumber=1&pageSize=25
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

#### Response (200 OK)

```json
{
  "tasks": [
    {
      "taskId": 1,
      "title": "Monthly Equipment Inspection",
      "planTitle": "Annual Maintenance Plan",
      "assetId": "PUMP-001",
      "category": "Preventive",
      "frequency": "Monthly",
      "tbCb": 1,
      "sequence": 10,
      "status": "APPROVED"
    },
    {
      "taskId": 2,
      "title": "Quarterly Safety Check",
      "planTitle": "Safety Plan Q1",
      "assetId": "VALVE-042",
      "category": "Safety",
      "frequency": "Quarterly",
      "tbCb": 0,
      "sequence": 5,
      "status": "PENDING"
    }
  ],
  "totalCount": 150,
  "pageNumber": 1,
  "pageSize": 25
}
```

#### Response Fields

| Field | Type | Description |
|-------|------|-------------|
| `tasks` | Array | Array of task objects |
| `tasks[].taskId` | number | Unique task identifier |
| `tasks[].title` | string | Task title/name |
| `tasks[].planTitle` | string | Associated plan title |
| `tasks[].assetId` | string | Equipment/asset identifier |
| `tasks[].category` | string | Task category |
| `tasks[].frequency` | string | How often task occurs |
| `tasks[].tbCb` | number | Time-based (1) or Condition-based (0) |
| `tasks[].sequence` | number | Task sequence number |
| `tasks[].status` | string | Task status: "APPROVED", "PENDING", or "ARCHIVED" |
| `totalCount` | number | Total tasks for project (all pages) |
| `pageNumber` | number | Current page number |
| `pageSize` | number | Items per page |

#### Error Responses

**400 Bad Request:**
```json
{
  "type": "https://tools.ietf.org/html/rfc9110#section-15.5.1",
  "title": "One or more validation errors occurred.",
  "status": 400,
  "errors": {
    "projectId": ["The projectId field is required."]
  }
}
```

**401 Unauthorized:**
```json
{
  "message": "Invalid or expired token"
}
```

---

### 2. PUT /api/tasks/archive

**Purpose:** Archive selected tasks

**Status:** Already implemented in backend (ready to use)

#### Request

**Method:** PUT  
**URL:** `{API_BASE_URL}/api/tasks/archive`

**Headers:**
- `Authorization: Bearer {id_token}`
- `Content-Type: application/json`

**Body:**
```json
{
  "taskIds": [1, 2, 3],
  "email": "user@example.com"
}
```

**Body Fields:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `taskIds` | number[] | Yes | Array of task IDs to archive |
| `email` | string | Yes | Email of user performing action (for audit) |

**Example Request:**
```
PUT https://localhost:56802/api/tasks/archive
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
Content-Type: application/json

{
  "taskIds": [15, 23, 47],
  "email": "alex.rodriguez@example.com"
}
```

#### Response (200 OK)

```json
{
  "success": true,
  "tasksAffected": 3,
  "message": "3 task(s) archived successfully"
}
```

**Response Fields:**

| Field | Type | Description |
|-------|------|-------------|
| `success` | boolean | Whether operation succeeded |
| `tasksAffected` | number | Number of tasks successfully archived |
| `message` | string | Human-readable success message |

#### Error Responses

**400 Bad Request - Missing Email:**
```json
{
  "type": "https://tools.ietf.org/html/rfc9110#section-15.5.1",
  "title": "One or more validation errors occurred.",
  "status": 400,
  "errors": {
    "Email": ["The Email field is required."]
  }
}
```

**400 Bad Request - Empty Task IDs:**
```json
{
  "type": "https://tools.ietf.org/html/rfc9110#section-15.5.1",
  "title": "One or more validation errors occurred.",
  "status": 400,
  "errors": {
    "taskIds": ["At least one task ID is required."]
  }
}
```

**401 Unauthorized:**
```json
{
  "message": "Invalid or expired token"
}
```

**404 Not Found:**
```json
{
  "message": "One or more tasks not found"
}
```

**500 Internal Server Error:**
```json
{
  "message": "An error occurred while archiving tasks"
}
```

---

## Frontend Implementation Notes

### Fetching Tasks

```typescript
// Example implementation in taskService.ts
export const fetchTasksByProject = async (
  projectId: number,
  pageNumber: number = 1,
  pageSize: number = 25
): Promise<Task[]> => {
  const apiBaseUrl = getApiBaseUrl();
  const url = `${apiBaseUrl}/api/tasks/tasksbyproject?projectId=${projectId}&pageNumber=${pageNumber}&pageSize=${pageSize}`;
  
  const response = await makeAuthenticatedRequest(url);
  
  if (!response.ok) {
    throw new Error(`HTTP error! status: ${response.status}`);
  }
  
  const data = await response.json();
  
  // Filter out archived tasks (only show APPROVED and PENDING)
  const visibleTasks = data.tasks.filter(
    (task: any) => task.status !== "ARCHIVED"
  );
  
  // Transform to frontend Task type
  return visibleTasks.map((task: any) => ({
    taskId: task.taskId,
    taskName: task.title,
    status: task.status,
    // ... map other fields
  }));
};
```

### Archiving Tasks

```typescript
// Example implementation in taskService.ts
export const archiveTasks = async (
  taskIds: number[],
  email: string
): Promise<ArchiveResponse> => {
  const apiBaseUrl = getApiBaseUrl();
  const url = `${apiBaseUrl}/api/tasks/archive`;
  
  const response = await makeAuthenticatedRequest(url, {
    method: "PUT",
    body: JSON.stringify({ taskIds, email }),
  });
  
  if (!response.ok) {
    throw new Error(`HTTP error! status: ${response.status}`);
  }
  
  return await response.json();
};
```

### Getting User Email from JWT

```typescript
// Example utility function
export const getUserEmailFromToken = (): string | null => {
  const idToken = localStorage.getItem("id_token");
  if (!idToken) return null;
  
  try {
    // JWT has 3 parts: header.payload.signature
    const base64Url = idToken.split(".")[1];
    const base64 = base64Url.replace(/-/g, "+").replace(/_/g, "/");
    const jsonPayload = decodeURIComponent(
      atob(base64)
        .split("")
        .map((c) => "%" + ("00" + c.charCodeAt(0).toString(16)).slice(-2))
        .join("")
    );
    
    const payload = JSON.parse(jsonPayload);
    return payload.email || payload.sub || null;
  } catch (error) {
    console.error("Error decoding JWT:", error);
    return null;
  }
};
```

---

## Type Definitions

### Task Interface (Frontend)

**Location:** `types/tasks.types.ts`

```typescript
import { TaskStatus } from './taskstatus.enum';

export interface Task {
  taskId: number;
  taskName: string;          // Maps to API "title"
  status: TaskStatus;        // "APPROVED" | "PENDING" | "ARCHIVED"
  planTitle?: string;
  assetId?: string;
  category?: string;
  frequency?: string;
  tbCb?: number;
  sequence?: number;
}

export interface ArchiveResponse {
  success: boolean;
  tasksAffected: number;
  message: string;
}

export interface TasksByProjectResponse {
  tasks: Task[];
  totalCount: number;
  pageNumber: number;
  pageSize: number;
}
```

### TaskStatus Enum

**Location:** `types/taskstatus.enum.ts`

```typescript
export enum TaskStatus {
  APPROVED = "APPROVED",
  PENDING = "PENDING",
  ARCHIVED = "ARCHIVED",
}
```

---

## API Authentication

### JWT Token Storage
- **Key:** `id_token` (in localStorage)
- **Format:** Bearer token
- **Usage:** Automatically added by `makeAuthenticatedRequest()`

### Token Expiration
- If API returns 401, token is likely expired
- User should be redirected to login
- Handled by existing authentication utilities

---

**Last Updated:** 2025-11-07  
**Status:** Volatile - Specific to Task 471259  
**Usage:** Include when working on Task 471259 only

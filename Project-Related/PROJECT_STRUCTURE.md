# Project Structure - mfg-asset-strategy

## Root Directory

```
mfg-asset-strategy/
├── .devcontainer/          # Dev container configuration
├── .github/                # GitHub Actions workflows
├── db/                     # Database migrations (Flyway)
├── docs/                   # Project documentation
├── playwright.mcp/         # Playwright tests (MCP integration)
├── src/                    # Source code (backend + frontend)
├── terraform/              # Infrastructure as Code
└── wiki/                   # Project wiki
```

---

## Backend Structure

**Path:** `src/mfg-asset-strategy-api/`

```
mfg-asset-strategy-api/
├── MFG.ASAP.Data/                      # Data layer
│   ├── Entities/                       # Database entities (EF Core models)
│   ├── Enums/                          # Enums (ProjectStatus, SystemType, TaskStatus)
│   ├── Repositories/                   # Repository pattern implementation
│   │   ├── Project/
│   │   ├── Task/
│   │   ├── PlatformPlan/
│   │   ├── RecommendedSparePart/
│   │   └── User/
│   ├── ResponseObjects/                # API response DTOs
│   └── AsapDbContext.cs                # EF Core DbContext
│
├── MFG.ASAP.Logic/                     # Business logic layer
│   └── Services/                       # Service layer
│       ├── Project/
│       ├── Task/
│       ├── PlatformPlan/
│       ├── RecommendedSparePart/
│       └── User/
│
├── MFG.ASAP.WebApi/                    # Web API layer
│   ├── Controllers/                    # API Controllers
│   │   ├── ProjectController.cs
│   │   ├── TaskController.cs
│   │   ├── PlatformPlanController.cs
│   │   ├── RecommendedSparePartController.cs
│   │   └── UserController.cs
│   ├── RequestModels/                  # API request DTOs
│   ├── Extensions/                     # Middleware and extensions
│   ├── Models/                         # Other models
│   └── Startup.cs                      # Application startup configuration
│
├── MFG.ASAP.Logic.UnitTests/          # Unit tests
├── MFG.ASAP.Logic.IntegrationTests/   # Integration tests
└── MFG.ASAP.WebApi.IntegrationTests/  # Web API integration tests
```

---

## Frontend Structure

**Path:** `src/mfg-asset-strategy-ui/`

```
mfg-asset-strategy-ui/
├── environments/                       # Environment configurations
│   ├── local.env
│   ├── dev.env
│   ├── qa.env
│   ├── uat.env
│   └── prod.env
│
├── features/                           # Feature modules
│   ├── auth/                           # Authentication utilities
│   │   └── auth-util.ts
│   └── role/                           # Role-based utilities
│       └── role-util.ts
│
├── src/                                # Main source code
│   ├── components/                     # React components
│   │   ├── dashboard/                  # Dashboard page components
│   │   │   ├── Dashboard.tsx
│   │   │   ├── ProjectProgressWidget.tsx
│   │   │   ├── ProjectsBySystemWidget.tsx
│   │   │   ├── ProjectsNeedingAttention.tsx
│   │   │   └── RecentProjects.tsx
│   │   ├── reliability/                # Reliability page components
│   │   │   ├── Reliability.tsx
│   │   │   └── Reliability.css
│   │   ├── reusable/                   # Reusable components
│   │   │   ├── ReusableDataWidget.tsx
│   │   │   ├── ReusableNavigationButton.tsx
│   │   │   └── ReusablePageHeader.tsx
│   │   ├── Projects/                   # Projects page (placeholder)
│   │   ├── SpareParts/                 # Spare Parts page (placeholder)
│   │   ├── UserAlertsPage/             # Alerts page (placeholder)
│   │   ├── styles/                     # Shared styles
│   │   ├── Login.tsx                   # Login component
│   │   ├── NavigationBar.tsx           # Navigation bar
│   │   └── ProtectedRoute.tsx          # Route protection HOC
│   │
│   ├── data/                           # Sample/mock data
│   │   └── sampleProjectsNeedingAttention.ts
│   │
│   ├── utils/                          # Utility functions
│   │   └── api.ts                      # API helper functions
│   │
│   ├── assets/                         # Static assets
│   │   ├── fonts/                      # Custom fonts
│   │   └── [various SVG icons]
│   │
│   ├── App.tsx                         # Main App component
│   ├── index.tsx                       # Entry point
│   ├── index.html                      # HTML template
│   └── styles.css                      # Global styles
│
├── types/                              # TypeScript type definitions
│   ├── auth.types.ts
│   ├── facilities.types.ts
│   ├── projects.types.ts
│   ├── projectsbystatus.types.ts
│   ├── projectstatus.enum.ts
│   ├── systems.types.ts
│   ├── tasks.types.ts
│   ├── taskstatus.enum.ts
│   └── [other type definitions]
│
├── package.json                        # npm dependencies
├── tsconfig.json                       # TypeScript configuration
└── webpack.config.js                   # Webpack configuration
```

---

## Key Directories Explained

### Backend

| Directory | Purpose |
|-----------|---------|
| `Entities/` | EF Core database models (tables) |
| `Enums/` | Shared enums (TaskStatus, SystemType, etc.) |
| `Repositories/` | Data access layer (queries, CRUD) |
| `Services/` | Business logic layer |
| `Controllers/` | REST API endpoints |
| `ResponseObjects/` | DTOs for API responses |
| `RequestModels/` | DTOs for API requests |

### Frontend

| Directory | Purpose |
|-----------|---------|
| `components/` | React components organized by page/feature |
| `types/` | TypeScript interfaces and type definitions |
| `utils/` | Helper functions (API calls, etc.) |
| `features/` | Feature-specific utilities (auth, roles) |
| `assets/` | Static files (images, fonts, icons) |
| `environments/` | Environment-specific configuration |

---

## File Naming Conventions

### Backend (C#)
- **Components:** PascalCase (e.g., `ProjectController.cs`)
- **Interfaces:** Start with `I` (e.g., `IProjectRepository.cs`)
- **Tests:** End with `Tests` (e.g., `ProjectServiceTests.cs`)

### Frontend (React/TypeScript)
- **Components:** PascalCase (e.g., `Reliability.tsx`)
- **CSS Files:** Same name as component (e.g., `Reliability.css`)
- **Types:** lowercase with `.types.ts` suffix (e.g., `tasks.types.ts`)
- **Enums:** lowercase with `.enum.ts` suffix (e.g., `taskstatus.enum.ts`)
- **Utilities:** camelCase (e.g., `auth-util.ts`)

---

## Import Path Aliases (TypeScript)

Configured in `tsconfig.json`:

```typescript
"paths": {
  "@/*": ["src/*"],
  "@features/*": ["features/*"],
  "types/*": ["types/*"]
}
```

**Example usage:**
```typescript
import { Task } from "types/tasks.types";
import { isAsapUser } from "@features/role/role-util";
```

---

**Last Updated:** 2025-11-07  
**Status:** Semi-Stable - Changes when project structure evolves  
**Usage:** Reference when creating new files or understanding project organization

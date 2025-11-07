# Technology Stack - mfg-asset-strategy

## Project Overview
- **Project Name:** mfg-asset-strategy
- **Type:** Full-stack web application
- **Backend:** .NET 8 Web API
- **Frontend:** React with TypeScript

---

## Backend Stack

### Framework
- **.NET:** 8.0
- **Language:** C# 12

### Database
- **Primary:** PostgreSQL
- **ORM:** Entity Framework Core
- **Migrations:** Flyway (versioned migrations in `db/` directory)

### API Documentation
- **Swagger/OpenAPI:** Enabled for development

---

## Frontend Stack

### Core Framework
- **React:** 19.1.1
- **TypeScript:** 5.3.3
- **React Router DOM:** 7.8.1

### Build Tools
- **Webpack:** 5.101.3
- **Webpack Dev Server:** 5.2.2
- **TypeScript Loader:** ts-loader 9.5.2

### UI Component Libraries
- **Kendo React Grid:** 12.0.1
- **Kendo React Buttons:** 12.0.1
- **Kendo React Dropdowns:** 12.0.1
- **Kendo React Data Tools:** 12.0.1
- **Kendo React Charts:** 12.0.1
- **Kendo React Intl:** 12.0.1
- **Kendo Theme Default:** 12.0.1

**Note:** Kendo React requires a license key (configured in project)

### Styling
- **CSS:** Native CSS modules per component
- **Theme:** Kendo Theme Default
- **Fonts:** Barlow Semi Condensed (included in assets)

---

## State Management

- **Approach:** React native hooks (useState, useEffect)
- **No external libraries:** No Redux, Zustand, or other state management libraries
- **Rationale:** Simplicity for current project scope

---

## Authentication

- **Method:** JWT (JSON Web Tokens)
- **Storage:** localStorage
- **Token Types:**
  - `id_token`: Used for API authentication (in Authorization header)
  - `access_token`: OAuth/OIDC flow

---

## API Communication

### Pattern
- **Helper Function:** `makeAuthenticatedRequest()` in `src/utils/api.ts`
- **Authentication:** Bearer token from localStorage
- **Base URL:** Environment variable `API_BASE_URL`

### Example
```typescript
const response = await makeAuthenticatedRequest(
  `${apiBaseUrl}/API/endpoint`,
  { method: "GET" }
);
```

---

## Development Dependencies

### Code Quality
- **Linters:** ESLint
- **Formatters:** Prettier

### Testing
- **Framework:** (To be implemented)
- **Current Status:** No automated tests in frontend yet

---

## Infrastructure

### Deployment
- **Backend:** AWS Lambda (via Terraform)
- **Frontend:** AWS CloudFront (static hosting)
- **Database:** AWS RDS PostgreSQL

### CI/CD
- **Platform:** GitHub Actions
- **Workflows:** Defined in `.github/workflows/`

---

## Package Management

### Backend
- **Manager:** NuGet (.NET packages)
- **Lock File:** `packages.lock.json`

### Frontend
- **Manager:** npm
- **Lock File:** `package-lock.json`
- **Registry:** Default npm registry

---

## Browser Support

### Target Browsers
- Modern browsers (Chrome, Firefox, Safari, Edge)
- **ES Version:** ES2020
- **No IE11 support**

---

**Last Updated:** 2025-11-07  
**Status:** Semi-Stable - Changes when project upgrades dependencies  
**Usage:** Include when working on mfg-asset-strategy project (any task)

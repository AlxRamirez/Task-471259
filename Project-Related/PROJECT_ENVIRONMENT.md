# Project Environment - mfg-asset-strategy

## Repository Information

### Repository Details
- **Repository Name:** `mfg-asset-strategy`
- **Repository full Name ssl:** `git@github.com:Georgia-Pacific/mfg-asset-strategy.git`
- **Primary Branch:** `main`
- **Development Branch:** `wip/sprint-7`
- **GitHub:** (Company private repository)

### Local Paths
- **Windows:** `C:\Users\alexr24\Documents\LPL\ASAP\mfg-asset-strategy\`
- **WSL (optional):** `/mnt/c/Users/alexr24/Documents/LPL/ASAP/mfg-asset-strategy/`

---

## Current Sprint and Branches

### Active Sprint
- **Sprint:** Sprint 6/7 - Reliability Page Development
- **Base Branch:** `wip/sprint-7`

### Feature Branches Pattern
- **Format:** `feature/[task-number]-[description]`
- **Example:** `feature/471259-archive-tasks-ui`

### Backend API Branches
- **Format:** `feature/[task-number]-api-[description]`
- **Example:** `feature/478150-api-task-table`

---

## Development URLs

### Backend (.NET API)

| Environment | URL | Notes |
|-------------|-----|-------|
| Local HTTPS | https://localhost:56802 | Primary development URL |
| Local HTTP | http://localhost:56803 | Alternative (not typically used) |
| Swagger | https://localhost:56802/swagger | API documentation |
| Base API Path | https://localhost:56802/API | All endpoints under /API |

### Frontend (React)

| Environment | URL | Notes |
|-------------|-----|-------|
| Dev Server | http://localhost:3000 | Hot reload enabled |
| Build Output | `dist/` directory | Webpack output |

---

## Database Configuration

### PostgreSQL

#### Remote Database (Team Shared)
- **Host:** (AWS RDS - configured per environment)
- **Purpose:** Primary development and testing database
- **Access:** Via connection string in `appsettings.Development.json`

#### Local Database (Docker)
- **Host:** `localhost`
- **Port:** `5432`
- **Container:** Defined in `docker-compose.yml`
- **Purpose:** Local development/testing when offline or for isolated work

#### pgAdmin (Web UI)
- **URL:** http://localhost:5050
- **Purpose:** Database administration and query tool
- **Container:** Runs alongside PostgreSQL in Docker

---

## Environment Files

### Backend Configuration
- **Primary:** `appsettings.json` (checked into git)
- **Development:** `appsettings.Development.json` (local only, NOT in git)
- **Location:** `src/mfg-asset-strategy-api/MFG.ASAP.WebApi/`

**Note:** `appsettings.Development.json` contains local database connection strings and secrets

### Frontend Configuration
- **Location:** `src/mfg-asset-strategy-ui/environments/`
- **Files:**
  - `local.env` - Local development
  - `dev.env` - DEV environment
  - `qa.env` - QA environment
  - `uat.env` - UAT environment
  - `prod.env` - Production environment
  - `shared.env` - Shared variables

**Key Variables:**
- `API_BASE_URL` - Backend API base URL

---

## NPM Scripts (Frontend)

### Development
```bash
npm run dev          # Start dev server
npm start            # Alias for dev
```

### Build
```bash
npm run build        # Production build
npm run build:local  # Local build
npm run build:dev    # DEV environment build
npm run build:qa     # QA environment build
npm run build:uat    # UAT environment build
npm run build:prod   # Production build
```

### Serve
```bash
npm run serve        # Serve built files (from dist/)
```

---

## .NET Commands (Backend)

### Build and Run
```bash
dotnet build                          # Build solution
dotnet run                            # Run API
dotnet watch run                      # Run with hot reload
```

### Database Migrations
```bash
# Migrations are handled via Flyway, not EF Core migrations
# See db/mfg-asset-strategy/asap/public/ for SQL scripts
```

---

## Docker Compose (Local)

### Services
- **PostgreSQL:** Database server
- **pgAdmin:** Database administration UI

### Commands
```bash
docker-compose up -d           # Start services
docker-compose down            # Stop services
docker-compose logs -f         # View logs
```

**Note:** `docker-compose.yml` location (check project root or `db/` directory)

---

## API Base URLs by Environment

| Environment | API Base URL | Notes |
|-------------|--------------|-------|
| Local | https://localhost:56802 | Development machine |
| DEV | (AWS URL) | Development environment |
| QA | (AWS URL) | Quality assurance |
| UAT | (AWS URL) | User acceptance testing |
| PROD | (AWS URL) | Production |

**Configuration:** Set via `API_BASE_URL` environment variable in frontend

---

## Authentication Configuration

### JWT Tokens
- **Storage:** localStorage
- **Key Names:**
  - `id_token` - Used for API authentication
  - `access_token` - OAuth/OIDC access token
  - `refresh_token` - Token refresh (if applicable)

### Token Usage
- **Authorization Header:** `Bearer {id_token}`
- **Handled by:** `makeAuthenticatedRequest()` in `src/utils/api.ts`

---

## Deployment

### Backend
- **Platform:** AWS Lambda
- **Provisioning:** Terraform (see `terraform/` directory)
- **CI/CD:** GitHub Actions

### Frontend
- **Platform:** AWS CloudFront + S3
- **Provisioning:** Terraform
- **CI/CD:** GitHub Actions

---

## Key Project Contacts (Mentioned in Logs)

- **Thomas:** Team member (backend/database work)
- **Reference:** Can verify data model decisions and backend logic

---

**Last Updated:** 2025-11-07  
**Status:** Semi-Stable - Changes when project infrastructure changes  
**Usage:** Include when working on mfg-asset-strategy project (any task)

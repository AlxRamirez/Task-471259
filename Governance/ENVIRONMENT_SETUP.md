# Development Environment Setup

## Overview

Alex uses a **"Windows-First"** development approach with repository clones in the Windows filesystem, optimized for .NET development while maintaining compatibility with Linux containers for testing.

---

## Base Directory Structure

### Standard Location
- **Base Path:** `C:\Users\alexr24\Documents\LPL\`
- **Project Pattern:** `C:\Users\alexr24\Documents\LPL\[projectFolder]\[repo-name]\`
- **Access from WSL (optional):** `/mnt/c/Users/alexr24/Documents/LPL/[projectFolder]/[repo-name]/`

### Single Clone Strategy
- One repository clone per project in Windows filesystem
- Rationale: Single source of truth, avoids sync issues between multiple clones
- All development happens in Windows; Linux containers used only for testing

---

## Development Tools

### Company Standards
- **Backend Language:** C# with .NET
- **Frontend Stack:** React with TypeScript
- **Version Control:** Git
- These are company standards and do NOT change between projects

### IDEs and Editors
- **Backend (C# / .NET):** Visual Studio 2022 (Windows)
  - Terminal: PowerShell (integrated)
  - Used for: Building, debugging, running .NET projects
  - Opens backend project directories
  
- **Frontend (React / TypeScript):** VS Code (Windows)
  - Terminal: PowerShell (integrated)
  - Used for: Editing React/TypeScript code, running npm commands
  - Opens frontend project directories

### Version Control
- **Git:** Can be executed from Windows (PowerShell/Git Bash) or WSL (Ubuntu)
  - WSL is **optional** - use it if you prefer consistent behavior and avoid line-ending issues
  - Windows git works fine for most operations
  - Choose based on personal preference

### Database Client
- **DBeaver:** For queries and database verification
- **Note:** Specific database system (PostgreSQL, SQL Server, etc.) varies by project

### Containers
- **Docker Desktop:** For local containers and testing

---

## Common Workflows

### Backend Development (.NET)

**In Visual Studio 2022 (PowerShell terminal):**
```powershell
# Navigate to backend (example path)
cd C:\Users\alexr24\Documents\LPL\[projectFolder]\[repo-name]\[backend-directory]\

# Build
dotnet build

# Run (or press F5 for debug)
dotnet run

# Typical backend runs on HTTPS (port varies by project)
```

**For Git operations (Windows or WSL):**
```bash
# Windows PowerShell:
cd C:\Users\alexr24\Documents\LPL\[projectFolder]\[repo-name]
git status
git add [files]
git commit -m "fix: your message"
git push origin [branch-name]

# OR WSL (optional):
cd /mnt/c/Users/alexr24/Documents/LPL/[projectFolder]/[repo-name]
git status
# ... same git commands
```

---

### Frontend Development (React)

**In VS Code (PowerShell terminal):**
```powershell
# Navigate to frontend (example path)
cd C:\Users\alexr24\Documents\LPL\[projectFolder]\[repo-name]\[frontend-directory]\

# Install dependencies
npm install

# Development server
npm run dev
# or
npm start

# Build (commands may vary by project)
npm run build
```

**For Git operations (Windows or WSL):**
```bash
# Same pattern as backend
cd C:\Users\alexr24\Documents\LPL\[projectFolder]\[repo-name]
git status
git add [files]
git commit -m "feat: your message"
git push origin [branch-name]
```

---

## Branch Management

### Typical Branch Workflow

**Create new feature branch:**
```bash
cd C:\Users\alexr24\Documents\LPL\[projectFolder]\[repo-name]
git fetch origin
git checkout main
git pull origin main
git checkout -b feature/task-number-description
```

**Switch branches:**
```bash
git fetch origin
git checkout feature/existing-branch
git pull origin feature/existing-branch
```

**Sync with main:**
```bash
git checkout main
git pull origin main
git checkout feature/your-branch
git merge main
# Resolve conflicts if any
git push origin feature/your-branch
```

---

## Development URLs

### Typical Local Services
These are common patterns, but **specific ports vary by project**:

| Service Type | Typical URL Pattern | Notes |
|--------------|---------------------|-------|
| Frontend Dev Server | http://localhost:3000 | Port may vary |
| Backend API (HTTPS) | https://localhost:5xxxx | Check project config |
| Backend API (HTTP) | http://localhost:5xxxx | Check project config |
| Backend Swagger | https://localhost:5xxxx/swagger | If enabled |
| Database | localhost:5432 | Default PostgreSQL |
| pgAdmin | http://localhost:5050 | If using Docker |

**Note:** Always check the project-specific configuration for actual URLs and ports.

---

## Shared Interaction Standards

| Topic | Workflow Expectation | Source |
|-------|----------------------|--------|
| **Language split** | Conversaciones en español; menús/comandos en inglés | Developer Profile · Interaction Quick Reference · "Language split" |
| **Command cadence** | Ejecutar un comando a la vez y esperar la salida antes de continuar | IA Interaction Rules · Operational Rule 9 |
| **Complete files** | Entregar archivos completos para copiar y pegar | IA Interaction Rules · Operational Rule 2 |
| **Verification** | Confirmar resultados críticos con fuentes oficiales | Developer Profile · Interaction Quick Reference · "Verification mindset" |
| **Tone & humor** | Directo, colegial, sarcasmo afilado bienvenido | IA Interaction Rules · Operational Rule 3 |

---

## Testing Strategy

### Local Testing
- Backend: Visual Studio 2022 debugger
- Frontend: Chrome DevTools with dev server
- Database: Database client (DBeaver) or specific project tools

### Linux Container Testing
- Build containers to verify Linux compatibility
- Ensures parity between development (Windows) and deployment (typically Linux)

---

**Last Updated:** 2025-11-07  
**Status:** Stable - Changes only when personal environment setup changes  
**Usage:** Include in ALL AI sessions as base environment context  
**Note:** Project-specific details (repo name, structure, URLs) should be in PROJECT_ENVIRONMENT.md
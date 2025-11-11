# Development Environment Setup

## Overview

Alex uses a **"Windows-First"** development approach with repository clones in the Windows filesystem, optimized for .NET development while maintaining compatibility with Linux containers for testing.

---

## Base Directory Structure

### Standard Location

| Elemento | Ruta |
|----------|------|
| Base Path | `C:\Users\alexr24\Documents\LPL\` |
| Patrón de proyecto | `C:\Users\alexr24\Documents\LPL\[projectFolder]\[repo-name]\` |
| Acceso WSL (opcional) | `/mnt/c/Users/alexr24/Documents/LPL/[projectFolder]/[repo-name]/` |

### Single Clone Strategy
- Un repo por proyecto en Windows como fuente única.
- Evita problemas de sincronización entre clones.
- Linux/WSL solo para pruebas y compatibilidad.

---

## Development Tools

| Category | Tooling | Usage Notes |
|----------|---------|-------------|
| **Company stack** | C# + .NET backend · React + TypeScript frontend | Base combo for todos los proyectos |
| **IDEs** | Visual Studio 2022 (backend) · VS Code (frontend) | Ejecutar terminal integrada PowerShell |
| **Version control** | Git (PowerShell o Git Bash). WSL opcional | Usa la ruta Windows como fuente única; WSL solo para compatibilidad |
| **Database** | DBeaver | Cliente genérico, apunta a la base definida por proyecto |
| **Containers** | Docker Desktop | Levantar ambientes locales y validar paridad Linux |

---

## Common Workflows

### Backend Development (.NET)

**En Visual Studio 2022 (PowerShell integrada):**
```powershell
# Navigate to backend (example path)
cd C:\Users\alexr24\Documents\LPL\[projectFolder]\[repo-name]\[backend-directory]\

# Build
dotnet build

# Run (or press F5 for debug)
dotnet run

# Typical backend runs on HTTPS (port varies by project)
```

Consulta el flujo Git consolidado más abajo.

---

### Frontend Development (React)

**En VS Code (PowerShell integrada):**
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

Consulta el flujo Git consolidado más abajo.

---

### Git Workflow (Windows base, WSL opcional)

| Acción | Comando | Notas |
|--------|---------|-------|
| Ir al repo | `cd C:\Users\alexr24\Documents\LPL\[projectFolder]\[repo-name]` | En WSL usa `/mnt/c/...` |
| Revisar estado | `git status` | Igual en PowerShell o WSL |
| Publicar cambios | `git add <files>` → `git commit -m "<type>: mensaje"` → `git push origin <branch>` | Mantén un solo origen (Windows) |
| Sincronizar main | `git fetch origin` → `git checkout main` → `git pull origin main` | Ejecuta antes de crear ramas |
| Crear rama | `git checkout -b feature/<ticket>-descripcion` | Mismo patrón para frontend/backend |
| Actualizar rama | `git checkout feature/<ticket>` → `git pull origin feature/<ticket>` | Resuelve conflictos en Windows |

WSL solo replica rutas (`/mnt/c/...`). Úsalo cuando necesites herramientas Linux o normalizar finales de línea.

---

## Branch Management

El flujo completo vive en la tabla anterior para evitar duplicidad con guías de proyecto.

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

## Interaction Reminders

Las normas de conversación viven en `Governance/IA_INTERACTION_RULES.md`. Usa este documento solo para logística del entorno.

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
**Note:** Project-specific details (repo name, structure, URLs) should be in `Project-Related/PROJECT_ENVIRONMENT.md`.

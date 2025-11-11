# Project Structure Cheatsheet · mfg-asset-strategy

## Vista rápida de carpetas
```
mfg-asset-strategy/
├── src/                    # Backend y frontend
├── db/                     # Migraciones Flyway
├── docs/                   # Documentación extendida
├── terraform/              # Infraestructura
├── playwright.mcp/         # Pruebas E2E
└── wiki/                   # Documentos internos
```

## Backend (`src/mfg-asset-strategy-api/`)
- `MFG.ASAP.Data/`
  - `Entities/` · Modelos EF Core
  - `Repositories/` · Acceso a datos (`Project/`, `Task/`, ...)
  - `ResponseObjects/` · DTOs de salida
  - `AsapDbContext.cs`
- `MFG.ASAP.Logic/Services/` · Capa de negocio
- `MFG.ASAP.WebApi/`
  - `Controllers/` (`TaskController.cs`, etc.)
  - `RequestModels/`, `Extensions/`, `Startup.cs`
- Tests: `MFG.ASAP.Logic.UnitTests/`, `MFG.ASAP.Logic.IntegrationTests/`, `MFG.ASAP.WebApi.IntegrationTests/`

✅ **Checklist backend**
- [ ] Nuevas entidades en `Entities/` + migración Flyway correspondiente.
- [ ] Lógica en `Services/` y acceso a datos en `Repositories/`.
- [ ] Exponer endpoints vía `Controllers/` + DTO en `RequestModels/` o `ResponseObjects/`.

## Frontend (`src/mfg-asset-strategy-ui/`)
- `src/components/`
  - `reliability/` (`Reliability.tsx`, `.css`)
  - `dashboard/`, `reusable/`, etc.
- `features/` · utilidades (auth/roles)
- `types/` · definiciones TypeScript (`tasks.types.ts`, `taskstatus.enum.ts`)
- `utils/` · helpers (`api.ts`, `jwtUtils.ts`)
- `environments/` · `.env` por entorno
- Raíz: `App.tsx`, `index.tsx`, `styles.css`

✅ **Checklist frontend**
- [ ] Componentes bajo `components/<feature>/` + CSS homónimo.
- [ ] Tipos nuevos en `types/` (`<entity>.types.ts`).
- [ ] Helpers compartidos en `utils/`; reutiliza `makeAuthenticatedRequest`.
- [ ] Usa alias de `tsconfig.json` (`@/*`, `@features/*`, `types/*`).

## Naming Reminders
- Backend: PascalCase; interfaces `IExample`; pruebas terminan en `Tests`.
- Frontend: Componentes PascalCase, enums `*.enum.ts`, tipos `*.types.ts`, utilidades camelCase.

> Para mapas completos de carpetas revisa `docs/architecture/structure.md` o el wiki del proyecto.

**Actualizado:** 2025-11-07 · Mantén esta guía junto al IDE.

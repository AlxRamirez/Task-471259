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

| Directorio | Contenido clave | Nota rápida |
|------------|-----------------|-------------|
| `MFG.ASAP.Data/Entities/` | Modelos EF Core | Generan migraciones Flyway |
| `MFG.ASAP.Data/Repositories/` | Acceso a datos (`Project/`, `Task/`, …) | Mantener interfaces coherentes |
| `MFG.ASAP.Data/ResponseObjects/` | DTOs de salida | Usa naming consistente con Swagger |
| `MFG.ASAP.Logic/Services/` | Capa de negocio | Contiene orquestación principal |
| `MFG.ASAP.WebApi/Controllers/` | Endpoints (`TaskController.cs`, etc.) | Complementar con `RequestModels/` |
| Tests (`MFG.ASAP.Logic.*Tests/`) | Unit e integration tests | Revisa checklist maestro para cobertura |

## Frontend (`src/mfg-asset-strategy-ui/`)

| Directorio | Uso | Nota rápida |
|------------|-----|-------------|
| `src/components/` | Componentes UI (`reliability/`, `dashboard/`, etc.) | CSS gemelo por carpeta |
| `src/features/` | Utilidades y lógica transversal (auth/roles) | Evita duplicar hooks |
| `src/types/` | Tipos (`tasks.types.ts`, `taskstatus.enum.ts`) | Mantén enums en archivos dedicados |
| `src/utils/` | Helpers (`api.ts`, `jwtUtils.ts`) | Centraliza llamadas HTTP |
| `environments/` | Archivos `.env` por entorno | Sin credenciales en Git |
| Raíz (`App.tsx`, `index.tsx`) | Bootstrap de la app | Verifica rutas al actualizar |

## Naming Reminders
- Backend: PascalCase; interfaces `IExample`; pruebas terminan en `Tests`.
- Frontend: Componentes PascalCase, enums `*.enum.ts`, tipos `*.types.ts`, utilidades camelCase.

> Checklist de entrega consolidado en `Project-Related/TECH_STACK.md` · sección "Delivery Checklist".

> Para mapas completos de carpetas revisa `docs/architecture/structure.md` o el wiki del proyecto.

**Actualizado:** 2025-11-07 · Mantén esta guía junto al IDE.


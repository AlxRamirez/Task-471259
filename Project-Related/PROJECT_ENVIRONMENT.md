# Environment Quick Reference · mfg-asset-strategy

## Repo & Branching

| Elemento | Valor |
|----------|-------|
| Repo | `git@github.com:Georgia-Pacific/mfg-asset-strategy.git` |
| Base branch | `wip/sprint-7` |
| Convención de ramas | `feature/<task>-<descripcion>` (frontend) · `feature/<task>-api-<descripcion>` (backend) |
| Clone Windows | `C:\Users\alexr24\Documents\LPL\ASAP\mfg-asset-strategy\` |
| Clone WSL (espejo) | `/mnt/c/Users/alexr24/Documents/LPL/ASAP/mfg-asset-strategy/` |

## Runtime URLs
| Contexto | URL | Nota |
|----------|-----|------|
| API local HTTPS | https://localhost:56802 | Swagger en `/swagger` |
| API local HTTP | http://localhost:56803 | Alternativo |
| Frontend dev server | http://localhost:3000 | `npm run dev` |
| pgAdmin | http://localhost:5050 | Admin DB (Docker) |

## Configuration Files
- Backend: `src/mfg-asset-strategy-api/MFG.ASAP.WebApi/appsettings.json` + `appsettings.Development.json` (local, secreto).
- Frontend envs: `src/mfg-asset-strategy-ui/environments/*.env` (`local`, `dev`, `qa`, `uat`, `prod`, `shared`).
- Variable crítica: `API_BASE_URL`.

## Command Deck

| Contexto | Comandos clave | Nota |
|----------|----------------|------|
| Frontend | `npm run dev` · `npm run build[:env]` · `npm run serve` | `build:local/dev/qa/uat/prod` según ambiente |
| Backend | `dotnet build` · `dotnet run` · `dotnet watch run` | Ejecuta desde Visual Studio o PowerShell |
| Docker local | `docker-compose up -d` · `docker-compose down` · `docker-compose logs -f` | Migraciones Flyway en `db/mfg-asset-strategy/asap/public/` |

## Data & Auth
- PostgreSQL: RDS compartido (por defecto) o contenedor local `localhost:5432`.
- Tokens JWT en `localStorage` (`id_token`, `access_token`, `refresh_token`).
- `makeAuthenticatedRequest()` adjunta automáticamente `Authorization: Bearer {id_token}`.

## Deployment & Ops
- Backend: AWS Lambda (Terraform) · Frontend: CloudFront + S3.
- CI/CD: GitHub Actions (`.github/workflows/`).
- Contacto clave backend/DB: **Thomas**.

> Checklist de entrega y convenciones en `Project-Related/TECH_STACK.md` · sección "Delivery Checklist".

> Para topologías completas y credenciales, revisa `docs/environment/README.md` o el espacio interno de Infra.

**Actualizado:** 2025-11-07 · Mantén esta tarjeta abierta durante la puesta en marcha.


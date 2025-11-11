# Tech Stack One-Pager · mfg-asset-strategy

## Core
- **Tipo:** Aplicación full-stack (.NET 8 API + React 19.1 + TypeScript 5.3).
- **Base de datos:** PostgreSQL gestionado vía Flyway (`db/`).
- **Autenticación:** JWT (`id_token` en localStorage) consumido por `makeAuthenticatedRequest()`.

## Backend Toolkit
- [.NET 8](https://dotnet.microsoft.com/) · C# 12
- Entity Framework Core · DTOs en `ResponseObjects/`
- Swagger habilitado en DEV (`https://localhost:56802/swagger`)
- Despliegue: AWS Lambda (Terraform) · Paquetes NuGet (`packages.lock.json`)

✅ **Checklist backend**
- [ ] Documentar endpoints en Swagger + `docs/api/`.
- [ ] Añadir pruebas en `MFG.ASAP.Logic.*Tests` cuando se crea lógica.
- [ ] Actualizar scripts Flyway si cambia el esquema.

## Frontend Toolkit
- React 19.1 · React Router DOM 7.8
- Webpack 5 + ts-loader · npm (`package-lock.json`)
- UI: KendoReact (Grid/Buttons/Dropdowns/Data/Charts/Intl) v12 + tema Default
- Estilos CSS modulares por componente · Fuente Barlow Semi Condensed

✅ **Checklist frontend**
- [ ] Registrar componentes en `components/<feature>/` con CSS dedicado.
- [ ] Mantener tipados en `types/` y enums en `*.enum.ts`.
- [ ] Respetar licencia de KendoReact (ver `docs/licenses/kendo.md`).

## Calidad & Herramientas
- ESLint + Prettier (frontend)
- CI/CD: GitHub Actions (`.github/workflows/`)
- Infraestructura: Terraform (`terraform/`), AWS CloudFront + S3 para UI

> Revisa `wiki/Tech-Stack.md` para versiones históricas y decisiones arquitectónicas.

**Actualizado:** 2025-11-07 · Mantén este resumen sincronizado con upgrades.

# Code Standards Quick Reference · mfg-asset-strategy

## Frontend (React + TypeScript)
- [ ] Component files y funciones en **PascalCase** (`Reliability.tsx`).
- [ ] Variables/handlers en **camelCase** (`selectedTaskIds`, `handleArchive`).
- [ ] CSS en `kebab-case` con prefijo de componente (`.reliability-grid__toolbar`).
- [ ] Constantes reales en `UPPER_SNAKE_CASE`; enums `TaskStatus.APPROVED`.
- [ ] Mantén estructura: imports → tipos → componente → estado/hooks → helpers → JSX → export.
- [ ] Usa `makeAuthenticatedRequest()` y controla `response.ok` antes de parsear.

## Backend (C#)
- [ ] Clases/propiedades/métodos en **PascalCase**; interfaces prefijo `I`.
- [ ] Campos privados `_camelCase` con guion bajo inicial.
- [ ] Control de errores: captura específica (`NotFoundException`, `ValidationException`) antes de `Exception` genérica.
- [ ] Devuelve códigos HTTP apropiados (`Ok`, `BadRequest`, `NotFound`, `StatusCode(500)`).

## CSS & Styling
- [ ] Agrupa reglas por bloques (Layout → Typography → States → Responsive).
- [ ] Evita `!important` y anidamientos profundos.
- [ ] Coloca estilos junto al componente (`ComponentName.css`).

## State & Effects
- [ ] Declara `useState` por responsabilidad (`isLoading`, `selectedIds`).
- [ ] Usa `useEffect(() => { ... }, [])` para carga inicial y dependencias explícitas para recargas.
- [ ] Limpia suscripciones/intervalos en retorno de `useEffect`.

## Documentation & Comments
- [ ] Documenta *por qué* existe la lógica (no qué hace) y referencia tickets (`TODO: revisit per TASK-123`).
- [ ] Evita comentarios redundantes con el propio código.

## Git Commit Format
```
<type>: <short description>

[optional body]
[optional footer]
```
Tipos comunes: `feat`, `fix`, `refactor`, `style`, `docs`, `test`, `chore`.

## Testing Targets
- [ ] Backend: clases de prueba `SomethingTests` con método `Method_Scenario_Expected` (xUnit).
- [ ] Frontend: escenarios críticos cubiertos en `playwright.mcp/` (E2E) cuando UI cambia.

> Consulta la guía extendida en `wiki/Coding-Standards.md` para ejemplos completos y justificación.

**Actualizado:** 2025-11-07 · Usa esta lista como verificación previa a PRs.

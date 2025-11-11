# Task 471259 · Archive Action (Frontend)

## Snapshot
- **Sprint:** 6 · Reliability Page Development
- **Status:** ✅ Completed (Frontend + Backend)
- **Primary Goal:** Activar la acción **Archive** para filas seleccionadas en la tabla de tareas de Reliability, consumiendo el endpoint existente de backend.

## Key Outcomes
1. **Grid Modernization:** Sustitución de la tabla estática por `@progress/kendo-react-grid` con soporte para selección múltiple.
2. **User Flow Control:** Botón **Archive** habilitado/deshabilitado según selección y recuento visible.
3. **UX Safeguards:** Diálogo de confirmación previo, manejo de carga y notificaciones de éxito/error.
4. **State + Auth Utilities:** Gestión local (`useState`) de selección/carga y helper `getUserEmailFromToken()` para obtener el email del JWT.
5. **API Bridge:** Servicio `archiveTasks()` + `fetchTasksByProject()` encapsulan llamadas a `PUT /api/tasks/archive` y `GET /api/tasks/tasksbyproject`, filtrando tareas archivadas en la carga inicial.

## Source References
- Frontend container y UI principal: [`src/mfg-asset-strategy-ui/src/components/reliability/Reliability.tsx`](../src/mfg-asset-strategy-ui/src/components/reliability/Reliability.tsx)
- Estilos específicos del grid: [`src/mfg-asset-strategy-ui/src/components/reliability/Reliability.css`](../src/mfg-asset-strategy-ui/src/components/reliability/Reliability.css)
- Servicio de tareas: [`src/mfg-asset-strategy-ui/src/services/taskService.ts`](../src/mfg-asset-strategy-ui/src/services/taskService.ts)
- Utilidad JWT: [`src/mfg-asset-strategy-ui/src/utils/jwtUtils.ts`](../src/mfg-asset-strategy-ui/src/utils/jwtUtils.ts)
- Tipos compartidos: [`src/mfg-asset-strategy-ui/types/tasks.types.ts`](../src/mfg-asset-strategy-ui/types/tasks.types.ts)

> Para detalles completos de UI/UX, consulta el diseño en `docs/reliability/Archive-Flow.md` (wiki interno).

## Backend Dependency
- **Task relacionada:** 478150 (Backend).
- **Endpoint:** `PUT /api/tasks/archive` refactorizado a LINQ; dependencia crítica para finalizar la acción.
- **Cobertura adicional:** `GET /api/tasks/tasksbyproject` sirve como fuente de datos.

## Deployment & Follow-up
- Ramas publicadas: `feature/471259-archive-tasks-ui` (frontend) y `feature/478150-api-task-table` (backend).
- Próximo paso documentado: fusionar ambas en rama de pruebas `*-final-test` para validación E2E.
- Estado final: listo para QA automatizado (Richard).

**Actualizado:** 2025-11-10 · Referencia histórica consolidada en una página.

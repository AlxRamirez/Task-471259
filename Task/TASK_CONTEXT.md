# Task 471259 · Archive Action (Frontend)

<details open>
<summary><strong>Resumen</strong></summary>

| Campo | Valor |
|-------|-------|
| Sprint | 7 · Reliability Page Development |
| Status | ✅ Completado (frontend + backend) |
| Objetivo | Activar la acción **Archive** para las filas seleccionadas en el grid de Reliability, apoyándose en el endpoint existente. |

**Resultados clave**

1. Grid modernizado con `@progress/kendo-react-grid` y selección múltiple.
2. Botón **Archive** habilitado por selección + contador visible.
3. Diálogo de confirmación, estados de carga y toasts de éxito/error.
4. `useState` para selección/carga y `getUserEmailFromToken()` para firmar solicitudes.
5. Servicios `archiveTasks()` y `fetchTasksByProject()` consumen `PUT /api/tasks/archive` y `GET /api/tasks/tasksbyproject`, filtrando archivados.

**Archivos fuente**

| Pieza | Ruta |
|-------|------|
| Contenedor UI | `src/mfg-asset-strategy-ui/src/components/reliability/Reliability.tsx` |
| Estilos grid | `src/mfg-asset-strategy-ui/src/components/reliability/Reliability.css` |
| Servicio tareas | `src/mfg-asset-strategy-ui/src/services/taskService.ts` |
| Utilidad JWT | `src/mfg-asset-strategy-ui/src/utils/jwtUtils.ts` |
| Tipos | `src/mfg-asset-strategy-ui/types/tasks.types.ts` |

> Detalle UX adicional en `docs/reliability/Archive-Flow.md` (wiki interno).

</details>

<details>
<summary><strong>API</strong></summary>

| Endpoint | Método | Uso | Payload principal | Respuesta |
|----------|--------|-----|------------------|-----------|
| `/api/tasks/tasksbyproject` | GET | Carga inicial de tareas activas | `projectId` obligatorio (`pageNumber`/`pageSize` opcional) | `tasks` paginadas + `totalCount` |
| `/api/tasks/archive` | PUT | Archivado masivo desde el grid | `taskIds[]`, `email` del usuario | `{ success, tasksAffected, message }` |

**Requisitos de solicitud**
- Header `Authorization: Bearer {id_token}` gestionado por `makeAuthenticatedRequest()`.
- `Content-Type: application/json`.
- Errores comunes: `400` (datos faltantes), `401` (token inválido), `404` (IDs inexistentes), `500` (fallos generales).

**Dependencias backend**
- Task relacionada: 478150.
- Refactor LINQ en `PUT /api/tasks/archive` + datos de `GET /api/tasks/tasksbyproject`.

</details>

<details>
<summary><strong>Checklist</strong></summary>

- Confirmar `id_token` antes de disparar `archiveTasks`.
- Mostrar `tasksAffected` en la notificación de éxito.
- Manejar `response.ok` falso con messaging amigable y logging.
- Mantener selección coherente tras refrescar datos del grid.
- Fusionar ramas `feature/471259-archive-tasks-ui` y `feature/478150-api-task-table` en rama de pruebas `*-final-test` previo a QA (Richard).

</details>

**Actualizado:** 2025-11-10 · Referencia consolidada en una tarjeta.


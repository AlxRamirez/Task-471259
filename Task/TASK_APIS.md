# APIs Snapshot · Task 471259 Archive Action

## Objective
Habilitar la operación de archivado desde la UI de Reliability consumiendo los endpoints existentes y garantizando autenticación con `id_token`.

## Endpoint Matrix
| Endpoint | Método | Uso principal | Payload esencial | Respuesta clave |
|----------|--------|---------------|------------------|-----------------|
| `/api/tasks/tasksbyproject` | GET | Cargar tareas activas de un proyecto | `projectId` obligatorio; `pageNumber`/`pageSize` opcionales | Lista paginada (`tasks`, `totalCount`) filtrada en frontend para excluir `ARCHIVED` |
| `/api/tasks/archive` | PUT | Archivar tareas seleccionadas | `taskIds[]`, `email` del usuario | `{ success, tasksAffected, message }` |

> Para contratos completos revisa `docs/api/tasks-archive.md` en el repo principal o la especificación Swagger (`/swagger`).

## Request Essentials
- **Auth:** `Authorization: Bearer {id_token}` gestionado por `makeAuthenticatedRequest()`.
- **Headers:** `Content-Type: application/json`.
- **Errores relevantes:**
  - `400` por `projectId` faltante, `taskIds` vacío o `email` sin valor.
  - `401` por token inválido/expirado → redirigir a login.
  - `404` cuando alguna tarea no existe.
  - `500` para fallos no controlados (loggear + mostrar notificación).

## Frontend Integration Hooks
- **Carga inicial:** `fetchTasksByProject(projectId, pageNumber = 1, pageSize = 25)` → transforma respuesta y descarta `status === "ARCHIVED"`.
- **Acción Archive:** `archiveTasks(taskIds, email)` → confirma en modal antes de enviar.
- **Email del usuario:** `getUserEmailFromToken()` obtiene `email`/`sub` desde JWT almacenado en `localStorage`.

## Source Links
- Servicio y helpers: [`src/mfg-asset-strategy-ui/src/services/taskService.ts`](../src/mfg-asset-strategy-ui/src/services/taskService.ts)
- Utilidad JWT: [`src/mfg-asset-strategy-ui/src/utils/jwtUtils.ts`](../src/mfg-asset-strategy-ui/src/utils/jwtUtils.ts)
- Tipos de respuesta: [`src/mfg-asset-strategy-ui/types/tasks.types.ts`](../src/mfg-asset-strategy-ui/types/tasks.types.ts)

## Validation Checklist
- [ ] Confirmar que el usuario tiene `id_token` válido antes de disparar PUT.
- [ ] Mostrar recuento de `tasksAffected` en toast de éxito.
- [ ] Manejar `response.ok` falso con mensaje amigable y logging.
- [ ] Mantener selección consistente tras recargar datos.

**Actualizado:** 2025-11-07 · Referencia compacta (1 página).

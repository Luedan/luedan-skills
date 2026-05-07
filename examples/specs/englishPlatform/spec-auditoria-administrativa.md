# Spec: Auditoría administrativa

## Objetivo

Definir el módulo de trazabilidad de acciones administrativas, permitiendo registrar, consultar y filtrar eventos relevantes del panel admin para asegurar control operativo y reducir conflictos.

## Alcance

Incluye:

1. Eventos auditables.
2. Registro de acciones administrativas.
3. Consulta de logs desde panel admin.
4. Filtros básicos.
5. Datos mínimos de trazabilidad.

## Fuera de alcance

1. SIEM o seguridad enterprise.
2. Alertado en tiempo real.
3. Cumplimiento normativo avanzado.
4. Exportación avanzada de logs, salvo futura ampliación.

## Dependencias

1. `spec-modelo-datos-plataforma.md`
2. `spec-base-app-y-arquitectura.md`
3. `spec-panel-admin-codigos.md`
4. `spec-panel-admin-contenido.md`

## Actores y permisos

### admin
- puede consultar auditoría
- genera eventos auditables al ejecutar acciones críticas

### student
- no puede acceder al módulo

## Rutas / pantallas

1. `/admin/audit`

## Componentes UI

1. Tabla de logs.
2. Filtros por tipo de entidad.
3. Filtros por acción.
4. Filtros por administrador.
5. Vista resumida o detalle del evento.

## Server Actions / operaciones servidor

### 1. `writeAuditLog`
Responsabilidades:
- registrar acción administrativa relevante
- persistir entidad, acción, actor y metadata

### 2. `getAuditLogs`
Responsabilidades:
- listar eventos con filtros básicos

### 3. `getAuditLogDetail`
Responsabilidades:
- devolver detalle del evento si la UI lo requiere

## Modelo de datos afectado

1. `AuditLog`
2. `User` como actor administrativo

## Eventos auditables mínimos

1. creación de códigos
2. activación o desactivación de códigos
3. creación o edición de niveles
4. creación o edición de lecciones
5. cambio de estado de contenido
6. cambio de porcentaje mínimo por lección
7. acciones relevantes sobre usuarios si luego se habilitan

## Reglas de negocio

1. Toda acción administrativa crítica debe generar un log auditable.
2. El log debe incluir quién realizó la acción, sobre qué entidad y cuándo.
3. La auditoría debe ser solo accesible por administradores.
4. La lectura de auditoría debe facilitar trazabilidad operativa y resolución de conflictos.
5. El sistema no debe depender de la UI para generar el log; debe generarse en la capa servidor.

## Validaciones

1. solo admins pueden consultar auditoría
2. los eventos deben incluir `adminUserId`
3. los eventos deben incluir `entityType` y `actionType`
4. el timestamp debe registrarse al momento de la acción

## Estados y errores

1. log registrado
2. listado cargado
3. sin eventos disponibles
4. error al cargar auditoría
5. acceso denegado

## Criterios de aceptación

### CA-001 Registro automático
**Dado** un administrador autenticado  
**Cuando** ejecuta una acción crítica sobre códigos o contenido  
**Entonces** el sistema registra automáticamente un evento en auditoría.

### CA-002 Consulta restringida
**Dado** un usuario no administrador  
**Cuando** intenta acceder a `/admin/audit`  
**Entonces** el sistema bloquea el acceso.

### CA-003 Filtro funcional
**Dado** un administrador autenticado  
**Cuando** consulta la auditoría filtrando por tipo de entidad o acción  
**Entonces** el sistema devuelve solo los eventos que cumplan ese criterio.

### CA-004 Trazabilidad mínima
**Dado** un evento auditado  
**Cuando** se consulta en el panel  
**Entonces** debe indicar actor, acción, entidad y fecha/hora.

## Casos límite

1. Acción administrativa exitosa con fallo posterior al registrar log.
2. Gran volumen de logs.
3. Acción repetida varias veces por el mismo admin.
4. Entidad eliminada lógicamente después de haber sido auditada.

## Notas técnicas

1. El log debe escribirse desde Server Actions o servicios servidor, nunca desde cliente.
2. Se recomienda almacenar `detailJson` con metadata resumida útil.
3. La UI inicial puede usar tabla con filtros simples y paginación posterior.
4. La auditoría no debe bloquear el negocio por detalles cosméticos de la vista.

## Definición de terminado

Se considerará terminado este módulo cuando:

1. Estén definidos los eventos mínimos auditables.
2. El acceso y la lectura admin estén claros.
3. Un agente pueda implementar la trazabilidad administrativa sin ambigüedad crítica.

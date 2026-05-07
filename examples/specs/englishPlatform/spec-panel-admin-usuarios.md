# Spec: Panel admin de usuarios

## Objetivo

Definir el módulo administrativo para consultar, revisar y gestionar usuarios de la plataforma, incluyendo estado de cuenta, rol, vínculo con código de acceso y visibilidad de progreso a nivel operativo.

## Alcance

Incluye:

1. Listado de usuarios.
2. Búsqueda y filtrado.
3. Vista de detalle de usuario.
4. Estado de la cuenta.
5. Rol del usuario.
6. Relación con código de acceso y actividad general.
7. Auditoría de acciones administrativas relevantes.

## Fuera de alcance

1. Suplantación de sesión.
2. Edición avanzada de credenciales.
3. Soporte técnico conversacional.
4. CRM externo.

## Dependencias

1. `spec-modelo-datos-plataforma.md`
2. `spec-base-app-y-arquitectura.md`
3. `spec-auth-registro-con-codigo.md`
4. `spec-auditoria-administrativa.md`

## Actores y permisos

### admin
- puede ver listado de usuarios
- puede consultar detalle de usuario
- puede modificar estado o rol según reglas internas

### student
- no puede acceder a este módulo

## Rutas / pantallas

1. `/admin/users`
2. `/admin/users/[userId]`

## Componentes UI

1. Tabla de usuarios.
2. Filtros por rol, estado y fecha.
3. Buscador por nombre o email.
4. Vista de detalle del usuario.
5. Badges de estado y rol.
6. Confirmaciones para cambios sensibles.

## Server Actions / operaciones servidor

### 1. `getUsers`
### 2. `getUserDetail`
### 3. `updateUserStatus`
### 4. `updateUserRole`

Todas las mutaciones deben:
1. validar permiso admin
2. validar payload con Zod
3. persistir cambios
4. registrar auditoría

## Modelo de datos afectado

1. `User`
2. `AccessCode`
3. `LessonProgress`
4. `AuditLog`

## Reglas de negocio

1. Solo admins pueden gestionar usuarios.
2. El listado debe permitir identificar rápidamente email, rol, estado y fecha de alta.
3. El detalle de usuario debe mostrar el código usado en el registro si existe.
4. El sistema debe permitir distinguir usuarios activos, inactivos o bloqueados.
5. Los cambios administrativos sobre usuarios deben quedar auditados.
6. La visibilidad del progreso desde admin es operativa, no para modificar resultados pedagógicos desde este módulo.

## Validaciones

1. solo admins pueden acceder
2. el usuario objetivo debe existir
3. los cambios de rol deben estar restringidos a valores válidos
4. los cambios de estado deben estar restringidos a valores válidos

## Estados y errores

1. usuarios cargados
2. usuario no encontrado
3. cambio de estado aplicado
4. cambio de rol aplicado
5. error de validación
6. acceso denegado

## Criterios de aceptación

### CA-001 Listado operativo
**Dado** un administrador autenticado  
**Cuando** accede a `/admin/users`  
**Entonces** el sistema muestra un listado de usuarios con datos operativos básicos.

### CA-002 Detalle de usuario
**Dado** un administrador autenticado  
**Cuando** accede al detalle de un usuario existente  
**Entonces** el sistema muestra su información principal, su código asociado si existe y contexto general de actividad.

### CA-003 Cambio de estado
**Dado** un administrador autenticado  
**Cuando** actualiza el estado de un usuario con un valor válido  
**Entonces** el sistema guarda el cambio y lo registra en auditoría.

### CA-004 Restricción de acceso
**Dado** un usuario no administrador  
**Cuando** intenta acceder al módulo  
**Entonces** el sistema bloquea el acceso.

## Casos límite

1. Usuario sin código asociado por carga manual histórica.
2. Usuario bloqueado con progreso existente.
3. Gran volumen de usuarios.
4. Cambio de rol sobre el propio admin autenticado.

## Notas técnicas

1. La tabla debe permitir paginación futura.
2. Las acciones sensibles deben estar protegidas por permiso admin y auditoría.
3. La UI puede construirse con tabla y sheet/dialog usando **shadcn/ui**.
4. El detalle puede resolver agregados ligeros de progreso sin entrar en analítica profunda.

## Definición de terminado

Se considerará terminado este módulo cuando:

1. El listado, detalle y mutaciones básicas estén definidos.
2. Los cambios de estado/rol tengan criterios claros.
3. Un agente pueda implementar el módulo sin ambigüedad crítica.

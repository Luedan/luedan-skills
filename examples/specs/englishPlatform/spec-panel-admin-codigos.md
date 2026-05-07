# Spec: Panel admin de códigos

## Objetivo

Definir el módulo administrativo para crear, consultar, activar, desactivar, regalar y auditar códigos de acceso de un solo uso vinculados al producto único de la plataforma.

## Alcance

Incluye:

1. Listado de códigos.
2. Creación manual de códigos.
3. Cambio de estado de códigos.
4. Asignación o regalo de códigos.
5. Consulta de consumo y trazabilidad.
6. Registro de auditoría por acciones administrativas.

## Fuera de alcance

1. Venta automatizada externa.
2. Integración con pasarelas de pago.
3. Importación masiva avanzada, salvo decisión futura.
4. Reembolso o reversión comercial.

## Dependencias

1. `spec-modelo-datos-plataforma.md`
2. `spec-base-app-y-arquitectura.md`
3. `spec-auth-registro-con-codigo.md`

## Actores y permisos

### admin
- puede acceder al panel de códigos
- puede crear códigos
- puede activar o desactivar códigos
- puede regalar códigos
- puede consultar consumo y estado

### student
- no puede acceder a este módulo

## Rutas / pantallas

1. `/admin/codes`
2. `/admin/codes/new`
3. `/admin/codes/[codeId]`

## Componentes UI

1. Tabla de códigos.
2. Filtros por estado, origen y consumo.
3. Formulario de creación de código.
4. Acciones por fila: activar, desactivar, ver detalle.
5. Vista de detalle de código.
6. Diálogos de confirmación para acciones críticas.

## Server Actions / operaciones servidor

### 1. `createAccessCode`
Responsabilidades:
- generar o validar código manual
- persistir origen y metadata
- dejarlo disponible para uso
- registrar auditoría

### 2. `toggleAccessCodeStatus`
Responsabilidades:
- activar o desactivar código
- impedir inconsistencia sobre códigos consumidos si así se define
- registrar auditoría

### 3. `giftAccessCode`
Responsabilidades:
- crear o asignar código de regalo
- asociar metadata de regalo
- registrar auditoría

### 4. `getAccessCodes`
Responsabilidades:
- listar códigos con filtros y ordenación

### 5. `getAccessCodeDetail`
Responsabilidades:
- devolver estado, consumo, creador, usuario consumidor si existe y notas

## Modelo de datos afectado

1. `AccessCode`
2. `User` indirectamente cuando un código es consumido
3. `AuditLog`

## Reglas de negocio

1. Cada código es de un solo uso.
2. Un código consumido no puede volver a quedar disponible para reutilización funcional.
3. El administrador puede crear códigos para compra, regalo o gestión manual.
4. Los códigos deben tener trazabilidad de creación y consumo.
5. Las acciones administrativas sobre códigos deben registrarse en auditoría.
6. Debe poder visualizarse si un código ya fue usado y por qué cuenta.

## Validaciones

1. Solo admins pueden ejecutar acciones del módulo.
2. El código debe ser único.
3. El estado inicial por defecto debe ser `active`, salvo decisión explícita contraria.
4. `usesAllowed` por defecto debe ser 1.
5. No debe permitirse cambiar un código consumido a un estado que implique reutilización efectiva.

## Estados y errores

1. código creado
2. código activado
3. código desactivado
4. código ya existe
5. código no encontrado
6. acción no permitida
7. código ya consumido
8. error al guardar cambios

## Criterios de aceptación

### CA-001 Crear código
**Dado** un administrador autenticado  
**Cuando** crea un código válido  
**Entonces** el sistema lo persiste con estado inicial definido  
**Y** registra la acción en auditoría.

### CA-002 Desactivar código disponible
**Dado** un administrador autenticado y un código activo no consumido  
**Cuando** lo desactiva  
**Entonces** el sistema cambia su estado  
**Y** evita su uso en registros futuros.

### CA-003 Consultar consumo
**Dado** un administrador autenticado  
**Cuando** abre el detalle de un código consumido  
**Entonces** el sistema muestra que fue utilizado  
**Y** por qué usuario, si la información está disponible.

### CA-004 Regalo de código
**Dado** un administrador autenticado  
**Cuando** genera un código de regalo  
**Entonces** el sistema lo marca con origen de regalo  
**Y** registra la acción en auditoría.

## Casos límite

1. Intento de crear código duplicado manualmente.
2. Desactivar un código que ya fue consumido.
3. Regalar un código a un email aún no registrado.
4. Admin viendo listados muy extensos de códigos.

## Notas técnicas

1. La vista de listado puede implementarse inicialmente con tabla simple de **shadcn/ui**.
2. Las acciones de crear, activar y desactivar deben ejecutarse con **Server Actions**.
3. Debe existir índice único sobre `AccessCode.code`.
4. Se recomienda reflejar `sourceType` para distinguir compra, regalo y creación manual.
5. Debe existir logging de auditoría con `adminUserId`, acción, entidad y timestamp.

## Definición de terminado

Se considerará terminado este módulo cuando:

1. El comportamiento del ciclo de vida del código esté completamente definido.
2. Las operaciones administrativas principales tengan validaciones y errores claros.
3. Un agente pueda implementar el panel de códigos y sus acciones sin ambigüedad crítica.

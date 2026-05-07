# Spec: Permisos y seguridad funcional

## Objetivo

Definir las reglas de acceso, segregación funcional por rol, protección de rutas y restricciones de visibilidad de datos para evitar accesos indebidos y asegurar el comportamiento esperado del producto.

## Alcance

Incluye:

1. Roles funcionales.
2. Accesos por zona de producto.
3. Restricciones por ruta.
4. Restricciones por acción servidor.
5. Visibilidad de datos por rol.

## Fuera de alcance

1. Hardening de infraestructura.
2. Seguridad ofensiva avanzada.
3. MFA.
4. Gestión de secretos.

## Dependencias

1. `spec-base-app-y-arquitectura.md`
2. `spec-auth-registro-con-codigo.md`
3. `spec-panel-admin-usuarios.md`
4. `spec-auditoria-administrativa.md`

## Actores y permisos

### visitor
- acceso a home, login, register y legales

### student
- acceso a dashboard, catálogo, lecciones, progreso y acciones de aprendizaje propias

### admin
- acceso a panel administrativo y métricas

## Rutas / pantallas

### Públicas
- `/`
- `/login`
- `/register`
- `/terminos`
- `/privacidad`

### Estudiante
- `/app/*`

### Admin
- `/admin/*`

## Server Actions / operaciones servidor

Todas las Server Actions deben evaluar:

1. sesión válida
2. rol requerido
3. pertenencia del recurso cuando aplique

## Modelo de datos afectado

1. `User`
2. `AuditLog`
3. resto de entidades, por control de acceso indirecto

## Reglas de negocio

1. Un visitante no puede acceder a zonas privadas.
2. Un estudiante no puede acceder a zonas admin.
3. Un estudiante solo puede consultar y modificar sus propios datos de aprendizaje.
4. Un admin puede acceder a funciones administrativas.
5. Las mutaciones sensibles deben validar permisos en servidor, no solo en UI.
6. Los errores de acceso no deben exponer información innecesaria.

## Validaciones

1. sesión requerida en rutas privadas
2. rol `admin` requerido en rutas admin
3. ownership requerido para datos de estudiante
4. payload no confiable; siempre validar en servidor

## Estados y errores

1. no autenticado
2. no autorizado
3. acceso permitido
4. recurso no accesible

## Criterios de aceptación

### CA-001 Visitante bloqueado
**Dado** un visitante no autenticado  
**Cuando** intenta acceder a una ruta privada  
**Entonces** el sistema redirige a login o bloquea el acceso.

### CA-002 Student bloqueado en admin
**Dado** un estudiante autenticado  
**Cuando** intenta acceder a una ruta admin  
**Entonces** el sistema bloquea el acceso.

### CA-003 Ownership protegido
**Dado** un estudiante autenticado  
**Cuando** intenta consultar datos de aprendizaje de otro usuario  
**Entonces** el sistema bloquea el acceso.

### CA-004 Validación servidor
**Dado** una mutación sensible  
**Cuando** se ejecuta desde cliente manipulado  
**Entonces** el servidor vuelve a validar permisos antes de aplicar cambios.

## Casos límite

1. Sesión válida con rol cambiado recientemente.
2. Usuario autenticado intentando consumir recursos archivados o no publicados.
3. URL manipulada con identificadores ajenos.

## Notas técnicas

1. Se recomienda centralizar reglas de autorización en `lib/permissions`.
2. Middleware puede ayudar en rutas, pero la autorización final debe mantenerse en servidor.
3. La UI no debe ser la única barrera de seguridad.

## Definición de terminado

Se considerará terminado este módulo cuando:

1. La matriz de acceso por rol esté clara.
2. Las restricciones de servidor estén definidas.
3. Un agente pueda implementar guards y checks sin ambigüedad crítica.

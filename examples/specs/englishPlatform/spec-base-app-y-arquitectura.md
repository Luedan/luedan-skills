# Spec: Base app y arquitectura

## Objetivo

Definir la estructura base de la aplicación web, sus zonas funcionales, convenciones de organización, stack técnico y criterios de arquitectura para que múltiples agentes de código trabajen de forma consistente.

## Alcance

Incluye:

1. Stack técnico base.
2. Estructura de carpetas y zonas del producto.
3. Convenciones de App Router.
4. Convenciones de Server Actions.
5. Layouts, navegación y guards de acceso.
6. Principios de diseño UI.

## Fuera de alcance

1. Código final de bootstrap.
2. Configuración DevOps.
3. CI/CD.
4. Estrategia de observabilidad avanzada.

## Dependencias

1. `spec-plataforma-aprender-ingles.md`
2. `spec-modelo-datos-plataforma.md`

## Actores y permisos

### visitor
- Accede a zonas públicas.

### student
- Accede a zonas autenticadas de aprendizaje.

### admin
- Accede a panel de administración y auditoría.

## Stack técnico base

1. **Framework:** Next.js App Router.
2. **UI:** Tailwind CSS.
3. **Componentes:** shadcn/ui.
4. **Mutaciones servidor:** Server Actions.
5. **Persistencia:** PostgreSQL.
6. **ORM:** Prisma.
7. **Auth:** Auth.js.
8. **Validación:** Zod.
9. **Audio storage:** proveedor externo compatible con URL segura.

## Estructura funcional de la app

### Zona pública
Propuesta de rutas:

1. `/`
2. `/login`
3. `/register`
4. `/terminos`
5. `/privacidad`

### Zona privada estudiante
Propuesta de rutas:

1. `/app`
2. `/app/dashboard`
3. `/app/levels`
4. `/app/levels/[levelSlug]`
5. `/app/lessons/[lessonSlug]`
6. `/app/progress`

### Zona administrativa
Propuesta de rutas:

1. `/admin`
2. `/admin/users`
3. `/admin/codes`
4. `/admin/content`
5. `/admin/content/levels`
6. `/admin/content/lessons`
7. `/admin/content/exercises`
8. `/admin/audit`
9. `/admin/metrics`

## Convención de layouts

1. `PublicLayout` para rutas abiertas.
2. `AppLayout` para estudiantes autenticados.
3. `AdminLayout` para usuarios admin.
4. Los layouts deben resolver navegación, cabecera y control de visibilidad base.

## Convención de organización propuesta

Ejemplo orientativo:

```text
src/
  app/
    (public)/
    (student)/
    (admin)/
  components/
    ui/
    shared/
    student/
    admin/
  actions/
    auth/
    codes/
    content/
    exercises/
    progress/
    admin/
  lib/
    auth/
    db/
    permissions/
    validators/
    scoring/
  features/
    auth/
    codes/
    lessons/
    exercises/
    progress/
    audit/
```

## Componentes UI base

1. Header público.
2. Sidebar o navegación estudiante.
3. Sidebar o navegación admin.
4. Formularios shadcn/ui para login, registro, creación de códigos y edición de contenido.
5. Tablas admin para listados.
6. Cards para lecciones, niveles y progreso.
7. Toaster o patrón uniforme de feedback.

## Server Actions / operaciones servidor

Convenciones:

1. Toda mutación de datos debe ejecutarse desde Server Actions.
2. Las Server Actions deben validar entrada con Zod.
3. Las Server Actions deben aplicar autorización antes de mutar.
4. Las Server Actions administrativas deben registrar auditoría cuando aplique.
5. Las lecturas se priorizan en Server Components o funciones servidor reutilizables.

Operaciones base esperadas:

1. registro con código
2. login/logout según integración Auth.js
3. creación y consumo de códigos
4. CRUD de niveles/lecciones/ejercicios
5. guardado de intentos
6. actualización de progreso
7. escritura de logs de auditoría

## Modelo de datos afectado

Impacta indirectamente todas las entidades del dominio, especialmente:

1. User
2. AccessCode
3. Level
4. Lesson
5. Exercise
6. LessonAttempt
7. LessonProgress
8. AuditLog

## Reglas de negocio

1. Un visitante no debe poder acceder a rutas privadas.
2. Un estudiante no debe poder acceder a rutas administrativas.
3. Un admin sí puede acceder a zonas privadas administrativas.
4. Toda mutación sensible debe pasar por validación de permisos.
5. La arquitectura debe favorecer desacoplamiento por módulos.

## Validaciones

1. Rutas privadas protegidas por sesión válida.
2. Rutas admin protegidas por rol `admin`.
3. Formularios validados en cliente y servidor, siendo el servidor la fuente de verdad.
4. Parámetros de ruta deben resolverse de forma segura.

## Estados y errores

1. no autenticado
2. autenticado sin permiso
3. recurso no encontrado
4. validación fallida
5. error de persistencia
6. error de red o proveedor externo

## Criterios de aceptación

### CA-001 Separación de zonas
**Dado** un usuario visitante  
**Cuando** intenta acceder a una ruta privada  
**Entonces** el sistema debe redirigirlo a login.

### CA-002 Protección admin
**Dado** un estudiante autenticado  
**Cuando** intenta acceder a una ruta de administración  
**Entonces** el sistema debe bloquear el acceso.

### CA-003 Convención de mutaciones
**Dado** una operación de escritura  
**Cuando** el sistema modifica datos del dominio  
**Entonces** la operación debe ejecutarse a través de Server Actions.

### CA-004 Consistencia visual
**Dado** diferentes módulos del sistema  
**Cuando** muestran formularios, tablas o feedback  
**Entonces** deben reutilizar patrones coherentes basados en Tailwind y shadcn/ui.

## Casos límite

1. Usuario con sesión válida pero rol cambiado en base de datos.
2. Usuario autenticado intentando acceder a contenido despublicado.
3. Ruta con slug inexistente.
4. Server Action llamada con payload manipulado.

## Notas técnicas

1. Se recomienda App Router con grupos de rutas `(public)`, `(student)` y `(admin)`.
2. Se recomienda centralizar permisos en `lib/permissions`.
3. Se recomienda separar validadores Zod por feature.
4. Se recomienda encapsular acceso a base de datos en una capa de utilidades o servicios ligeros.
5. Los componentes de `shadcn/ui` deben ser la base para formularios, diálogos, tablas y feedback.

## Definición de terminado

Se considerará terminado este módulo cuando:

1. Exista una arquitectura aprobada para organización del proyecto.
2. Se hayan definido las zonas públicas, privadas y admin.
3. Los agentes de código puedan ubicar rutas, acciones y componentes sin ambigüedad alta.

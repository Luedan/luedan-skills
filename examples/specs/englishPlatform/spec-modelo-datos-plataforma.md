# Spec: Modelo de datos de la plataforma

## Objetivo

Definir el modelo de datos base de la plataforma de aprendizaje de inglés, sus entidades principales, relaciones, restricciones funcionales y decisiones técnicas mínimas para soportar registro con código, acceso vitalicio, contenido educativo, progreso, ejercicios y auditoría.

## Alcance

Incluye:

1. Definición de entidades principales.
2. Relaciones entre entidades.
3. Restricciones funcionales y de integridad.
4. Recomendaciones técnicas para persistencia.
5. Trazabilidad con la spec madre.

## Fuera de alcance

1. Migraciones concretas.
2. SQL final de producción.
3. Optimización avanzada de performance.
4. Estrategia de backups o infraestructura.

## Dependencias

1. `spec-plataforma-aprender-ingles.md`
2. Decisión técnica base:
   - Next.js App Router
   - PostgreSQL
   - Prisma
   - Auth.js
   - Zod

## Actores y permisos

### student
- Tiene datos propios de acceso, progreso e intentos.

### admin
- Gestiona usuarios, códigos, contenido, parámetros y auditoría.

## Entidades principales

### 1. User
Representa una cuenta del sistema.

Campos funcionales mínimos:
- id
- name
- email
- passwordHash o credencial equivalente
- role (`student`, `admin`)
- status (`active`, `inactive`, `blocked`)
- accessGrantedAt
- accessCodeId usado en el registro
- createdAt
- updatedAt

Restricciones:
- email único
- una cuenta no puede estar vinculada a más de un código consumido como origen de acceso inicial

### 2. AccessCode
Representa un código de acceso de compra o regalo.

Campos funcionales mínimos:
- id
- code
- status (`active`, `inactive`, `consumed`)
- sourceType (`purchase`, `gift`, `manual`)
- usesAllowed
- usesConsumed
- consumedByUserId
- createdByAdminId
- assignedToEmail opcional
- notes
- expiresAt opcional
- createdAt
- updatedAt
- consumedAt opcional

Restricciones:
- `code` único
- en esta plataforma el valor esperado por defecto es `usesAllowed = 1`
- si `usesConsumed >= usesAllowed`, el código no puede reutilizarse

### 3. Course
Representa el producto académico principal.

Campos funcionales mínimos:
- id
- slug
- title
- description
- status (`draft`, `published`, `archived`)
- createdAt
- updatedAt

Restricciones:
- inicialmente existirá un solo curso comercial: aprender inglés
- `slug` único

### 4. Level
Agrupa lecciones por nivel académico.

Campos funcionales mínimos:
- id
- courseId
- slug
- title
- description
- orderIndex
- status (`draft`, `published`, `archived`)
- createdAt
- updatedAt

Restricciones:
- `orderIndex` único dentro de un mismo curso

### 5. Lesson
Unidad de aprendizaje consumible por el estudiante.

Campos funcionales mínimos:
- id
- levelId
- slug
- title
- summary
- content
- status (`draft`, `published`, `archived`)
- orderIndex
- minScorePercent
- estimatedDurationMinutes opcional
- createdAt
- updatedAt

Restricciones:
- `minScorePercent` por defecto: 90
- `minScorePercent` configurable por admin
- `orderIndex` único dentro de un mismo nivel

### 6. LessonAudio
Recursos de audio asociados a una lección o parte de ella.

Campos funcionales mínimos:
- id
- lessonId
- title
- storageKey o url
- durationSeconds opcional
- orderIndex
- transcript opcional
- status (`active`, `inactive`)
- createdAt
- updatedAt

### 7. Exercise
Representa una actividad evaluable dentro de una lección.

Campos funcionales mínimos:
- id
- lessonId
- type (`multiple_choice`, `fill_blank`, `translation`, `listening`, `dictation`)
- prompt
- instructions opcional
- orderIndex
- isRequired
- points
- configJson
- status (`draft`, `published`, `archived`)
- createdAt
- updatedAt

Notas:
- `configJson` permite representar opciones, texto base, respuestas aceptadas, tolerancia, audio vinculado, etc.

### 8. ExerciseOption
Opciones para ejercicios de selección cuando aplique.

Campos funcionales mínimos:
- id
- exerciseId
- label
- value
- isCorrect
- orderIndex

###+ Nota
Puede omitirse si se decide persistir opciones dentro de `configJson`, pero se recomienda entidad separada para facilitar edición administrativa.

### 9. ExerciseAttempt
Registro de un intento de respuesta sobre un ejercicio.

Campos funcionales mínimos:
- id
- userId
- lessonId
- exerciseId
- attemptGroupId
- submittedAnswer
- normalizedAnswer opcional
- scorePercent
- isCorrect
- evaluationDetailJson
- submittedAt

Notas:
- `attemptGroupId` permite agrupar varios ejercicios dentro de una misma ejecución de lección.

### 10. LessonAttempt
Representa una ejecución completa de una lección por un usuario.

Campos funcionales mínimos:
- id
- userId
- lessonId
- attemptNumber
- scorePercent
- passed
- startedAt
- finishedAt opcional

Restricciones:
- `attemptNumber` incremental por combinación `userId + lessonId`

### 11. LessonProgress
Estado acumulado del estudiante respecto a una lección.

Campos funcionales mínimos:
- id
- userId
- lessonId
- status (`not_started`, `in_progress`, `completed`)
- bestScorePercent
- completedAt opcional
- lastAttemptAt opcional
- attemptsCount
- updatedAt

Restricciones:
- único por combinación `userId + lessonId`

### 12. AuditLog
Registro de acciones administrativas críticas.

Campos funcionales mínimos:
- id
- adminUserId
- entityType
- entityId
- actionType
- summary
- detailJson
- createdAt

Restricciones:
- solo debe registrar acciones de contexto administrativo o de seguridad funcional relevante

## Relaciones

1. Un `Course` tiene muchos `Level`.
2. Un `Level` tiene muchas `Lesson`.
3. Una `Lesson` tiene muchos `LessonAudio`.
4. Una `Lesson` tiene muchos `Exercise`.
5. Un `Exercise` puede tener muchas `ExerciseOption`.
6. Un `User` puede tener muchos `ExerciseAttempt`.
7. Un `User` puede tener muchos `LessonAttempt`.
8. Un `User` puede tener muchos `LessonProgress`.
9. Un `AccessCode` puede quedar consumido por un único `User`.
10. Un `Admin User` puede crear muchos `AccessCode`.
11. Un `Admin User` puede generar muchos `AuditLog`.

## Rutas / pantallas afectadas

Este módulo no define pantallas finales, pero impacta directamente:

1. registro
2. login
3. dashboard
4. visor de lecciones
5. panel admin de códigos
6. panel admin de contenido
7. panel admin de usuarios
8. auditoría

## Server Actions / operaciones servidor impactadas

1. Crear usuario con código.
2. Validar y consumir código.
3. Crear/editar/desactivar código.
4. Crear/editar/publicar lección.
5. Crear/editar ejercicio.
6. Guardar intento de ejercicio.
7. Calcular y persistir progreso.
8. Registrar evento de auditoría.

## Reglas de negocio

1. Un código de acceso es de un solo uso.
2. El código consumido debe quedar asociado a la cuenta creada.
3. El acceso es vitalicio y no debe depender de renovaciones.
4. Existe un único producto académico, pero el modelo debe permitir varios niveles y lecciones.
5. La completitud depende de alcanzar el porcentaje mínimo de la lección.
6. Deben permitirse reintentos ilimitados.
7. La plataforma debe guardar historial de intentos y el mejor resultado.
8. La edición administrativa debe ser auditable.

## Validaciones

1. Email único en `User`.
2. Código único en `AccessCode`.
3. `usesAllowed` mayor o igual a 1.
4. `usesConsumed` no puede ser mayor que `usesAllowed`.
5. `minScorePercent` entre 1 y 100.
6. `scorePercent` entre 0 y 100.
7. `orderIndex` no debe duplicarse dentro del mismo contenedor lógico.

## Estados y errores

1. Código inválido.
2. Código consumido.
3. Código inactivo.
4. Lección no publicada.
5. Ejercicio no publicado.
6. Error de integridad al crear relaciones obligatorias.

## Criterios de aceptación

### CA-001 Código consumible una sola vez
**Dado** un código activo con un uso permitido  
**Cuando** se utiliza en un registro exitoso  
**Entonces** el sistema incrementa su consumo  
**Y** evita cualquier reutilización posterior.

### CA-002 Vinculación de acceso
**Dado** un registro válido  
**Cuando** se crea la cuenta  
**Entonces** el código utilizado queda vinculado al usuario creado.

### CA-003 Progreso único por usuario y lección
**Dado** un usuario y una lección  
**Cuando** el sistema actualiza el progreso  
**Entonces** existe un solo registro acumulado de progreso para esa combinación.

### CA-004 Auditoría registrable
**Dado** una acción administrativa crítica  
**Cuando** se persiste el cambio  
**Entonces** el sistema puede crear un registro trazable en `AuditLog`.

## Casos límite

1. Código creado pero sin admin asociado por importación masiva.
2. Usuario bloqueado con acceso vitalicio previamente concedido.
3. Cambio del porcentaje mínimo después de intentos previos.
4. Ejercicios con múltiples respuestas aceptadas.
5. Eliminación lógica de contenido con progreso existente.

## Notas técnicas

1. Se recomienda **PostgreSQL** por consistencia relacional y facilidad para índices únicos compuestos.
2. Se recomienda **Prisma** como ORM para acelerar iteración con agentes de código.
3. Las entidades con estados de negocio deben preferir **soft delete lógico** o `status` sobre borrado físico.
4. Los detalles variables de corrección pueden persistirse en JSON tipado/validado.
5. Las acciones de creación y mutación deben ejecutarse mediante **Server Actions**.
6. La autenticación puede resolverse con **Auth.js** usando email/password.

## Definición de terminado

Se considerará terminado este módulo cuando:

1. Exista una definición aprobada de entidades y relaciones.
2. Se hayan acordado restricciones clave de unicidad e integridad.
3. Los módulos de auth, contenido, ejercicios, progreso y auditoría puedan apoyarse en este modelo sin ambigüedad crítica.

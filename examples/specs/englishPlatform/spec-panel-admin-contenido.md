# Spec: Panel admin de contenido

## Objetivo

Definir el módulo administrativo para gestionar la estructura académica y el contenido: niveles, lecciones, materiales, orden de publicación y parámetros académicos como el porcentaje mínimo requerido por lección.

## Alcance

Incluye:

1. Gestión de niveles.
2. Gestión de lecciones.
3. Gestión del orden académico.
4. Estados de publicación.
5. Parametrización del porcentaje mínimo por lección.
6. Auditoría de acciones relevantes.

## Fuera de alcance

1. Gestión detallada del motor de ejercicios.
2. Corrección automática.
3. Gestión avanzada de métricas.
4. Versionado editorial sofisticado.

## Dependencias

1. `spec-modelo-datos-plataforma.md`
2. `spec-base-app-y-arquitectura.md`
3. `spec-catalogo-estructura-academica.md`

## Actores y permisos

### admin
- puede crear, editar, despublicar y archivar contenido
- puede ordenar niveles y lecciones
- puede configurar porcentaje mínimo por lección

### student
- no puede acceder a este módulo

## Rutas / pantallas

1. `/admin/content`
2. `/admin/content/levels`
3. `/admin/content/levels/new`
4. `/admin/content/levels/[levelId]`
5. `/admin/content/lessons`
6. `/admin/content/lessons/new`
7. `/admin/content/lessons/[lessonId]`

## Componentes UI

1. Tabla de niveles.
2. Tabla de lecciones.
3. Formularios de creación/edición.
4. Selectores de estado.
5. Inputs de orden.
6. Input de porcentaje mínimo.
7. Confirmaciones para cambios críticos.

## Server Actions / operaciones servidor

### 1. `createLevel`
### 2. `updateLevel`
### 3. `changeLevelStatus`
### 4. `reorderLevels`
### 5. `createLesson`
### 6. `updateLesson`
### 7. `changeLessonStatus`
### 8. `reorderLessons`
### 9. `updateLessonMinScore`

Todas deben:
1. validar payload con Zod
2. comprobar permiso admin
3. persistir cambios
4. registrar auditoría cuando aplique

## Modelo de datos afectado

1. `Course`
2. `Level`
3. `Lesson`
4. `AuditLog`

## Reglas de negocio

1. Solo admins pueden gestionar contenido.
2. Los niveles y lecciones pueden estar en estados `draft`, `published` o `archived`.
3. Solo el contenido publicado debe aparecer al estudiante.
4. Cada lección debe pertenecer a un nivel.
5. El porcentaje mínimo por defecto es 90%, pero puede configurarse por lección.
6. El orden académico debe respetarse en catálogo y navegación.
7. Toda acción de creación, edición o cambio de estado relevante debe quedar auditada.

## Validaciones

1. título de nivel obligatorio
2. título de lección obligatorio
3. slug único por entidad
4. `orderIndex` válido dentro del contenedor correspondiente
5. `minScorePercent` entre 1 y 100
6. lección obligatoriamente asociada a un nivel existente

## Estados y errores

1. nivel creado
2. nivel actualizado
3. lección creada
4. lección actualizada
5. cambio de estado guardado
6. porcentaje mínimo actualizado
7. slug duplicado
8. error de validación
9. error al guardar cambios

## Criterios de aceptación

### CA-001 Crear nivel
**Dado** un administrador autenticado  
**Cuando** crea un nivel con datos válidos  
**Entonces** el sistema lo persiste  
**Y** registra la acción en auditoría.

### CA-002 Crear lección
**Dado** un administrador autenticado  
**Cuando** crea una lección asociada a un nivel válido  
**Entonces** el sistema la persiste correctamente.

### CA-003 Publicación controlada
**Dado** una lección en estado borrador  
**Cuando** el administrador la publica  
**Entonces** pasa a estar disponible para el catálogo del estudiante.

### CA-004 Parametrizar porcentaje mínimo
**Dado** un administrador autenticado  
**Cuando** establece un porcentaje mínimo válido para una lección  
**Entonces** el sistema guarda el valor  
**Y** lo utiliza en la evaluación futura.

## Casos límite

1. Cambio de orden que provoca conflicto con índices existentes.
2. Lección archivada con progreso ya existente.
3. Nivel publicado sin lecciones.
4. Reducción o aumento del porcentaje mínimo tras intentos previos.

## Notas técnicas

1. Se recomienda separar formularios de nivel y lección por feature.
2. Las operaciones de reorder pueden resolverse con batch updates vía Server Actions.
3. Los campos de contenido largo pueden apoyarse en editor simple inicialmente.
4. Las tablas y formularios deben reutilizar patrones **shadcn/ui**.

## Definición de terminado

Se considerará terminado este módulo cuando:

1. El CRUD funcional de niveles y lecciones esté definido.
2. Las reglas de publicación y orden estén cerradas.
3. Un agente pueda construir el panel admin de contenido sin ambigüedad crítica.

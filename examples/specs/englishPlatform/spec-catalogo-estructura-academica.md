# Spec: Catálogo y estructura académica

## Objetivo

Definir cómo se organiza, publica y expone al estudiante la estructura académica de la plataforma: plan único, niveles, lecciones y orden de consumo.

## Alcance

Incluye:

1. Plan único de aprendizaje.
2. Jerarquía de curso, niveles y lecciones.
3. Reglas de visibilidad.
4. Orden y navegación académica.
5. Exposición del catálogo al estudiante.

## Fuera de alcance

1. Edición administrativa detallada del contenido.
2. Renderizado interno de la lección.
3. Corrección de ejercicios.
4. Métricas analíticas avanzadas.

## Dependencias

1. `spec-plataforma-aprender-ingles.md`
2. `spec-modelo-datos-plataforma.md`
3. `spec-base-app-y-arquitectura.md`

## Actores y permisos

### student
- puede consultar niveles y lecciones publicadas
- puede navegar por la estructura académica

### admin
- puede definir la estructura y publicación desde módulos administrativos relacionados

## Rutas / pantallas

1. `/app/levels`
2. `/app/levels/[levelSlug]`
3. `/app/lessons/[lessonSlug]`

## Componentes UI

1. Vista de listado de niveles.
2. Vista de detalle de nivel con lecciones.
3. Cards de nivel.
4. Cards o filas de lección.
5. Indicadores de estado de progreso por lección.
6. Estados vacíos cuando no haya contenido publicado.

## Server Actions / operaciones servidor

Este módulo prioriza lectura, por lo que se apoya principalmente en consultas servidor.

Operaciones esperadas:
1. obtener niveles publicados
2. obtener detalle de nivel con lecciones publicadas
3. obtener navegación previa/siguiente de lecciones

## Modelo de datos afectado

1. `Course`
2. `Level`
3. `Lesson`
4. `LessonProgress` para estados del usuario

## Reglas de negocio

1. Existe un único plan comercial con acceso a todos los niveles.
2. El estudiante autenticado con acceso válido puede consultar todo el catálogo publicado.
3. Solo deben mostrarse niveles y lecciones publicadas.
4. El orden de visualización debe respetar `orderIndex`.
5. Una lección despublicada no debe mostrarse al estudiante.
6. El catálogo debe reflejar el estado de progreso del estudiante cuando exista.

## Validaciones

1. El usuario debe estar autenticado.
2. Los slugs de nivel y lección deben resolverse a registros publicados.
3. Si un slug no existe o no está publicado, se debe responder como no disponible o no encontrado.

## Estados y errores

1. catálogo cargando
2. catálogo sin contenido disponible
3. nivel no encontrado
4. lección no disponible
5. error al consultar contenido

## Criterios de aceptación

### CA-001 Listado de niveles
**Dado** un estudiante autenticado  
**Cuando** accede a `/app/levels`  
**Entonces** el sistema muestra los niveles publicados ordenados correctamente.

### CA-002 Detalle de nivel
**Dado** un estudiante autenticado  
**Cuando** accede al detalle de un nivel publicado  
**Entonces** el sistema muestra sus lecciones publicadas en el orden configurado.

### CA-003 Exclusión de contenido no publicado
**Dado** un estudiante autenticado  
**Cuando** existe una lección en estado borrador o archivado  
**Entonces** esa lección no se muestra en el catálogo.

### CA-004 Visibilidad de progreso
**Dado** un estudiante con progreso previo  
**Cuando** consulta el catálogo  
**Entonces** el sistema refleja el estado conocido de sus lecciones.

## Casos límite

1. Nivel publicado sin lecciones publicadas.
2. Lección publicada en un nivel despublicado.
3. Cambios de orden mientras el estudiante navega.
4. Lección despublicada después de estar visible en caché.

## Notas técnicas

1. Se recomienda resolver el catálogo mediante **Server Components**.
2. El progreso puede enriquecerse cruzando `Lesson` con `LessonProgress`.
3. Los slugs deben ser estables y únicos.
4. La navegación debe priorizar simplicidad y consistencia con **shadcn/ui** y **Tailwind CSS**.

## Definición de terminado

Se considerará terminado este módulo cuando:

1. La estructura jerárquica del catálogo esté claramente definida.
2. Las reglas de publicación y visibilidad estén cerradas.
3. Un agente pueda implementar el catálogo del estudiante sin ambigüedad crítica.

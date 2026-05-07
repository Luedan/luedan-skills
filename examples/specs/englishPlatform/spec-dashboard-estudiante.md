# Spec: Dashboard del estudiante

## Objetivo

Definir el panel principal del estudiante autenticado, mostrando su estado de avance, acceso rápido al contenido, resumen de progreso y contexto útil para continuar aprendiendo.

## Alcance

Incluye:

1. Vista inicial tras login del estudiante.
2. Resumen de progreso general.
3. Accesos rápidos a niveles o lecciones.
4. Resumen de actividad reciente.
5. Estados vacíos o iniciales.

## Fuera de alcance

1. Detalle completo de métricas analíticas.
2. Corrección de ejercicios.
3. Gestión administrativa.
4. Gamificación avanzada.

## Dependencias

1. `spec-catalogo-estructura-academica.md`
2. `spec-progreso-y-completitud.md`
3. `spec-base-app-y-arquitectura.md`

## Actores y permisos

### student
- puede acceder a su dashboard y ver solo su información

### admin
- no usa este dashboard como experiencia principal del producto

## Rutas / pantallas

1. `/app/dashboard`

## Componentes UI

1. Encabezado de bienvenida.
2. Card de progreso general.
3. Listado o grid de niveles/lecciones destacadas.
4. Sección de actividad reciente.
5. Estado vacío para usuario nuevo.

## Server Actions / operaciones servidor

Este módulo prioriza lectura en servidor.

Operaciones esperadas:
1. obtener resumen de progreso del usuario
2. obtener lecciones recientes o sugeridas
3. obtener últimos intentos o actividad reciente

## Modelo de datos afectado

1. `User`
2. `LessonProgress`
3. `LessonAttempt`
4. `Level`
5. `Lesson`

## Reglas de negocio

1. El dashboard solo debe mostrar información del usuario autenticado.
2. Debe servir como punto de reentrada principal al aprendizaje.
3. Debe mostrar un resumen claro del avance del estudiante.
4. Si no existe progreso previo, debe mostrar un estado inicial útil y guiar a empezar.
5. La información debe reflejar contenido publicado y accesible.

## Validaciones

1. acceso solo para estudiantes autenticados
2. no exponer datos de otros usuarios
3. no enlazar contenido no publicado

## Estados y errores

1. cargando dashboard
2. dashboard sin progreso previo
3. error al cargar resumen
4. contenido recomendado no disponible

## Criterios de aceptación

### CA-001 Acceso tras login
**Dado** un estudiante autenticado  
**Cuando** entra a `/app/dashboard`  
**Entonces** el sistema muestra su panel personal.

### CA-002 Usuario nuevo
**Dado** un estudiante sin progreso previo  
**Cuando** accede al dashboard  
**Entonces** el sistema muestra un estado inicial con llamada a comenzar el aprendizaje.

### CA-003 Usuario con progreso
**Dado** un estudiante con lecciones ya iniciadas o completadas  
**Cuando** accede al dashboard  
**Entonces** el sistema muestra un resumen coherente de su avance.

### CA-004 Actividad reciente
**Dado** un estudiante con intentos registrados  
**Cuando** accede al dashboard  
**Entonces** el sistema puede mostrar su actividad reciente o últimos avances.

## Casos límite

1. Usuario con acceso pero sin ninguna lección publicada aún.
2. Progreso existente sobre contenido archivado.
3. Dashboard con gran cantidad de actividad histórica.
4. Resumen desactualizado por fallo temporal de lectura.

## Notas técnicas

1. Se recomienda implementar el dashboard como **Server Component** enriquecido con datos agregados.
2. El cálculo del progreso general puede derivarse inicialmente de `LessonProgress`.
3. La UI debe priorizar claridad, continuidad y acceso rápido a seguir aprendiendo.
4. Deben reutilizarse componentes de cards, badges y tablas ligeras de **shadcn/ui**.

## Definición de terminado

Se considerará terminado este módulo cuando:

1. Esté definido el contenido mínimo del dashboard.
2. Los estados de usuario nuevo y usuario con progreso estén cubiertos.
3. Un agente pueda implementarlo sin ambigüedad crítica.

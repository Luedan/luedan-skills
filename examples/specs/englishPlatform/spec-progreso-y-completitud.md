# Spec: Progreso y completitud

## Objetivo

Definir cómo se calcula, persiste y expone el progreso del estudiante, incluyendo historial de intentos, mejor score, estado de lección y criterio de completitud basado en porcentaje mínimo configurable.

## Alcance

Incluye:

1. Persistencia de intentos.
2. Actualización de progreso por lección.
3. Estado no iniciada / en progreso / completada.
4. Reintentos ilimitados.
5. Criterio de completitud por porcentaje mínimo.
6. Mejor score acumulado.

## Fuera de alcance

1. Gamificación avanzada.
2. Recomendaciones adaptativas.
3. Métricas agregadas de negocio.
4. Certificados o logros.

## Dependencias

1. `spec-modelo-datos-plataforma.md`
2. `spec-correccion-y-tolerancia.md`
3. `spec-motor-ejercicios.md`

## Actores y permisos

### student
- puede consultar su progreso
- puede reintentar lecciones ilimitadamente

### admin
- puede parametrizar el umbral mínimo por lección desde otros módulos

## Rutas / pantallas

1. `/app/lessons/[lessonSlug]`
2. `/app/progress`
3. `/app/dashboard`

## Componentes UI

1. Indicador de estado de lección.
2. Porcentaje obtenido en intento.
3. Mejor score alcanzado.
4. Mensaje de completada o pendiente.
5. Historial resumido de intentos.

## Server Actions / operaciones servidor

### 1. `finalizeLessonAttempt`
Responsabilidades:
- persistir el intento de lección
- guardar intentos de ejercicios si aplica
- calcular `passed`
- actualizar progreso acumulado

### 2. `updateLessonProgress`
Responsabilidades:
- crear o actualizar `LessonProgress`
- mantener `bestScorePercent`
- mantener `attemptsCount`
- actualizar `status`

## Modelo de datos afectado

1. `LessonAttempt`
2. `ExerciseAttempt`
3. `LessonProgress`
4. `Lesson`

## Reglas de negocio

1. Los reintentos son ilimitados.
2. Cada intento debe quedar registrado.
3. El progreso por lección es único por usuario.
4. Una lección se marca como completada solo si el score del intento alcanza o supera el porcentaje mínimo configurado.
5. El porcentaje mínimo por defecto es 90%, pero puede variar por lección.
6. Si un usuario mejora su score, el progreso debe conservar el mejor resultado.
7. Un usuario puede tener múltiples intentos fallidos antes de completar una lección.
8. El estado debe pasar de `not_started` a `in_progress` cuando exista actividad, y a `completed` al alcanzar el umbral.

## Validaciones

1. la lección debe existir y ser accesible
2. el porcentaje del intento debe estar entre 0 y 100
3. el progreso debe ser único por combinación usuario-lección
4. `attemptNumber` debe incrementarse correctamente

## Estados y errores

1. lección no iniciada
2. lección en progreso
3. lección completada
4. intento guardado
5. error al guardar intento
6. error al actualizar progreso

## Criterios de aceptación

### CA-001 Primer intento crea progreso
**Dado** un estudiante sin progreso previo en una lección  
**Cuando** realiza su primer intento  
**Entonces** el sistema crea su registro de progreso.

### CA-002 Intento insuficiente
**Dado** un estudiante en una lección  
**Cuando** obtiene menos del porcentaje mínimo  
**Entonces** el sistema mantiene la lección como no completada o en progreso.

### CA-003 Intento suficiente
**Dado** un estudiante en una lección  
**Cuando** obtiene un porcentaje igual o superior al mínimo configurado  
**Entonces** el sistema marca la lección como completada.

### CA-004 Mejor score persistido
**Dado** un estudiante con varios intentos sobre una lección  
**Cuando** uno de ellos supera el score anterior  
**Entonces** el sistema actualiza el mejor score guardado.

### CA-005 Reintentos ilimitados
**Dado** un estudiante que no ha alcanzado el umbral  
**Cuando** vuelve a intentar la lección varias veces  
**Entonces** el sistema registra cada intento sin impedir nuevos reintentos.

## Casos límite

1. Lección completada y luego porcentaje mínimo aumentado por admin.
2. Varios intentos casi simultáneos.
3. Progreso existente con lección archivada.
4. Intento con guardado parcial de ejercicios pero fallo en progreso.

## Notas técnicas

1. Se recomienda tratar la persistencia de intento y progreso dentro de una operación transaccional cuando sea posible.
2. El historial debe distinguir claramente entre intento individual y progreso acumulado.
3. `LessonProgress` debe ser la fuente rápida de consulta y `LessonAttempt` la fuente histórica.
4. La UI debe mostrar mejor score y estado actual de forma consistente.

## Definición de terminado

Se considerará terminado este módulo cuando:

1. Esté definido el ciclo de vida del progreso por lección.
2. Estén claras las reglas de completitud y reintento.
3. Un agente pueda implementar progreso e historial sin ambigüedad crítica.

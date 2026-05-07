# Spec: Corrección y tolerancia

## Objetivo

Definir las reglas funcionales y técnico-funcionales para evaluar respuestas de ejercicios, calcular porcentajes de acierto y aplicar tolerancia parcial en tipos de respuesta textual como traducción y dictado.

## Alcance

Incluye:

1. Reglas de corrección por tipo de ejercicio.
2. Cálculo de score por ejercicio.
3. Cálculo de score por intento de lección.
4. Tolerancia parcial para traducción y dictado.
5. Estructura esperada del resultado de corrección.

## Fuera de alcance

1. Visualización detallada del dashboard.
2. Analítica avanzada de rendimiento pedagógico.
3. IA semántica compleja de corrección.
4. Recomendaciones adaptativas de contenido.

## Dependencias

1. `spec-modelo-datos-plataforma.md`
2. `spec-motor-ejercicios.md`
3. `spec-plataforma-aprender-ingles.md`

## Actores y permisos

### student
- recibe evaluación de sus respuestas

### admin
- define configuración del contenido que condiciona la corrección

## Rutas / pantallas

Este módulo se aplica principalmente dentro de:

1. `/app/lessons/[lessonSlug]`
2. `/app/dashboard` de manera resumida

## Componentes UI

1. Resumen de score del intento.
2. Indicadores por respuesta correcta o incorrecta.
3. Mensajes de porcentaje alcanzado o no alcanzado.
4. Feedback parcial cuando el producto lo exponga.

## Server Actions / operaciones servidor

### 1. `evaluateLessonAttempt`
Responsabilidades:
- recibir respuestas enviadas
- obtener configuración de ejercicios
- aplicar reglas de corrección por tipo
- calcular score por ejercicio
- calcular porcentaje total del intento
- devolver resultado estructurado

### 2. `normalizeTextAnswer`
Responsabilidades:
- normalizar respuestas textuales para corrección
- aplicar reglas como trim, case folding, espacios y puntuación, según criterio definido

## Modelo de datos afectado

1. `Exercise`
2. `ExerciseAttempt`
3. `LessonAttempt`
4. `LessonProgress`

## Reglas de corrección por tipo

### 1. Opción múltiple
- se corrige contra la opción válida configurada
- resultado binario por defecto: correcto o incorrecto

### 2. Completar huecos
- se corrige cada hueco contra respuestas esperadas
- puede calcularse score proporcional según cantidad de huecos correctos

### 3. Traducción
- se corrige como respuesta textual
- debe contemplar tolerancia parcial razonable
- deben tolerarse diferencias no sustanciales cuando la intención sea correcta según configuración funcional

### 4. Listening
- se corrige según el subtipo de respuesta asociado
- puede ser opción múltiple o textual

### 5. Dictado
- se corrige como respuesta textual
- debe contemplar tolerancia parcial razonable

## Reglas de tolerancia textual

Se aplicarán al menos estas normalizaciones base antes de evaluar:

1. eliminación de espacios extra al inicio y final
2. unificación básica de mayúsculas/minúsculas
3. tratamiento configurable de puntuación no esencial
4. comparación con una o varias respuestas válidas

La tolerancia parcial debe permitir:

1. reconocer respuestas cercanas pero no perfectas
2. evitar exigir precisión nativa absoluta
3. otorgar score parcial cuando la configuración del ejercicio lo permita

No debe permitir:

1. aceptar respuestas claramente incorrectas por coincidencia mínima
2. romper el criterio pedagógico del ejercicio

## Reglas de negocio

1. Todo intento debe producir un porcentaje de acierto entre 0 y 100.
2. La corrección debe ser consistente y repetible para el mismo input.
3. Traducción y dictado deben admitir tolerancia parcial.
4. El criterio de tolerancia debe ser funcionalmente razonable y parametrizable mediante configuración del ejercicio cuando aplique.
5. El resultado de corrección debe poder persistirse para historial y progreso.

## Validaciones

1. las respuestas deben corresponder a ejercicios de la lección
2. cada tipo de ejercicio debe recibirse en el formato esperado
3. las respuestas textuales deben poder normalizarse sin perder trazabilidad del input original
4. el porcentaje final debe acotarse entre 0 y 100

## Estados y errores

1. corrección completada
2. corrección parcial
3. formato de respuesta inválido
4. ejercicio no encontrado
5. intento inconsistente
6. error de evaluación

## Criterios de aceptación

### CA-001 Corrección de opción múltiple
**Dado** un ejercicio de opción múltiple  
**Cuando** el estudiante envía la opción correcta  
**Entonces** el sistema lo evalúa como correcto.

### CA-002 Score proporcional en completar huecos
**Dado** un ejercicio con varios huecos  
**Cuando** el estudiante acierta solo una parte  
**Entonces** el sistema puede asignar score parcial según configuración.

### CA-003 Tolerancia en traducción
**Dado** un ejercicio de traducción  
**Cuando** el estudiante envía una respuesta cercana y funcionalmente válida  
**Entonces** el sistema debe poder reconocerla como parcialmente o totalmente válida según reglas configuradas.

### CA-004 Tolerancia en dictado
**Dado** un ejercicio de dictado  
**Cuando** el estudiante comete diferencias menores no sustanciales  
**Entonces** el sistema debe aplicar tolerancia parcial razonable.

### CA-005 Porcentaje total del intento
**Dado** un conjunto de ejercicios respondidos  
**Cuando** finaliza la corrección  
**Entonces** el sistema devuelve un porcentaje total del intento.

## Casos límite

1. Respuesta vacía en ejercicio textual.
2. Varias respuestas válidas equivalentes.
3. Texto correcto con puntuación distinta.
4. Audio entendido parcialmente y respuesta incompleta.
5. Ejercicios mixtos con distintos pesos.

## Notas técnicas

1. Se recomienda encapsular la lógica de corrección en `lib/scoring`.
2. Las respuestas originales y normalizadas deben poder conservarse.
3. La tolerancia puede modelarse en `configJson` por ejercicio.
4. Debe evitarse acoplar la UI a la lógica de corrección.
5. La primera versión debe priorizar reglas deterministas y auditables.

## Definición de terminado

Se considerará terminado este módulo cuando:

1. Existan reglas claras de corrección para todos los tipos del MVP.
2. La tolerancia parcial esté definida de forma utilizable por ingeniería.
3. Un agente pueda implementar la lógica de evaluación sin ambigüedad crítica.

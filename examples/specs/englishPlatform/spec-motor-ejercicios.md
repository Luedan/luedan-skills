# Spec: Motor de ejercicios

## Objetivo

Definir el comportamiento funcional y técnico base del módulo de ejercicios, incluyendo tipos soportados, estructura de interacción, captura de respuestas y criterios de integración con corrección y progreso.

## Alcance

Incluye:

1. Tipos de ejercicios del MVP.
2. Estructura funcional de cada tipo.
3. Captura de respuestas del usuario.
4. Reglas mínimas de presentación.
5. Integración con evaluación posterior.

## Fuera de alcance

1. Algoritmo detallado de scoring.
2. Dashboard de progreso.
3. Analítica avanzada de respuesta.
4. Editor administrativo completo de ejercicios.

## Dependencias

1. `spec-modelo-datos-plataforma.md`
2. `spec-visor-leccion-y-audio.md`
3. `spec-plataforma-aprender-ingles.md`

## Actores y permisos

### student
- puede responder ejercicios de una lección publicada
- puede reintentarlos ilimitadamente

### admin
- define y publica ejercicios desde otros módulos

## Rutas / pantallas

Principalmente dentro de:

1. `/app/lessons/[lessonSlug]`

## Componentes UI

1. Bloque de ejercicio de opción múltiple.
2. Bloque de completar huecos.
3. Bloque de traducción.
4. Bloque de listening.
5. Bloque de dictado.
6. Botones de enviar o validar intento.
7. Estados visuales de respuesta.

## Server Actions / operaciones servidor

### 1. `submitLessonAttempt`
Responsabilidades:
- recibir respuestas del estudiante
- validar payload
- persistir intento
- delegar corrección
- devolver resultado resumido

### 2. `saveExerciseAttempt` o persistencia integrada
Responsabilidades:
- guardar respuestas por ejercicio o por intento grupal

## Modelo de datos afectado

1. `Exercise`
2. `ExerciseOption`
3. `ExerciseAttempt`
4. `LessonAttempt`
5. `LessonProgress`

## Tipos de ejercicio soportados

### 1. Opción múltiple
- el usuario selecciona una opción válida entre varias

### 2. Completar huecos
- el usuario completa uno o varios espacios faltantes

### 3. Traducción
- el usuario introduce una respuesta textual
- la corrección debe contemplar tolerancia parcial

### 4. Listening
- el usuario escucha un audio y responde una pregunta asociada

### 5. Dictado
- el usuario escucha un audio y escribe lo entendido
- la corrección debe contemplar tolerancia parcial

## Reglas de negocio

1. Los ejercicios pertenecen a una lección.
2. El usuario solo puede responder ejercicios de lecciones publicadas y accesibles.
3. Deben permitirse reintentos ilimitados.
4. Los ejercicios pueden ser obligatorios o no.
5. La estructura de respuesta debe ser válida según el tipo de ejercicio.
6. Traducción y dictado deben admitir corrección con tolerancia parcial.
7. El resultado de los ejercicios impactará en el progreso de la lección a través de módulos posteriores.

## Validaciones

1. el payload de respuestas debe corresponder a ejercicios existentes
2. los ejercicios deben pertenecer a la lección enviada
3. el usuario debe tener acceso a la lección
4. las respuestas obligatorias deben estar presentes si el flujo lo exige
5. el formato de respuesta debe coincidir con el tipo de ejercicio

## Estados y errores

1. ejercicios cargando
2. ejercicio respondido
3. intento enviado
4. respuesta inválida
5. audio no disponible en listening o dictado
6. error al guardar intento

## Criterios de aceptación

### CA-001 Responder opción múltiple
**Dado** un estudiante autenticado en una lección publicada  
**Cuando** selecciona una opción válida y envía su respuesta  
**Entonces** el sistema registra el intento.

### CA-002 Responder completar huecos
**Dado** un estudiante autenticado  
**Cuando** completa los huecos y envía la respuesta  
**Entonces** el sistema registra el intento para corrección.

### CA-003 Responder traducción
**Dado** un estudiante autenticado  
**Cuando** envía una traducción textual  
**Entonces** el sistema registra la respuesta  
**Y** la prepara para evaluación con tolerancia parcial.

### CA-004 Responder listening
**Dado** un estudiante autenticado  
**Cuando** escucha un audio y responde la pregunta asociada  
**Entonces** el sistema registra el intento.

### CA-005 Responder dictado
**Dado** un estudiante autenticado  
**Cuando** escucha un audio y escribe su respuesta  
**Entonces** el sistema registra el intento  
**Y** lo prepara para evaluación con tolerancia parcial.

## Casos límite

1. Envío parcial de ejercicios.
2. Doble envío del mismo intento.
3. Audio no reproducible en ejercicios auditivos.
4. Respuestas vacías en ejercicios obligatorios.
5. Payload manipulado con exerciseIds ajenos a la lección.

## Notas técnicas

1. Se recomienda modelar la configuración específica de cada ejercicio en `configJson`.
2. El submit de una lección puede agrupar respuestas en una única Server Action.
3. La UI debe componerse con bloques reutilizables por tipo de ejercicio.
4. Los ejercicios de audio deben reutilizar un componente de reproductor común.
5. La validación de payload debe centralizarse con **Zod**.

## Definición de terminado

Se considerará terminado este módulo cuando:

1. Estén definidos los tipos de ejercicios del MVP.
2. Las estructuras de respuesta esperadas estén claras.
3. Un agente pueda implementar la capa de interacción de ejercicios sin ambigüedad crítica.

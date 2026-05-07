# Spec: Testing y criterios de QA

## Objetivo

Definir el alcance mínimo de pruebas funcionales y técnico-funcionales necesarias para validar la plataforma por módulos y asegurar que los agentes de código entreguen incrementos verificables.

## Alcance

Incluye:

1. Criterios mínimos de validación por módulo.
2. Tipos de pruebas recomendadas.
3. Cobertura funcional prioritaria.
4. Casos críticos de regresión.
5. Definición de entregable verificable por agente.

## Fuera de alcance

1. Estrategia enterprise de testing.
2. Performance testing avanzado.
3. Pentesting.
4. Matrices exhaustivas de compatibilidad cross-browser.

## Dependencias

1. Todas las mini-specs funcionales principales del módulo englishPlatform.

## Actores y permisos

### QA
- valida flujos principales y regresiones

### developer / coding agent
- implementa módulos con cobertura verificable

### admin y student
- actores de referencia para escenarios de prueba

## Rutas / pantallas

Transversal a toda la plataforma.

## Componentes UI

Aplica a formularios, tablas, dashboard, visor de lección, motor de ejercicios y panel admin.

## Server Actions / operaciones servidor

Debe validarse especialmente:

1. auth y registro con código
2. consumo de códigos
3. CRUD admin
4. envío y corrección de intentos
5. progreso
6. auditoría

## Modelo de datos afectado

Todo el dominio, especialmente:

1. `User`
2. `AccessCode`
3. `Lesson`
4. `Exercise`
5. `LessonAttempt`
6. `LessonProgress`
7. `AuditLog`

## Reglas de negocio a cubrir sí o sí

1. registro solo con código válido
2. código de un solo uso
3. acceso vitalicio vinculado a cuenta
4. contenido solo visible si está publicado
5. reintentos ilimitados
6. completitud por porcentaje mínimo
7. tolerancia parcial en traducción y dictado
8. auditoría de acciones admin
9. aislamiento de permisos por rol

## Tipos de pruebas recomendadas

### 1. Unitarias
- validadores
- scoring
- permisos

### 2. Integración
- Server Actions
- persistencia de intentos
- actualización de progreso
- auditoría

### 3. End to end
- registro con código
- login y redirecciones
- recorrido estudiante
- acciones admin principales

## Criterios de aceptación

### CA-001 Registro verificado
**Dado** un entorno de prueba con un código activo  
**Cuando** se ejecuta el flujo de registro  
**Entonces** debe verificarse que el usuario se crea y el código se consume.

### CA-002 Reutilización bloqueada
**Dado** un código ya consumido  
**Cuando** se intenta reutilizar en un nuevo registro  
**Entonces** la prueba debe confirmar que el sistema lo rechaza.

### CA-003 Flujo de progreso
**Dado** una lección con ejercicios  
**Cuando** un estudiante completa intentos con distintos scores  
**Entonces** debe verificarse el estado final y el mejor score persistido.

### CA-004 Seguridad por rol
**Dado** usuarios con roles distintos  
**Cuando** intentan acceder a rutas o acciones no permitidas  
**Entonces** la prueba debe confirmar que el acceso es bloqueado.

### CA-005 Auditoría verificable
**Dado** una acción admin crítica  
**Cuando** se ejecuta correctamente  
**Entonces** la prueba debe confirmar la creación del registro de auditoría.

## Casos límite

1. doble submit de registro
2. concurrencia en consumo de código
3. audio faltante
4. lección despublicada
5. cambio del porcentaje mínimo con progresos previos

## Notas técnicas

1. Se recomienda combinar tests unitarios, integración y e2e.
2. Los agentes de código deberían entregar junto al módulo al menos pruebas mínimas del flujo principal y del principal caso de error.
3. Las fixtures de prueba deben alinearse con el módulo de seeding y datos iniciales.
4. Las Server Actions críticas deben tener pruebas de validación y autorización.

## Definición de terminado

Se considerará terminado este módulo cuando:

1. Exista una estrategia mínima verificable por módulo.
2. Estén definidos los casos críticos de negocio a cubrir.
3. Un agente pueda saber qué debe probar para considerar un incremento aceptable.

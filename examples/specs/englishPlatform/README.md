# English Platform Specs Index

## Objetivo

Este directorio contiene la especificación madre y el conjunto de mini-especificaciones granulares para desarrollar la plataforma web de aprendizaje de inglés por módulos, de forma compatible con trabajo paralelo usando agentes de código.

La idea es que la spec madre defina el producto completo y que las specs hijas permitan ejecutar implementación, validación y QA por partes controladas.

---

## Documento raíz

- [spec-plataforma-aprender-ingles.md](./spec-plataforma-aprender-ingles.md)

Este documento define:
- visión global
- alcance funcional
- reglas de negocio principales
- actores y permisos base
- criterios de aceptación generales

---

## Índice de mini-especificaciones

### 1. Fundaciones
- [spec-modelo-datos-plataforma.md](./spec-modelo-datos-plataforma.md)
- [spec-base-app-y-arquitectura.md](./spec-base-app-y-arquitectura.md)

### 2. Acceso y entrada al sistema
- [spec-auth-registro-con-codigo.md](./spec-auth-registro-con-codigo.md)
- [spec-panel-admin-codigos.md](./spec-panel-admin-codigos.md)

### 3. Catálogo y contenido
- [spec-catalogo-estructura-academica.md](./spec-catalogo-estructura-academica.md)
- [spec-panel-admin-contenido.md](./spec-panel-admin-contenido.md)
- [spec-visor-leccion-y-audio.md](./spec-visor-leccion-y-audio.md)

### 4. Aprendizaje, evaluación y progreso
- [spec-motor-ejercicios.md](./spec-motor-ejercicios.md)
- [spec-correccion-y-tolerancia.md](./spec-correccion-y-tolerancia.md)
- [spec-progreso-y-completitud.md](./spec-progreso-y-completitud.md)
- [spec-dashboard-estudiante.md](./spec-dashboard-estudiante.md)

### 5. Operación administrativa
- [spec-panel-admin-usuarios.md](./spec-panel-admin-usuarios.md)
- [spec-auditoria-administrativa.md](./spec-auditoria-administrativa.md)
- [spec-metricas-operativas.md](./spec-metricas-operativas.md)

### 6. Transversales
- [spec-permisos-seguridad-funcional.md](./spec-permisos-seguridad-funcional.md)
- [spec-estados-errores-y-mensajeria.md](./spec-estados-errores-y-mensajeria.md)
- [spec-seeding-y-datos-iniciales.md](./spec-seeding-y-datos-iniciales.md)
- [spec-testing-y-criterios-de-qa.md](./spec-testing-y-criterios-de-qa.md)

---

## Orden recomendado de implementación

### Fase 1 — Base estructural
1. [spec-modelo-datos-plataforma.md](./spec-modelo-datos-plataforma.md)
2. [spec-base-app-y-arquitectura.md](./spec-base-app-y-arquitectura.md)
3. [spec-permisos-seguridad-funcional.md](./spec-permisos-seguridad-funcional.md)
4. [spec-seeding-y-datos-iniciales.md](./spec-seeding-y-datos-iniciales.md)

### Fase 2 — Acceso al sistema
5. [spec-auth-registro-con-codigo.md](./spec-auth-registro-con-codigo.md)
6. [spec-panel-admin-codigos.md](./spec-panel-admin-codigos.md)

### Fase 3 — Contenido y navegación académica
7. [spec-catalogo-estructura-academica.md](./spec-catalogo-estructura-academica.md)
8. [spec-panel-admin-contenido.md](./spec-panel-admin-contenido.md)
9. [spec-visor-leccion-y-audio.md](./spec-visor-leccion-y-audio.md)

### Fase 4 — Aprendizaje y evaluación
10. [spec-motor-ejercicios.md](./spec-motor-ejercicios.md)
11. [spec-correccion-y-tolerancia.md](./spec-correccion-y-tolerancia.md)
12. [spec-progreso-y-completitud.md](./spec-progreso-y-completitud.md)
13. [spec-dashboard-estudiante.md](./spec-dashboard-estudiante.md)

### Fase 5 — Operación y observabilidad
14. [spec-panel-admin-usuarios.md](./spec-panel-admin-usuarios.md)
15. [spec-auditoria-administrativa.md](./spec-auditoria-administrativa.md)
16. [spec-metricas-operativas.md](./spec-metricas-operativas.md)
17. [spec-estados-errores-y-mensajeria.md](./spec-estados-errores-y-mensajeria.md)
18. [spec-testing-y-criterios-de-qa.md](./spec-testing-y-criterios-de-qa.md)

---

## Dependencias críticas entre specs

### Base para casi todo
- `spec-modelo-datos-plataforma.md`
- `spec-base-app-y-arquitectura.md`
- `spec-permisos-seguridad-funcional.md`

### Auth depende de
- modelo de datos
- base app
- permisos

### Contenido depende de
- modelo de datos
- base app

### Ejercicios y progreso dependen de
- modelo de datos
- contenido
- visor de lección

### Auditoría y métricas dependen de
- paneles admin
- progreso
- modelo de datos

---

## Recomendación de reparto entre agentes de código

### Agente A — Plataforma base
- modelo de datos
- base app
- permisos
- seeding

### Agente B — Auth y acceso
- auth con código
- panel admin de códigos

### Agente C — Contenido
- catálogo académico
- panel admin de contenido
- visor de lección y audio

### Agente D — Aprendizaje
- motor de ejercicios
- corrección y tolerancia
- progreso y completitud
- dashboard estudiante

### Agente E — Operación
- panel admin de usuarios
- auditoría
- métricas
- estados/errores/mensajería
- QA/testing

---

## Recomendaciones prácticas para implementación

1. Empezar por una **vertical funcional mínima**:
   - registro con código
   - login
   - catálogo
   - una lección
   - un tipo de ejercicio
   - progreso básico

2. Fijar primero decisiones técnicas compartidas:
   - PostgreSQL
   - Prisma
   - Auth.js
   - Zod
   - Server Actions

3. Evitar que agentes diferentes definan por separado:
   - nombres de tablas
   - contratos de Server Actions
   - estados de negocio
   - convenciones de rutas

4. Tratar estas specs como fuente de verdad antes de generar código.

5. Exigir que cada entrega de un agente incluya:
   - implementación del módulo
   - validaciones
   - manejo de errores
   - pruebas mínimas

---

## Recomendaciones siguientes

### Opción 1 — Convertir specs en backlog ejecutable
Transformar estas specs en:
- épicas
- historias de usuario
- tareas técnicas
- criterios de terminado por ticket

### Opción 2 — Crear un roadmap de implementación
Preparar un documento tipo:
- MVP fase 1
- MVP fase 2
- hardening
- admin y métricas

### Opción 3 — Empezar implementación técnica
Orden recomendado:
1. modelo de datos
2. base app
3. auth con código
4. panel admin de códigos

### Opción 4 — Crear prompts para agentes de código
Generar prompts específicos por módulo para que cada agente implemente respetando:
- la spec madre
- la mini-spec del módulo
- las dependencias técnicas
- las reglas de QA

---

## Recomendación principal actual

El siguiente paso con mayor valor sería crear uno de estos dos entregables:

1. **`IMPLEMENTATION_PLAN.md`** con fases, dependencias y orden real de desarrollo.
2. **Prompts por módulo para agentes de código** listos para ejecutar.

---

## Estado del trabajo actual

Estado: **base de especificación granular completada**.

Esta carpeta ya permite:
- arrancar refinamiento técnico
- dividir trabajo entre agentes
- estimar módulos
- preparar backlog
- comenzar implementación por fases

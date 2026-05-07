# Spec: Seeding y datos iniciales

## Objetivo

Definir el conjunto mínimo de datos iniciales necesarios para levantar, probar y demostrar la plataforma en entornos de desarrollo, QA o staging.

## Alcance

Incluye:

1. Usuarios base.
2. Códigos iniciales.
3. Curso único.
4. Niveles de ejemplo.
5. Lecciones de ejemplo.
6. Ejercicios de ejemplo.
7. Datos mínimos de auditoría o progreso si resultan útiles.

## Fuera de alcance

1. Datos masivos realistas de producción.
2. Migraciones finales.
3. Fixtures avanzados por entorno enterprise.

## Dependencias

1. `spec-modelo-datos-plataforma.md`
2. `spec-motor-ejercicios.md`
3. `spec-auth-registro-con-codigo.md`

## Actores y permisos

Aplica a desarrollo, QA y administración técnica.

## Rutas / pantallas

Impacta transversalmente en los módulos al facilitar pruebas.

## Componentes UI

No aplica de forma directa.

## Server Actions / operaciones servidor

No requiere Server Actions como comportamiento de usuario final; se orienta a scripts o utilidades de inicialización.

## Modelo de datos afectado

1. `User`
2. `AccessCode`
3. `Course`
4. `Level`
5. `Lesson`
6. `LessonAudio`
7. `Exercise`
8. `ExerciseOption`

## Datos iniciales mínimos recomendados

1. un admin inicial
2. uno o dos estudiantes de prueba
3. varios códigos activos no consumidos
4. al menos un código consumido para probar casos históricos
5. un curso único `aprender-ingles`
6. al menos dos niveles de ejemplo
7. al menos dos lecciones por nivel
8. ejercicios de todos los tipos del MVP
9. al menos un audio asociado a una lección y a un ejercicio auditivo

## Reglas de negocio

1. Los datos iniciales deben permitir recorrer los flujos principales del producto.
2. Deben incluir ejemplos suficientes para probar catálogo, lecciones, ejercicios, admin y auditoría.
3. No deben introducir contradicciones con reglas de negocio reales.
4. Deben ser reproducibles de forma consistente.

## Validaciones

1. integridad relacional válida
2. slugs únicos
3. códigos únicos
4. datos suficientes para pruebas básicas de cada módulo

## Estados y errores

1. seed ejecutado correctamente
2. seed parcial
3. conflicto por datos ya existentes
4. error de integridad en seed

## Criterios de aceptación

### CA-001 Datos mínimos funcionales
**Dado** un entorno nuevo  
**Cuando** se aplica el seeding inicial  
**Entonces** el sistema dispone de datos suficientes para probar los flujos principales.

### CA-002 Cobertura del MVP
**Dado** el conjunto de datos iniciales  
**Cuando** QA o desarrollo revisa el entorno  
**Entonces** existe al menos un caso navegable para cada tipo de ejercicio del MVP.

### CA-003 Datos administrativos iniciales
**Dado** un entorno de prueba  
**Cuando** se completa la inicialización  
**Entonces** existe al menos un administrador capaz de acceder al panel.

## Casos límite

1. Reejecución del seed.
2. Seed parcial sobre base con datos previos.
3. Inconsistencia entre course, levels y lessons generadas.

## Notas técnicas

1. Se recomienda implementar seeding reproducible desde Prisma o scripts equivalentes.
2. Deben definirse valores fácilmente reconocibles para cuentas y códigos de prueba.
3. Los datos iniciales deben ser suficientes pero no excesivos.

## Definición de terminado

Se considerará terminado este módulo cuando:

1. Exista una definición clara de los datos mínimos iniciales.
2. Se cubran los flujos principales del MVP.
3. Un agente pueda preparar scripts o fixtures sin ambigüedad crítica.

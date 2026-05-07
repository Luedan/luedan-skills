# Spec: Métricas operativas

## Objetivo

Definir las métricas operativas mínimas que el panel administrativo debe poder consultar para entender uso de la plataforma, consumo de códigos, actividad de aprendizaje y avance general del producto.

## Alcance

Incluye:

1. Métricas de acceso.
2. Métricas de códigos.
3. Métricas de progreso.
4. Métricas de actividad básica.
5. Visualización administrativa resumida.

## Fuera de alcance

1. BI avanzado.
2. Cohortes complejas.
3. Predicción o machine learning.
4. Exportación enterprise.

## Dependencias

1. `spec-modelo-datos-plataforma.md`
2. `spec-auditoria-administrativa.md`
3. `spec-progreso-y-completitud.md`

## Actores y permisos

### admin
- puede consultar métricas operativas

### student
- no puede acceder a este módulo

## Rutas / pantallas

1. `/admin/metrics`

## Componentes UI

1. Cards de KPIs.
2. Tablas resumidas.
3. Filtros por período si se implementan.
4. Bloques de métricas por dominio.

## Server Actions / operaciones servidor

### 1. `getOperationalMetrics`
Responsabilidades:
- devolver métricas agregadas del sistema

### 2. `getCodeMetrics`
### 3. `getLearningMetrics`

## Modelo de datos afectado

1. `User`
2. `AccessCode`
3. `LessonProgress`
4. `LessonAttempt`
5. `ExerciseAttempt`

## Métricas mínimas

1. número total de usuarios
2. número de cuentas creadas con código
3. número de códigos activos
4. número de códigos consumidos
5. tasa de activación de códigos
6. número de lecciones completadas
7. promedio de score por lección o global simplificado
8. número medio de reintentos por lección
9. actividad reciente básica

## Reglas de negocio

1. Las métricas deben ser solo visibles para admins.
2. Deben priorizar utilidad operativa sobre complejidad analítica.
3. Deben basarse en datos persistidos del sistema.
4. No deben exponer datos personales innecesarios en agregados.

## Validaciones

1. acceso solo admin
2. filtros de fecha válidos si se implementan
3. agregaciones consistentes con datos persistidos

## Estados y errores

1. métricas cargadas
2. sin datos suficientes
3. error al calcular métricas
4. acceso denegado

## Criterios de aceptación

### CA-001 Acceso restringido
**Dado** un administrador autenticado  
**Cuando** accede a `/admin/metrics`  
**Entonces** el sistema muestra métricas operativas básicas.

### CA-002 Métricas de códigos
**Dado** que existen códigos en el sistema  
**Cuando** el administrador consulta métricas  
**Entonces** el sistema muestra cuántos están activos y cuántos consumidos.

### CA-003 Métricas de aprendizaje
**Dado** que existen intentos y progreso registrados  
**Cuando** el administrador consulta métricas  
**Entonces** el sistema muestra indicadores de avance y actividad.

## Casos límite

1. Plataforma sin datos iniciales.
2. Datos incompletos por módulos aún no desplegados.
3. Agregados pesados con crecimiento de volumen.

## Notas técnicas

1. La primera versión puede resolver métricas con consultas agregadas simples.
2. La UI debe evitar gráficos complejos si no aportan valor inicial.
3. Debe haber separación entre agregados de negocio y listados detallados.

## Definición de terminado

Se considerará terminado este módulo cuando:

1. Exista un set mínimo de métricas definidas.
2. La visibilidad admin esté clara.
3. Un agente pueda implementar el panel operativo sin ambigüedad crítica.

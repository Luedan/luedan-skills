# Spec: Estados, errores y mensajería

## Objetivo

Unificar la forma en que la plataforma comunica estados de carga, éxito, error, vacío y restricción al usuario, tanto en experiencia estudiante como en panel administrativo.

## Alcance

Incluye:

1. Estados de carga.
2. Estados vacíos.
3. Mensajes de éxito.
4. Mensajes de error.
5. Mensajes de acceso restringido.
6. Consistencia de feedback entre módulos.

## Fuera de alcance

1. Copywriting final de marketing.
2. Localización avanzada multiidioma.
3. Sistema de notificaciones push.

## Dependencias

1. `spec-base-app-y-arquitectura.md`
2. `spec-auth-registro-con-codigo.md`
3. `spec-visor-leccion-y-audio.md`
4. `spec-panel-admin-codigos.md`
5. `spec-panel-admin-contenido.md`

## Actores y permisos

Aplica a:

1. visitor
2. student
3. admin

## Rutas / pantallas

Transversal a toda la plataforma.

## Componentes UI

1. Alertas inline.
2. Toasts.
3. Estados vacíos.
4. Skeletons de carga.
5. Mensajes de error de formulario.
6. Bloques de error recuperable.

## Server Actions / operaciones servidor

Todas las Server Actions deben devolver respuestas consistentes para que la UI pueda renderizar:

1. éxito
2. error de validación
3. error de negocio
4. error inesperado

## Modelo de datos afectado

No introduce entidades propias; aplica sobre toda la plataforma.

## Reglas de negocio

1. Los mensajes deben ser claros y accionables.
2. Los errores de negocio deben diferenciarse de errores técnicos cuando sea posible.
3. Los formularios deben mostrar validación cercana al campo y feedback global si aplica.
4. Los mensajes de acceso denegado no deben exponer información sensible.
5. Las acciones exitosas en admin deben confirmarse visualmente.

## Validaciones

1. consistencia de formato de respuesta desde Server Actions
2. mapeo uniforme de errores de validación
3. fallback visual para errores inesperados

## Estados y errores mínimos

### Auth
1. registro exitoso
2. email ya registrado
3. código inválido
4. código ya utilizado
5. login incorrecto

### Contenido
6. contenido cargando
7. sin contenido disponible
8. lección no encontrada
9. audio no disponible

### Ejercicios
10. intento guardado
11. respuesta inválida
12. porcentaje insuficiente
13. lección completada

### Admin
14. cambio guardado correctamente
15. acción auditada
16. acceso denegado

## Criterios de aceptación

### CA-001 Error de validación visible
**Dado** un formulario con datos inválidos  
**Cuando** el usuario lo envía  
**Entonces** el sistema muestra errores comprensibles cerca de los campos afectados.

### CA-002 Confirmación de éxito
**Dado** una acción administrativa exitosa  
**Cuando** el sistema guarda el cambio  
**Entonces** muestra una confirmación visual coherente.

### CA-003 Estado vacío útil
**Dado** una pantalla sin datos disponibles  
**Cuando** el usuario accede a ella  
**Entonces** el sistema muestra un estado vacío comprensible y no ambiguo.

### CA-004 Error no sensible
**Dado** un acceso no autorizado o fallo técnico  
**Cuando** el sistema responde con error  
**Entonces** no expone información sensible al usuario final.

## Casos límite

1. Error de servidor sin detalle funcional.
2. Múltiples errores concurrentes en una misma pantalla.
3. Acciones duplicadas por doble click.
4. Pérdida de conectividad durante una mutación.

## Notas técnicas

1. Se recomienda una convención uniforme de resultado para Server Actions.
2. Los formularios deben apoyarse en componentes reutilizables de error/feedback.
3. Deben existir componentes de estado vacío y error reutilizables.
4. Los toasts no deben sustituir validaciones inline importantes.

## Definición de terminado

Se considerará terminado este módulo cuando:

1. Existan patrones uniformes de feedback definidos.
2. Los estados mínimos de éxito, carga, error y vacío estén cubiertos.
3. Un agente pueda implementar UX de feedback sin ambigüedad crítica.

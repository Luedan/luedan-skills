# Spec: Plataforma de Gestión Operativa de Tareas

> Versión: 1 · Fecha: 2026-05-07

## Contexto

La organización necesita una plataforma interna para que líderes, PMs y otros roles autorizados puedan registrar tareas, asignarlas a desarrolladores, hacer seguimiento operativo, controlar tiempos, visualizar bloqueos y entender la carga de trabajo por proyecto, equipo, desarrollador y líder.

La plataforma debe combinar gestión operativa de tareas con prácticas ágiles básicas, sin depender inicialmente de herramientas externas como Jira, GitHub, Slack, Teams o correo.

## Historia de Usuario

Como **jefe de operaciones**, quiero una plataforma donde los líderes o PMs puedan registrar tareas, asignarlas a desarrolladores, hacer seguimiento de tiempos, entregas y bloqueos, para tener visibilidad clara de quién tiene asignación, quién no, cómo avanzan los proyectos y dónde existen desviaciones operativas.

## Objetivo

Centralizar la gestión operativa de tareas de desarrollo, permitiendo:

- Crear proyectos, equipos y personas.
- Configurar roles y permisos de forma parametrizable, incluyendo un usuario `root` permanente con acceso total.
- Crear, asignar, reasignar y monitorear tareas.
- Permitir que el desarrollador actualice el estado, agregue pasos internos, reporte bloqueos y entregue tareas.
- Medir tiempos manuales y automáticos de forma separada y transparente.
- Comparar tiempos estimados contra tiempos reales para identificar desviaciones.
- Consultar dashboards y reportes por proyecto, equipo, desarrollador, líder y personas a cargo.

## Alcance

Incluye:

1. Gestión de roles dinámicos y permisos por acción.
2. Roles predefinidos iniciales: jefe de operaciones, PM/líder y desarrollador.
3. Usuario `root` con permisos totales, no eliminable ni desactivable.
4. Gestión de proyectos, equipos y usuarios.
5. Creación, asignación, reasignación y seguimiento de tareas.
6. Estados operativos de tareas.
7. Ítems, pasos o información interna agregada por el desarrollador dentro de una tarea asignada.
8. Bloqueos con motivo obligatorio.
9. Entregas, revisiones, aprobaciones y reaperturas.
10. Registro manual de tiempo.
11. Medición automática de tiempos por ciclo de vida de la tarea.
12. Tiempos estimados en una unidad configurable, usando horas como unidad inicial.
13. Cálculo de desviaciones por porcentaje y por valor absoluto.
14. Dashboards y reportes internos.
15. Historial y auditoría funcional de cambios.
16. Diseño responsive, priorizando web/escritorio.
17. Internacionalización desde el inicio mediante textos parametrizables por idioma.

## Fuera de Alcance

- Integraciones externas con Jira, GitHub, Slack, Teams, correo u otras herramientas.
- Cronómetro en vivo tipo iniciar/detener.
- Scrum avanzado completo como gestión formal de ceremonias, capacity planning detallado o burndown avanzado obligatorio.
- Evaluaciones automáticas definitivas de desempeño individual.
- Gestión de nómina o facturación por horas.
- Control de código fuente.

## Actores

| Actor | Descripción |
|---|---|
| Jefe de operaciones | Consulta visibilidad global, dashboards, reportes, bloqueos, desviaciones y disponibilidad. Sus acciones editables dependen de permisos. |
| PM / líder | Rol predefinido que normalmente crea, asigna y revisa tareas, sujeto a permisos configurados. |
| Desarrollador | Consulta tareas asignadas, actualiza estados, agrega pasos internos, registra tiempos, reporta bloqueos y entrega resultados. |
| Administrador | Usuario con permisos para configurar roles, permisos, usuarios, proyectos y equipos. |
| Root | Usuario permanente con permisos totales sobre el sistema. No puede ser eliminado, desactivado ni degradado. |
| Rol personalizado | Cualquier rol creado por configuración, con acciones permitidas según parametrización. |

## Precondiciones

- El usuario debe estar autenticado para acceder a la plataforma.
- El usuario debe tener un rol activo.
- Debe existir siempre al menos un usuario `root` activo y protegido por el sistema.
- Las acciones disponibles dependen de los permisos configurados para el rol del usuario.
- Deben existir proyectos y usuarios activos antes de asignar tareas.
- Para reportes por equipo, las personas deben estar asociadas a uno o más equipos.

## Criterios de Aceptación

### Roles y permisos

- **AC-1**: Un usuario con permiso de administración puede crear, editar y desactivar roles.
- **AC-2**: Un usuario con permiso de administración puede asignar acciones permitidas a cada rol.
- **AC-3**: El sistema valida los permisos del rol antes de permitir acciones sensibles.
- **AC-4**: El sistema bloquea y registra los intentos de acción realizados sin permiso.
- **AC-47**: El sistema cuenta siempre con un usuario `root` con permisos totales, que no puede ser eliminado, desactivado ni degradado.

### Proyectos, equipos y personas

- **AC-5**: Un usuario autorizado puede registrar proyectos.
- **AC-6**: Un usuario autorizado puede registrar equipos asociados a proyectos.
- **AC-7**: Un usuario autorizado puede asociar personas a uno o varios equipos.
- **AC-8**: Una tarea puede asociarse a un proyecto y opcionalmente a un equipo.
- **AC-9**: El sistema permite filtrar tareas y reportes por proyecto y equipo.
- **AC-48**: Un usuario puede pertenecer simultáneamente a varios equipos y proyectos sin restricción funcional por defecto.

### Creación de tareas

- **AC-10**: Un usuario autorizado puede crear una tarea indicando título, descripción, proyecto, prioridad, fecha objetivo y tiempo estimado en la unidad configurada.
- **AC-11**: El sistema valida los campos obligatorios antes de guardar una tarea.
- **AC-12**: Una tarea nueva queda en estado `Sin asignar` si no tiene desarrollador asignado, o en `Pendiente` si se crea con responsable.

### Asignación y reasignación

- **AC-13**: Un usuario autorizado puede asignar una tarea a un usuario activo y elegible para recibir tareas.
- **AC-14**: Un usuario autorizado puede reasignar una tarea y el sistema registra el cambio en el historial.
- **AC-15**: El sistema impide asignar tareas a usuarios inactivos, inexistentes o no elegibles.

### Gestión del desarrollador

- **AC-16**: El desarrollador puede consultar sus tareas asignadas.
- **AC-17**: El desarrollador puede cambiar el estado de una tarea asignada según las transiciones permitidas.
- **AC-18**: El desarrollador puede agregar pasos, ítems o información operativa dentro de una tarea asignada.
- **AC-19**: Cada ítem interno de tarea registra descripción, estado, usuario creador y fecha de creación.
- **AC-20**: Los ítems internos pueden marcarse como `Pendiente`, `En progreso` o `Completado`.

### Bloqueos

- **AC-21**: El desarrollador puede marcar una tarea asignada como `Bloqueada` indicando un motivo obligatorio.
- **AC-22**: Cuando una tarea se bloquea, el sistema genera una notificación interna para el líder o responsable configurado.
- **AC-23**: Un usuario autorizado puede desbloquear una tarea y el sistema registra usuario, fecha y comentario del desbloqueo.

### Entrega, revisión y cierre

- **AC-24**: El desarrollador puede marcar una tarea como `Lista para revisión` o `Entregada` incluyendo comentario o evidencia mínima.
- **AC-25**: Solo un usuario cuyo rol tenga el permiso `aprobar entrega` puede aprobar y cerrar definitivamente una tarea.
- **AC-26**: Un usuario con permiso `aprobar entrega` puede reabrir una tarea con observaciones.
- **AC-27**: Una tarea cerrada no puede ser modificada salvo que sea reabierta por un usuario autorizado.

### Tiempos, estimaciones y desviaciones

- **AC-28**: El desarrollador puede registrar manualmente tiempo trabajado en una tarea asignada.
- **AC-29**: El tiempo automático de ejecución inicia cuando la tarea cambia a estado `En progreso`.
- **AC-30**: El sistema registra el tiempo de primera ejecución desde `En progreso` hasta la primera entrega.
- **AC-31**: El sistema registra el tiempo en revisión desde la entrega hasta la aprobación o reapertura.
- **AC-32**: Cada reapertura genera un ciclo de retrabajo medido por separado.
- **AC-33**: El sistema registra el tiempo bloqueado por separado del tiempo de ejecución.
- **AC-34**: El sistema calcula tiempo total de ciclo desde la creación o asignación hasta el cierre final.
- **AC-35**: El sistema calcula desviaciones entre tiempo estimado, tiempo manual registrado, tiempo automático, bloqueos, revisión y retrabajo.
- **AC-36**: El sistema no incluye cronómetro en vivo en la primera versión.
- **AC-49**: Las estimaciones se manejan inicialmente en horas, pero la unidad de estimación debe tener nombre parametrizable.
- **AC-50**: El sistema permite definir desviaciones relevantes usando porcentaje y horas absolutas.

### Dashboards y reportes

- **AC-37**: El dashboard muestra qué personas no tienen tareas activas asignadas.
- **AC-38**: El dashboard muestra cantidad de tareas por desarrollador agrupadas por estado.
- **AC-39**: El dashboard destaca tareas bloqueadas, vencidas, sin asignar, reabiertas y con desviación relevante.
- **AC-40**: Los reportes pueden filtrarse por proyecto, equipo, desarrollador, líder, estado, prioridad y rango de fechas.
- **AC-41**: El sistema permite consultar métricas por líder y por las personas bajo su gestión.
- **AC-42**: Los reportes y dashboards respetan los permisos configurados por rol.

### Historial, responsive e internacionalización

- **AC-43**: El sistema conserva historial de cambios de estado, asignaciones, tiempos, bloqueos, entregas, aprobaciones, comentarios e ítems internos.
- **AC-44**: Cada entrada del historial muestra fecha, usuario, acción y detalle del cambio.
- **AC-45**: La interfaz es responsive, con experiencia principal optimizada para web/escritorio y uso móvil funcional.
- **AC-46**: Los textos visibles de interfaz, mensajes, estados y notificaciones usan llaves de internacionalización desde el inicio.

## Permisos Iniciales Sugeridos por Rol

> Estos permisos son la configuración inicial recomendada. Salvo el usuario `root`, todos los roles y permisos podrán modificarse mediante parametrización.

| Acción / Permiso | Root | Administrador | Jefe de operaciones | PM / líder | Desarrollador |
|---|---|---|---|---|---|
| Administrar roles y permisos | Sí | Sí | No | No | No |
| Administrar usuarios | Sí | Sí | Ver | Ver equipo/proyecto | No |
| Crear/editar proyectos | Sí | Sí | Sí | No | No |
| Crear/editar equipos | Sí | Sí | Sí | Gestionar sus equipos | No |
| Asociar personas a equipos/proyectos | Sí | Sí | Sí | Solo sus equipos/proyectos | No |
| Crear tareas | Sí | Sí | Sí | Sí | No |
| Editar tareas | Sí | Sí | Sí | Tareas de sus proyectos/equipos | Solo información operativa de sus tareas |
| Asignar/reasignar tareas | Sí | Sí | Sí | Tareas de sus proyectos/equipos | No |
| Cambiar estado de tarea | Sí | Sí | Sí | Sí | Solo tareas asignadas |
| Agregar ítems internos/pasos | Sí | Sí | Sí | Sí | Solo tareas asignadas |
| Registrar tiempo manual | Sí | Sí | Sí | Sí | Solo tareas asignadas |
| Marcar bloqueo | Sí | Sí | Sí | Sí | Solo tareas asignadas |
| Desbloquear tareas | Sí | Sí | Sí | Sí | No, salvo permiso explícito |
| Entregar tarea | Sí | Sí | Sí | Sí | Solo tareas asignadas |
| Aprobar entrega / cerrar tarea | Sí | Sí | Sí | Sí | No |
| Reabrir tarea | Sí | Sí | Sí | Sí | No |
| Ver dashboard global | Sí | Sí | Sí | No | No |
| Ver dashboard por proyecto/equipo | Sí | Sí | Sí | Sus proyectos/equipos | No |
| Ver reportes por desarrollador | Sí | Sí | Sí | Personas bajo su gestión | Solo métricas propias, si se habilita |
| Configurar unidad de estimación | Sí | Sí | Sí | No | No |
| Configurar umbrales de desviación | Sí | Sí | Sí | Por proyecto/equipo si se habilita | No |
| Consultar historial | Sí | Sí | Sí | Sus proyectos/equipos | Solo tareas asignadas |

## Criterios de No Aceptación

- El sistema permite ejecutar acciones sensibles sin validar permisos.
- El sistema mezcla tiempo bloqueado, tiempo en revisión y tiempo de ejecución como si fueran el mismo indicador.
- El sistema muestra un desarrollador como disponible cuando tiene tareas activas asignadas.
- El sistema permite cerrar una tarea sin el permiso `aprobar entrega`.
- El sistema permite eliminar, desactivar o degradar el usuario `root`.
- El sistema no registra historial de reasignaciones, entregas, reaperturas o bloqueos.
- El sistema tiene textos de interfaz quemados que impiden internacionalización futura.

## Happy Path

1. Un administrador configura roles y permisos.
2. Un usuario autorizado registra un proyecto y un equipo.
3. Se asocian personas al equipo.
4. Un PM/líder autorizado crea una tarea con tiempo estimado.
5. La tarea se asigna a un desarrollador activo.
6. El desarrollador cambia la tarea a `En progreso`; inicia el tiempo automático de ejecución.
7. El desarrollador agrega pasos internos para organizar el trabajo.
8. El desarrollador registra tiempo manual trabajado.
9. El desarrollador entrega la tarea con comentario o evidencia.
10. Un usuario con permiso `aprobar entrega` revisa y aprueba la tarea.
11. La tarea queda cerrada.
12. El dashboard actualiza métricas, disponibilidad, tiempos y desviaciones.

## Flujos Alternativos

### FA-1: Tarea bloqueada

1. El desarrollador identifica un impedimento.
2. Cambia la tarea a `Bloqueada`.
3. Ingresa motivo obligatorio.
4. El sistema registra inicio de tiempo bloqueado.
5. El líder o responsable recibe notificación interna.
6. Un usuario autorizado desbloquea la tarea con comentario.
7. El sistema cierra el periodo bloqueado y permite continuar el flujo.

### FA-2: Tarea devuelta por revisión

1. El desarrollador entrega la tarea.
2. Un usuario con permiso `aprobar entrega` revisa.
3. La tarea se reabre con observaciones.
4. El sistema conserva la primera entrega.
5. Cuando el desarrollador vuelve a `En progreso`, inicia un nuevo ciclo de retrabajo.
6. La nueva entrega se mide separada de la primera ejecución.

### FA-3: Tarea creada sin responsable

1. Un usuario autorizado crea una tarea sin asignar desarrollador.
2. El sistema la deja en estado `Sin asignar`.
3. La tarea aparece destacada en el dashboard como pendiente de asignación.
4. Un usuario autorizado la asigna posteriormente.

## Sad Paths / Casos de Error

- **SP-1** (cubre AC-10, AC-11): Campos obligatorios incompletos al crear tarea.
  - Disparador: El usuario intenta guardar sin título, proyecto, prioridad, fecha objetivo o tiempo estimado.
  - Respuesta del sistema: No guarda la tarea y marca los campos faltantes.
  - Mensaje al usuario: **MSG-1**.

- **SP-2** (cubre AC-3, AC-4, AC-25, AC-42): Acción sin permiso.
  - Disparador: El usuario intenta crear, asignar, aprobar, cerrar o ver un reporte sin permiso.
  - Respuesta del sistema: Bloquea la acción y registra el intento.
  - Mensaje al usuario: **MSG-2**.

- **SP-3** (cubre AC-3): Sesión expirada o usuario no autenticado.
  - Disparador: El usuario ejecuta una acción con sesión vencida.
  - Respuesta del sistema: Redirige al inicio de sesión sin ejecutar la acción.
  - Mensaje al usuario: **MSG-3**.

- **SP-4** (cubre AC-8, AC-13, AC-14): Recurso inexistente o eliminado.
  - Disparador: Se intenta operar sobre tarea, proyecto, equipo o usuario que ya no existe.
  - Respuesta del sistema: Cancela la acción y solicita refrescar la información.
  - Mensaje al usuario: **MSG-4**.

- **SP-5** (cubre AC-14, AC-17, AC-43, AC-44): Edición concurrente.
  - Disparador: Dos usuarios modifican la misma tarea casi al mismo tiempo.
  - Respuesta del sistema: Conserva la versión más reciente y solicita revisar cambios antes de guardar.
  - Mensaje al usuario: **MSG-5**.

- **SP-6** (cubre AC-24): Entrega enviada dos veces.
  - Disparador: Doble clic, reintento o envío duplicado de entrega.
  - Respuesta del sistema: Procesa una sola entrega y evita duplicados.
  - Mensaje al usuario: **MSG-6**.

- **SP-7** (cubre AC-10, AC-28, AC-37): Error del sistema o dependencia interna no disponible.
  - Disparador: Falla al guardar tarea, tiempo o cargar dashboard.
  - Respuesta del sistema: Muestra error, evita pérdida de datos cuando sea posible y permite reintentar.
  - Mensaje al usuario: **MSG-7**.

- **SP-8** (cubre AC-37, AC-38): Estado vacío.
  - Disparador: No existen tareas, proyectos, equipos o asignaciones para mostrar.
  - Respuesta del sistema: Muestra estado vacío con orientación de siguiente acción.
  - Mensaje al usuario: **MSG-8**.

- **SP-9** (cubre AC-10, AC-28, AC-35, AC-49, AC-50): Valores inválidos o fuera de rango.
  - Disparador: Tiempo estimado negativo, tiempo manual cero, texto excesivamente largo o caracteres inválidos.
  - Respuesta del sistema: No guarda el valor y explica la validación.
  - Mensaje al usuario: **MSG-9**.

- **SP-10** (cubre AC-17, AC-24, AC-25, AC-27): Conflicto de estado.
  - Disparador: Se intenta entregar una tarea cerrada, aprobar una tarea no entregada o modificar una tarea cerrada.
  - Respuesta del sistema: Bloquea la transición inválida.
  - Mensaje al usuario: **MSG-10**.

- **SP-11** (cubre AC-21): Bloqueo sin motivo.
  - Disparador: El desarrollador intenta marcar una tarea como bloqueada sin motivo.
  - Respuesta del sistema: No cambia el estado.
  - Mensaje al usuario: **MSG-11**.

- **SP-12** (cubre AC-13, AC-15): Asignación a usuario inválido.
  - Disparador: Se intenta asignar una tarea a usuario inactivo, inexistente o no elegible.
  - Respuesta del sistema: Impide la asignación.
  - Mensaje al usuario: **MSG-12**.

- **SP-13** (cubre AC-46): Llave de idioma no encontrada.
  - Disparador: Un texto visible no tiene traducción configurada.
  - Respuesta del sistema: Usa texto fallback controlado y registra el incidente para corrección.
  - Mensaje al usuario: **MSG-13**.

- **SP-14** (cubre AC-47): Intento de eliminar, desactivar o degradar el usuario `root`.
  - Disparador: Un usuario intenta borrar, inactivar o quitar permisos totales al usuario `root`.
  - Respuesta del sistema: Bloquea la acción y conserva intacto el usuario `root`.
  - Mensaje al usuario: **MSG-14**.

## Checklist de Descubrimiento de Sad Paths

| Categoría | Resultado |
|---|---|
| Invalid input | Aplica: SP-1, SP-9, SP-11. |
| Authorization | Aplica: SP-2, SP-14. |
| Authentication | Aplica: SP-3. |
| Resource not found | Aplica: SP-4. |
| Concurrency | Aplica: SP-5. |
| Idempotency | Aplica: SP-6. |
| Network / timeout | Aplica: SP-7. |
| Dependency down | Aplica: SP-7. |
| Rate limit / quota | No aplica para MVP; podrá definirse si se agregan límites operativos. |
| Empty state | Aplica: SP-8. |
| Boundary values | Aplica: SP-9. |
| State conflict | Aplica: SP-10. |
| Partial success | Aplica parcialmente a guardado de datos; cubierto por SP-7. |
| Cancel / undo | No aplica como funcionalidad general en MVP; reapertura cubre corrección de entregas. |

## Reglas de Negocio

- **RN-1**: Las acciones del sistema se controlan mediante permisos configurables asociados a roles.
- **RN-2**: Los roles predefinidos iniciales son jefe de operaciones, PM/líder y desarrollador, pero no son los únicos permitidos.
- **RN-3**: La aprobación y cierre de tareas depende del permiso `aprobar entrega`, no de un rol fijo.
- **RN-4**: Un desarrollador puede modificar operativamente solo tareas que tenga asignadas, salvo que su rol tenga permisos adicionales.
- **RN-5**: Una tarea bloqueada siempre requiere motivo obligatorio.
- **RN-6**: Una tarea cerrada no puede modificarse excepto si es reabierta por un usuario autorizado.
- **RN-7**: Toda reasignación debe registrarse en historial.
- **RN-8**: Un desarrollador se considera sin asignación cuando no tiene tareas activas asignadas.
- **RN-9**: Las tareas cerradas, canceladas o aprobadas no cuentan como asignación activa.
- **RN-10**: El tiempo automático de ejecución inicia únicamente cuando la tarea cambia a `En progreso`.
- **RN-11**: El tiempo entre asignación y `En progreso` se registra como espera previa al inicio, no como ejecución del desarrollador.
- **RN-12**: Si una tarea es devuelta, la primera entrega no se sobrescribe.
- **RN-13**: Cada devolución genera un ciclo de retrabajo separado.
- **RN-14**: El tiempo bloqueado y el tiempo en revisión no deben mezclarse con tiempo de ejecución.
- **RN-15**: Las métricas de rendimiento deben mostrar contexto operativo y no emitir evaluación automática definitiva del desempeño individual.
- **RN-16**: Los dashboards y reportes deben respetar permisos de visualización configurados por rol.
- **RN-17**: Las tareas vencidas, bloqueadas, sin asignar, reabiertas o con desviación relevante deben destacarse visualmente.
- **RN-18**: Debe existir siempre un usuario `root` activo, con permisos totales, no eliminable, no desactivable y no degradable.
- **RN-19**: Las estimaciones se manejarán inicialmente en horas, pero la unidad de estimación debe tener nombre configurable para soportar cambios futuros sin rediseño funcional.
- **RN-20**: Una desviación puede considerarse relevante por superar un umbral de porcentaje, un umbral de horas absolutas o ambos.
- **RN-21**: Un usuario puede pertenecer a múltiples equipos y proyectos simultáneamente.

## Validaciones

### Tarea

- Título obligatorio.
- Proyecto obligatorio.
- Prioridad obligatoria.
- Fecha objetivo obligatoria.
- Tiempo estimado obligatorio y mayor a cero.
- Descripción recomendada; puede ser obligatoria según configuración futura.
- Responsable opcional al crear; si no existe, la tarea queda `Sin asignar`.

### Tiempo manual

- Debe ser mayor a cero.
- Debe asociarse a una tarea asignada al usuario o a un usuario con permiso especial.
- Debe registrar fecha, duración y comentario opcional.

### Unidad de estimación

- La unidad inicial será horas.
- El nombre de la unidad debe ser configurable.
- La abreviatura de la unidad debe ser configurable.
- La unidad configurada no debe quedar vacía.

### Umbrales de desviación

- Debe poder configurarse un umbral porcentual.
- Debe poder configurarse un umbral en horas absolutas.
- Una tarea se marca con desviación relevante si supera al menos uno de los umbrales configurados, salvo que se defina una regla más restrictiva por proyecto.

### Bloqueo

- Motivo obligatorio.
- No se permite bloquear una tarea cerrada.

### Entrega

- Debe incluir comentario o evidencia mínima.
- No se permite entregar una tarea bloqueada sin desbloquearla primero.

### Roles y permisos

- El nombre del rol no debe estar vacío.
- Un rol no puede eliminarse si tiene usuarios activos asociados; debe desactivarse o reasignarse primero.
- El usuario `root` no puede eliminarse, desactivarse ni perder permisos totales.

## Mensajes al Usuario

| ID | Contexto | Mensaje |
|---|---|---|
| MSG-1 | SP-1 | Completa los campos obligatorios antes de continuar. |
| MSG-2 | SP-2 | No tienes permisos para realizar esta acción. |
| MSG-3 | SP-3 | Tu sesión expiró. Inicia sesión nuevamente para continuar. |
| MSG-4 | SP-4 | La información ya no está disponible. Actualiza la pantalla e inténtalo de nuevo. |
| MSG-5 | SP-5 | Esta tarea fue actualizada por otro usuario. Revisa la versión más reciente antes de guardar. |
| MSG-6 | SP-6 | La entrega ya fue registrada. No es necesario enviarla nuevamente. |
| MSG-7 | SP-7 | No pudimos completar la acción. Intenta nuevamente en unos minutos. |
| MSG-8 | SP-8 | No hay información disponible para mostrar todavía. |
| MSG-9 | SP-9 | Revisa los valores ingresados. Algunos campos tienen datos inválidos. |
| MSG-10 | SP-10 | La tarea está en un estado que no permite esta acción. |
| MSG-11 | SP-11 | Debes indicar el motivo del bloqueo. |
| MSG-12 | SP-12 | El usuario seleccionado no está activo o no puede recibir tareas. |
| MSG-13 | SP-13 | Se mostrará un texto temporal mientras se corrige la configuración de idioma. |
| MSG-14 | SP-14 | El usuario root es permanente y no puede ser eliminado, desactivado ni degradado. |

## Estados

### Estados funcionales de tarea

- `Sin asignar`
- `Pendiente`
- `En progreso`
- `Bloqueada`
- `Lista para revisión`
- `Entregada`
- `Reabierta`
- `Cerrada`
- `Cancelada`

### Estados de UI

| Estado | Comportamiento esperado |
|---|---|
| Empty | Mostrar mensaje claro cuando no existan tareas, proyectos, equipos o asignaciones. |
| Loading | Mostrar indicador de carga en listados, dashboards y reportes. |
| Success | Mostrar datos, filtros y métricas disponibles según permisos. |
| Error | Mostrar mensaje de error y opción de reintentar. |
| Permission denied | Mostrar mensaje de falta de permisos sin exponer información restringida. |

## Datos Requeridos

### Proyecto

- ID
- Nombre
- Descripción
- Estado
- Fecha de creación

### Equipo

- ID
- Nombre
- Proyecto asociado
- Líder o responsable opcional
- Personas asociadas
- Estado

### Usuario

- ID
- Nombre
- Correo
- Rol o roles
- Estado: activo/inactivo
- Equipos asociados
- Proyectos asociados mediante sus equipos o asignación directa
- Líder asociado, si aplica
- Indicador de usuario root, si aplica

### Rol

- ID
- Nombre
- Descripción
- Estado
- Acciones permitidas
- Indicador de rol protegido, si aplica

### Unidad de estimación

- ID
- Nombre singular
- Nombre plural
- Abreviatura
- Unidad base inicial: horas
- Estado

### Configuración de desviaciones

- ID
- Proyecto o configuración global asociada
- Umbral porcentual
- Umbral en horas absolutas
- Regla de evaluación: porcentaje, horas absolutas o ambos
- Estado

### Tarea

- ID
- Título
- Descripción
- Proyecto
- Equipo opcional
- Prioridad
- Estado
- Líder o responsable
- Desarrollador asignado
- Fecha de creación
- Fecha objetivo
- Tiempo estimado
- Unidad de estimación
- Tiempo manual registrado
- Tiempo automático de ejecución
- Tiempo de primera ejecución
- Tiempo en revisión
- Tiempo de retrabajo
- Tiempo bloqueado
- Tiempo total de ciclo
- Desviación calculada
- Motivo de bloqueo
- Comentario o evidencia de entrega
- Historial

### Ítem interno de tarea

- ID
- Tarea asociada
- Descripción
- Estado
- Usuario creador
- Fecha de creación
- Fecha de actualización

## Dependencias

- Servicio de autenticación interno.
- Base de datos o almacenamiento persistente.
- Servicio interno de notificaciones dentro de la plataforma.
- Motor de internacionalización para textos visibles.

## Métricas / Eventos

| Evento | Cuándo se dispara | Propiedades sugeridas |
|---|---|---|
| `task_created` | Al crear una tarea | `task_id`, `project_id`, `team_id`, `created_by`, `estimated_time` |
| `task_assigned` | Al asignar o reasignar | `task_id`, `previous_user_id`, `new_user_id`, `assigned_by` |
| `task_started` | Al cambiar a `En progreso` | `task_id`, `user_id`, `timestamp` |
| `task_blocked` | Al marcar como bloqueada | `task_id`, `user_id`, `reason`, `timestamp` |
| `task_unblocked` | Al desbloquear | `task_id`, `user_id`, `timestamp` |
| `task_delivered` | Al enviar a revisión o entregar | `task_id`, `user_id`, `delivery_number`, `timestamp` |
| `task_reopened` | Al devolver con observaciones | `task_id`, `reviewer_id`, `reason`, `timestamp` |
| `task_approved` | Al aprobar y cerrar | `task_id`, `approver_id`, `timestamp` |
| `manual_time_logged` | Al registrar tiempo manual | `task_id`, `user_id`, `duration`, `date` |
| `permission_denied` | Al bloquear una acción sin permiso | `user_id`, `action`, `resource_type`, `timestamp` |

## Consideraciones de Seguridad

- Control de acceso por roles y permisos parametrizables.
- Validación de permisos en cada acción sensible.
- Registro de intentos no autorizados.
- Protección del usuario `root` contra eliminación, desactivación o degradación de permisos.
- No exponer datos de proyectos, equipos o usuarios si el rol no tiene permiso de visualización.
- Historial auditable de acciones relevantes.

## Consideraciones de Accesibilidad

- Los estados no deben depender solo del color; deben incluir texto o iconografía con etiqueta.
- Los filtros y tablas deben poder navegarse con teclado.
- Los mensajes de error deben ser claros y estar asociados al campo correspondiente cuando aplique.
- El contraste visual debe ser suficiente para lectura operativa.

## Consideraciones No Funcionales

- **Performance**: Dashboards y listados deben cargar en tiempos razonables para uso operativo diario.
- **Responsive**: La experiencia principal se optimiza para web/escritorio; móvil debe ser funcional con vistas resumidas.
- **Internacionalización**: Idioma inicial español; textos preparados mediante llaves de traducción.
- **Observabilidad**: Registrar eventos clave para auditoría y análisis operativo.
- **Escalabilidad funcional**: Debe soportar múltiples proyectos, equipos, roles y permisos.
- **Usabilidad**: El jefe de operaciones debe poder identificar rápidamente bloqueos, vencimientos, personas sin asignación y desviaciones.

## Escenarios BDD

```gherkin
Feature: Plataforma de gestión operativa de tareas

  # BDD-1 → AC-1, AC-2, AC-3, AC-4, AC-47
  Scenario: Configurar roles y permisos dinámicos
    Given que soy un usuario con permiso de administración
    When creo un rol y le asigno acciones permitidas
    Then el sistema guarda el rol
    And valida esos permisos antes de permitir acciones sensibles
    And bloquea y registra acciones sin permiso
    And mantiene un usuario root permanente con permisos totales

  # BDD-2 → AC-5, AC-6, AC-7, AC-8, AC-9, AC-48
  Scenario: Gestionar proyectos, equipos y personas
    Given que tengo permisos para administrar estructura operativa
    When creo un proyecto, creo un equipo y asocio personas
    Then el sistema permite asociar tareas al proyecto y al equipo
    And permite filtrar tareas y reportes por esas dimensiones
    And permite que un usuario pertenezca a varios equipos y proyectos

  # BDD-3 → AC-10, AC-11, AC-12
  Scenario: Crear una tarea válida
    Given que soy un usuario autorizado para crear tareas
    When ingreso título, descripción, proyecto, prioridad, fecha objetivo y tiempo estimado en la unidad configurada
    Then el sistema crea la tarea
    And asigna estado Sin asignar o Pendiente según exista responsable

  # BDD-4 → AC-13, AC-14, AC-15
  Scenario: Asignar y reasignar una tarea
    Given que existe una tarea y un usuario activo elegible
    When asigno o reasigno la tarea
    Then el sistema actualiza el responsable
    And registra el cambio en el historial
    And bloquea asignaciones a usuarios inválidos

  # BDD-5 → AC-16, AC-17, AC-18, AC-19, AC-20
  Scenario: Desarrollador gestiona una tarea asignada con pasos internos
    Given que soy desarrollador y tengo una tarea asignada
    When consulto la tarea, cambio su estado y agrego ítems internos
    Then el sistema registra los pasos con descripción, estado, creador y fecha
    And permite marcarlos como Pendiente, En progreso o Completado

  # BDD-6 → AC-21, AC-22, AC-23
  Scenario: Bloquear y desbloquear una tarea
    Given que tengo una tarea asignada
    When la marco como Bloqueada con motivo
    Then el sistema registra el bloqueo
    And notifica internamente al responsable
    When un usuario autorizado la desbloquea
    Then el sistema registra usuario, fecha y comentario del desbloqueo

  # BDD-7 → AC-24, AC-25, AC-26, AC-27
  Scenario: Entregar, aprobar o reabrir una tarea
    Given que terminé una tarea asignada
    When la marco como Lista para revisión con evidencia o comentario
    Then un usuario con permiso aprobar entrega puede aprobarla y cerrarla
    Or puede reabrirla con observaciones
    And una tarea cerrada no se modifica salvo reapertura autorizada

  # BDD-8 → AC-28, AC-29, AC-30, AC-31, AC-32, AC-33, AC-34, AC-35, AC-36, AC-49, AC-50
  Scenario: Medir tiempos y desviaciones de una tarea
    Given que una tarea tiene tiempo estimado
    When el desarrollador la cambia a En progreso, registra tiempo manual, la entrega y eventualmente es reabierta
    Then el sistema mide primera ejecución, revisión, retrabajo, bloqueo y ciclo total por separado
    And calcula desviaciones contra el tiempo estimado
    And usa horas como unidad inicial con nombre configurable
    And evalúa desviaciones por porcentaje y horas absolutas
    And no ofrece cronómetro en vivo en la primera versión

  # BDD-9 → AC-37, AC-38, AC-39, AC-40, AC-41, AC-42
  Scenario: Consultar dashboards y reportes operativos
    Given que soy un usuario con permisos de visualización
    When ingreso al dashboard y aplico filtros
    Then veo personas sin asignación, tareas por estado, bloqueos, vencimientos, reaperturas y desviaciones
    And puedo consultar métricas por proyecto, equipo, desarrollador, líder y personas a cargo
    And el sistema respeta mis permisos de rol

  # BDD-10 → AC-43, AC-44
  Scenario: Consultar historial de una tarea
    Given que una tarea tuvo cambios operativos
    When consulto su historial
    Then veo fecha, usuario, acción y detalle de cada cambio relevante

  # BDD-11 → AC-45, AC-46
  Scenario: Usar la plataforma en distintos dispositivos e idiomas futuros
    Given que accedo desde escritorio o móvil
    When uso pantallas, mensajes, estados y notificaciones
    Then la interfaz se adapta de forma funcional
    And los textos visibles provienen de llaves de internacionalización
```

## Mockup ASCII

```text
+--------------------------------------------------------------------------------+
| Plataforma Operativa de Tareas                                                  |
+--------------------------------------------------------------------------------+
| Filtros: Proyecto | Equipo | Líder | Dev | Estado | Prioridad | Fecha          |
+--------------------------------------------------------------------------------+

Resumen operativo
+----------------+----------------+----------------+----------------+----------+
| Pendientes: 12 | En progreso: 8 | Bloqueadas: 3 | Sin asignar: 4 | Vencidas:2|
+----------------+----------------+----------------+----------------+----------+

Disponibilidad por persona
+----------------------+--------------+------------+------------+-------------+
| Persona              | Equipo       | Asignadas  | Bloqueadas | Estado      |
+----------------------+--------------+------------+------------+-------------+
| Ana Pérez            | Backend      | 5          | 1          | Ocupada     |
| Luis Gómez           | Frontend     | 0          | 0          | Sin asignar |
| Carlos Rojas         | Backend      | 3          | 0          | Ocupado     |
+----------------------+--------------+------------+------------+-------------+

Tareas críticas
+--------+----------------------+------------+-------------+------------+--------+
| ID     | Tarea                | Estado     | Responsable | Est/Real   | Desv.  |
+--------+----------------------+------------+-------------+------------+--------+
| T-102  | API de pagos         | Bloqueada  | Ana Pérez   | 8h / 10h   | +2h    |
| T-118  | Login SSO            | Vencida    | Carlos R.   | 6h / 9h    | +3h    |
| T-130  | Reporte financiero   | Sin asignar| —           | 5h / —     | —      |
+--------+----------------------+------------+-------------+------------+--------+
```

## Supuestos Funcionales

- **S-1 [CRÍTICO]**: La plataforma manejará roles dinámicos y permisos parametrizables por acción.
- **S-2 [CRÍTICO]**: Los roles iniciales predefinidos serán jefe de operaciones, PM/líder y desarrollador, además de un usuario `root` permanente con permisos totales.
- **S-3 [CRÍTICO]**: El desarrollador podrá agregar pasos, ítems o información operativa dentro de tareas asignadas.
- **S-4 [CRÍTICO]**: El tiempo automático iniciará cuando la tarea pase a `En progreso`.
- **S-5 [CRÍTICO]**: El sistema separará tiempo manual, primera ejecución, revisión, retrabajo, bloqueo y ciclo total.
- **S-6 [CRÍTICO]**: La aprobación y cierre dependerá del permiso `aprobar entrega`.
- **S-7 [CRÍTICO]**: La plataforma manejará proyectos, equipos y personas asociadas a uno o varios equipos/proyectos.
- **S-8 [CRÍTICO]**: Las tareas tendrán tiempo estimado en horas como unidad inicial, con nombre de unidad configurable.
- **S-9 [CRÍTICO]**: Las desviaciones relevantes se medirán por porcentaje y por horas absolutas.
- **S-10 [MEDIO]**: Una persona se considera sin asignación cuando no tiene tareas activas asignadas.
- **S-11 [MEDIO]**: La plataforma incluirá prácticas ágiles básicas sin implementar Scrum avanzado completo en el MVP.
- **S-12 [MEDIO]**: El sistema será responsive, priorizando experiencia web/escritorio.
- **S-13 [MEDIO]**: El idioma inicial será español, con internacionalización preparada desde el inicio.
- **S-14 [BAJO]**: Las notificaciones iniciales serán internas dentro de la plataforma.

> Nota: Supuestos ajustados y aceptados durante la conversación con el usuario.

## Preguntas Abiertas

No quedan preguntas abiertas críticas para esta versión de la especificación.

Preguntas resueltas por el usuario:

- **Q-1 resuelta**: Los permisos iniciales quedan definidos por rol según la matriz sugerida. Debe existir siempre un usuario `root` todopoderoso, no eliminable.
- **Q-2 resuelta**: Las estimaciones se manejarán por ahora en horas, pero la unidad debe ser nombrable/configurable.
- **Q-3 resuelta**: Las desviaciones se evaluarán por porcentaje y por horas absolutas.
- **Q-4 resuelta**: Un usuario puede pertenecer a varios equipos y proyectos.

## Matriz de Trazabilidad

| AC | BDD | Sad Paths | Mensajes |
|---|---|---|---|
| AC-1 | BDD-1 | — | — |
| AC-2 | BDD-1 | — | — |
| AC-3 | BDD-1 | SP-2, SP-3 | MSG-2, MSG-3 |
| AC-4 | BDD-1 | SP-2 | MSG-2 |
| AC-5 | BDD-2 | SP-7 | MSG-7 |
| AC-6 | BDD-2 | SP-7 | MSG-7 |
| AC-7 | BDD-2 | SP-7 | MSG-7 |
| AC-8 | BDD-2 | SP-4 | MSG-4 |
| AC-9 | BDD-2 | — | — |
| AC-10 | BDD-3 | SP-1, SP-7, SP-9 | MSG-1, MSG-7, MSG-9 |
| AC-11 | BDD-3 | SP-1 | MSG-1 |
| AC-12 | BDD-3 | — | — |
| AC-13 | BDD-4 | SP-4, SP-12 | MSG-4, MSG-12 |
| AC-14 | BDD-4 | SP-4, SP-5 | MSG-4, MSG-5 |
| AC-15 | BDD-4 | SP-12 | MSG-12 |
| AC-16 | BDD-5 | — | — |
| AC-17 | BDD-5 | SP-5, SP-10 | MSG-5, MSG-10 |
| AC-18 | BDD-5 | — | — |
| AC-19 | BDD-5 | — | — |
| AC-20 | BDD-5 | — | — |
| AC-21 | BDD-6 | SP-11 | MSG-11 |
| AC-22 | BDD-6 | — | — |
| AC-23 | BDD-6 | — | — |
| AC-24 | BDD-7 | SP-6, SP-10 | MSG-6, MSG-10 |
| AC-25 | BDD-7 | SP-2, SP-10 | MSG-2, MSG-10 |
| AC-26 | BDD-7 | — | — |
| AC-27 | BDD-7 | SP-10 | MSG-10 |
| AC-28 | BDD-8 | SP-7, SP-9 | MSG-7, MSG-9 |
| AC-29 | BDD-8 | — | — |
| AC-30 | BDD-8 | — | — |
| AC-31 | BDD-8 | — | — |
| AC-32 | BDD-8 | — | — |
| AC-33 | BDD-8 | — | — |
| AC-34 | BDD-8 | — | — |
| AC-35 | BDD-8 | SP-9 | MSG-9 |
| AC-36 | BDD-8 | — | — |
| AC-37 | BDD-9 | SP-7, SP-8 | MSG-7, MSG-8 |
| AC-38 | BDD-9 | SP-8 | MSG-8 |
| AC-39 | BDD-9 | — | — |
| AC-40 | BDD-9 | — | — |
| AC-41 | BDD-9 | — | — |
| AC-42 | BDD-9 | SP-2 | MSG-2 |
| AC-43 | BDD-10 | SP-5 | MSG-5 |
| AC-44 | BDD-10 | SP-5 | MSG-5 |
| AC-45 | BDD-11 | — | — |
| AC-46 | BDD-11 | SP-13 | MSG-13 |
| AC-47 | BDD-1 | SP-14 | MSG-14 |
| AC-48 | BDD-2 | — | — |
| AC-49 | BDD-8 | SP-9 | MSG-9 |
| AC-50 | BDD-8 | SP-9 | MSG-9 |

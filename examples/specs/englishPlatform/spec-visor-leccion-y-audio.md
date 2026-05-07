# Spec: Visor de lección y audio

## Objetivo

Definir la experiencia del estudiante dentro de una lección, incluyendo presentación del contenido, navegación contextual, reproducción de audios y estados funcionales asociados.

## Alcance

Incluye:

1. Carga de una lección publicada.
2. Presentación del contenido.
3. Reproducción de audio asociado.
4. Navegación entre lecciones.
5. Estados vacíos, carga y error.

## Fuera de alcance

1. Corrección de ejercicios.
2. Persistencia detallada de intentos.
3. Dashboard completo.
4. Gestión admin de audios.

## Dependencias

1. `spec-catalogo-estructura-academica.md`
2. `spec-modelo-datos-plataforma.md`
3. `spec-base-app-y-arquitectura.md`

## Actores y permisos

### student
- puede visualizar lecciones publicadas
- puede reproducir audios asociados

### admin
- puede visualizar la experiencia si tiene acceso de prueba o revisión

## Rutas / pantallas

1. `/app/lessons/[lessonSlug]`

## Componentes UI

1. Cabecera de lección.
2. Área principal de contenido.
3. Lista o bloque de audios asociados.
4. Reproductor de audio.
5. Navegación a lección anterior/siguiente.
6. Indicadores de estado de carga/error.

## Server Actions / operaciones servidor

Prioridad en lectura mediante Server Components.

Operaciones esperadas:
1. obtener lección publicada por slug
2. obtener audios asociados
3. obtener navegación contextual

## Modelo de datos afectado

1. `Lesson`
2. `LessonAudio`
3. `Level`
4. `LessonProgress` para enriquecer contexto si aplica

## Reglas de negocio

1. Solo usuarios autenticados pueden acceder al visor de lección.
2. Solo deben poder visualizarse lecciones publicadas.
3. Si una lección no tiene audios, el contenido debe seguir siendo accesible.
4. Si un audio falla, el sistema debe comunicarlo sin romper el resto de la experiencia.
5. La navegación entre lecciones debe respetar el orden académico.

## Validaciones

1. slug de lección válido
2. lección publicada
3. acceso autenticado
4. recursos de audio activos para ser visibles

## Estados y errores

1. cargando lección
2. lección no encontrada
3. lección no disponible
4. sin audios asociados
5. audio no disponible
6. error al cargar contenido

## Criterios de aceptación

### CA-001 Visualizar lección publicada
**Dado** un estudiante autenticado  
**Cuando** accede a una lección publicada existente  
**Entonces** el sistema muestra su contenido.

### CA-002 Visualizar audios activos
**Dado** una lección publicada con audios activos  
**Cuando** el estudiante accede al visor  
**Entonces** el sistema muestra reproductores o accesos a esos audios.

### CA-003 Continuidad sin audio
**Dado** una lección publicada sin audios  
**Cuando** el estudiante accede al visor  
**Entonces** el sistema permite continuar con el contenido sin error bloqueante.

### CA-004 Navegación contextual
**Dado** una lección publicada dentro de un nivel con varias lecciones  
**Cuando** el estudiante está en el visor  
**Entonces** el sistema le permite navegar a la anterior o siguiente según orden.

## Casos límite

1. Lección despublicada mientras un estudiante la tiene abierta.
2. Audio con URL inválida.
3. Lección muy extensa con múltiples bloques y audios.
4. Navegación contextual con primera o última lección del nivel.

## Notas técnicas

1. Se recomienda renderizar la lección como **Server Component**.
2. El reproductor puede construirse inicialmente sobre HTML audio con estilado consistente.
3. Los recursos de audio deben resolverse desde un storage seguro y accesible.
4. El visor debe reutilizar componentes **shadcn/ui** para feedback y estructura visual.

## Definición de terminado

Se considerará terminado este módulo cuando:

1. El visor de lección tenga flujo y reglas definidas.
2. Los estados de audio y navegación estén cubiertos.
3. Un agente pueda implementar la vista de lección sin ambigüedad crítica.

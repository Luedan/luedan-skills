# Especificación funcional: Plataforma web de aprendizaje de inglés por suscripción con código de acceso

## Resumen ejecutivo

Se requiere una plataforma web privada para aprender inglés. El acceso no será gratuito: todo usuario deberá contar con una compra válida y registrarse usando un código de suscripción de un solo uso. La compra otorgará acceso de por vida y quedará vinculada a una única cuenta.

La plataforma ofrecerá lecciones, ejercicios interactivos, audio, comprensión auditiva, dictado, seguimiento de progreso y un panel de administración total con auditoría de acciones.

## Historia de usuario

Como usuario, quiero una plataforma web donde aprender inglés, para acceder a contenido estructurado, practicar ejercicios y avanzar en todos los niveles mediante un acceso privado de pago.

## Objetivo

Permitir que usuarios con acceso válido aprendan inglés desde una experiencia web estructurada, evaluable y administrable, con progresión por lecciones y control total del contenido desde un panel administrativo.

## Alcance

1. Pantalla pública de acceso o presentación.
2. Registro mediante código de suscripción válido.
3. Inicio y cierre de sesión.
4. Acceso privado a todo el contenido.
5. Plan único con acceso a todos los niveles de inglés.
6. Dashboard del estudiante.
7. Lecciones y módulos.
8. Ejercicios de:
   - opción múltiple
   - completar huecos
   - traducción
   - comprensión auditiva
   - dictado
9. Reproducción de audio.
10. Seguimiento de progreso.
11. Completitud por porcentaje mínimo de aciertos.
12. Reintentos ilimitados.
13. Panel de administración total.
14. Gestión de usuarios, contenido, audios, ejercicios y códigos.
15. Auditoría de acciones administrativas.

## Fuera de alcance

1. Contenido gratuito.
2. Suscripción recurrente mensual o anual.
3. Múltiples planes comerciales.
4. Clases en vivo.
5. App móvil nativa.
6. Certificaciones oficiales externas.
7. Comunidad social tipo foro/chat, salvo ampliación futura.

## Actores y roles

### 1. Visitante
- Puede ver la pantalla pública.
- Puede intentar registrarse.
- No puede acceder al contenido sin código válido.

### 2. Estudiante
- Puede registrarse con código válido.
- Puede iniciar sesión.
- Puede acceder a todos los niveles incluidos en el plan único.
- Puede realizar ejercicios y reintentarlos sin límite.
- Puede consultar su progreso.

### 3. Administrador
- Tiene control funcional total del sistema.
- Gestiona usuarios, contenido, ejercicios, audios, códigos y parámetros.
- Puede regalar accesos.
- Puede parametrizar el porcentaje mínimo requerido por lección.
- Puede consultar auditoría de acciones.

## Contexto y decisiones confirmadas

1. Todo el contenido es privado y de pago.
2. El usuario debe introducir un código de suscripción para crear su cuenta.
3. El código es de un solo uso.
4. Una vez usado, el código no puede volver a utilizarse.
5. La condición de un solo uso debe quedar explícita en términos y condiciones.
6. La compra es única y otorga acceso de por vida.
7. El acceso queda vinculado a una sola cuenta.
8. Existe un único plan: aprender inglés.
9. El plan único da acceso a todos los niveles.
10. La completitud de una lección depende de un porcentaje mínimo de aciertos.
11. El valor de referencia inicial del porcentaje mínimo es 90%.
12. El administrador puede parametrizar ese porcentaje por lección.
13. Los reintentos son ilimitados.
14. La corrección de traducción y dictado debe contemplar tolerancia parcial.
15. El sistema debe incluir reproducción de audio, preguntas auditivas y dictado.
16. El panel administrativo debe registrar auditoría de acciones.

## Precondiciones

1. Deben existir códigos válidos creados por administración.
2. Deben existir lecciones y ejercicios cargados en el sistema.
3. El usuario debe crear una cuenta usando un código válido para poder acceder.

## Flujos principales

### Flujo 1: Registro con código
1. El visitante accede a la plataforma.
2. Selecciona crear cuenta.
3. Completa nombre, email, contraseña y código de suscripción.
4. El sistema valida los datos y el código.
5. Si todo es correcto, crea la cuenta.
6. El código queda marcado como consumido.
7. El acceso queda vinculado a esa cuenta de por vida.

### Flujo 2: Inicio de sesión
1. El usuario introduce sus credenciales.
2. El sistema valida la autenticación.
3. El usuario accede a su dashboard.

### Flujo 3: Consumo de lección
1. El estudiante entra al dashboard.
2. Selecciona una lección.
3. Consulta contenido.
4. Reproduce audios asociados.
5. Resuelve ejercicios.
6. El sistema corrige respuestas.
7. El sistema calcula el porcentaje de acierto.
8. Si alcanza el umbral requerido, la lección queda completada.

### Flujo 4: Reintentos
1. El estudiante no alcanza el porcentaje mínimo.
2. El sistema informa el resultado.
3. El estudiante puede reintentar sin límite.

### Flujo 5: Gestión administrativa
1. El administrador accede al panel.
2. Gestiona usuarios, códigos, lecciones, ejercicios, audios y parámetros.
3. Guarda cambios.
4. El sistema persiste los cambios.
5. El sistema registra la acción en auditoría.

## Flujos alternativos y excepciones

1. Si el código es inválido, el registro no debe completarse.
2. Si el código ya fue usado, el sistema debe rechazarlo.
3. Si el código está deshabilitado, el sistema debe rechazarlo.
4. Si el usuario no alcanza el porcentaje mínimo, la lección no debe marcarse como completada.
5. Si falla la carga de audio, el sistema debe informar el error y permitir continuar cuando aplique.
6. Si el administrador desactiva contenido, este deja de ser accesible para estudiantes.
7. Si falla el guardado del progreso, el sistema debe informar el error y permitir reintento.

## Requisitos funcionales

### Acceso y autenticación
- **FR-001** El sistema debe mostrar una pantalla pública de acceso o presentación.
- **FR-002** El sistema debe permitir registro únicamente con código de suscripción válido.
- **FR-003** El sistema debe validar el código antes de crear la cuenta.
- **FR-004** El sistema debe impedir el registro sin código válido.
- **FR-005** El sistema debe impedir el registro con código ya consumido.
- **FR-006** El sistema debe permitir inicio y cierre de sesión.

### Códigos y acceso
- **FR-007** El sistema debe gestionar códigos de acceso asociados a compras únicas.
- **FR-008** El sistema debe permitir al administrador crear, activar, desactivar, asignar o regalar códigos.
- **FR-009** El sistema debe registrar el estado de cada código.
- **FR-010** El sistema debe marcar un código como consumido cuando se use en un registro exitoso.
- **FR-011** El sistema debe vincular el acceso concedido a la cuenta creada.
- **FR-012** El sistema debe conservar el acceso de por vida una vez vinculado a la cuenta válida.

### Aprendizaje
- **FR-013** El sistema debe listar cursos, unidades o lecciones disponibles.
- **FR-014** El sistema debe permitir visualizar contenido educativo.
- **FR-015** El sistema debe reproducir audios asociados a lecciones o ejercicios.
- **FR-016** El sistema debe permitir ejercicios de opción múltiple.
- **FR-017** El sistema debe permitir ejercicios de completar huecos.
- **FR-018** El sistema debe permitir ejercicios de traducción.
- **FR-019** El sistema debe permitir ejercicios de comprensión auditiva.
- **FR-020** El sistema debe permitir ejercicios de dictado.
- **FR-021** El sistema debe validar respuestas y calcular porcentaje de acierto.
- **FR-022** El sistema debe aplicar tolerancia parcial en ejercicios de traducción y dictado.
- **FR-023** El sistema debe permitir reintentos ilimitados.

### Progreso
- **FR-024** El sistema debe mostrar el progreso general y por lección.
- **FR-025** El sistema debe marcar una lección como completada solo cuando el estudiante alcance el porcentaje mínimo requerido.
- **FR-026** El sistema debe guardar historial de intentos y resultados.

### Administración
- **FR-027** El administrador debe poder gestionar usuarios.
- **FR-028** El administrador debe poder gestionar cursos, unidades, lecciones y ejercicios.
- **FR-029** El administrador debe poder gestionar audios y recursos asociados.
- **FR-030** El administrador debe poder gestionar códigos y accesos regalados.
- **FR-031** El administrador debe poder parametrizar el porcentaje mínimo requerido por lección.
- **FR-032** El administrador debe poder consultar métricas operativas.
- **FR-033** El sistema debe registrar auditoría de acciones administrativas.

## Reglas de negocio

1. Ningún usuario puede crear una cuenta sin un código válido.
2. Todo el contenido es privado y de pago.
3. La compra es única y no recurrente.
4. Cada código es de un solo uso.
5. Una vez consumido, un código no puede reutilizarse.
6. El acceso otorgado por una compra queda vinculado de por vida a una única cuenta.
7. Existe un solo plan comercial e incluye todos los niveles.
8. Una lección se considera completada únicamente cuando el usuario alcanza el porcentaje mínimo configurado.
9. El porcentaje mínimo inicial por defecto es 90%, pero puede parametrizarse por lección.
10. Los reintentos son ilimitados.
11. La corrección de traducción y dictado debe contemplar tolerancia parcial razonable.
12. Toda acción administrativa crítica debe quedar registrada en auditoría.
13. Los términos y condiciones deben dejar explícito que el código de acceso es de un solo uso y no reutilizable tras su consumo.

## Validaciones

1. Email con formato válido.
2. Contraseña válida según política definida.
3. Código de suscripción obligatorio en el registro.
4. Código existente.
5. Código activo.
6. Código no consumido.
7. Respuestas obligatorias en ejercicios que lo requieran.
8. Validación de formato para respuestas de texto, traducción y dictado.

## Datos requeridos

### Usuario
- nombre
- email
- credenciales
- rol
- estado
- fecha de alta
- código utilizado en el registro

### Código de acceso
- código
- estado
- fecha de creación
- tipo de acceso
- usos permitidos
- usos consumidos
- creado por
- asignado por
- observaciones

### Lección
- título
- descripción
- nivel
- contenido
- estado
- porcentaje mínimo requerido
- orden

### Ejercicio
- tipo
- enunciado
- contenido
- opciones si aplica
- respuestas válidas
- puntaje
- audio asociado
- obligatoriedad

### Resultado
- usuario
- lección
- intento
- respuestas
- aciertos
- porcentaje
- fecha

### Auditoría
- administrador
- acción realizada
- entidad afectada
- fecha/hora
- detalle resumido

## Estados y mensajes

1. Código válido.
2. Código inválido.
3. Código ya utilizado.
4. Código deshabilitado.
5. Registro exitoso.
6. Login correcto.
7. Login incorrecto.
8. Respuesta correcta.
9. Respuesta incorrecta.
10. Porcentaje insuficiente para completar la lección.
11. Lección completada.
12. Error al guardar progreso.
13. Audio no disponible.
14. Cambio administrativo guardado.
15. Acción administrativa registrada en auditoría.

## Seguridad funcional

1. Los estudiantes solo pueden ver su propio progreso.
2. Los visitantes no pueden acceder a contenido privado.
3. El panel administrativo debe estar restringido a administradores.
4. El sistema debe impedir accesos no autorizados a datos o funciones.
5. Las acciones administrativas relevantes deben quedar registradas.

## Dependencias

1. Base de datos.
2. Sistema de autenticación.
3. Almacenamiento o servicio de audio.
4. Mecanismo de generación y validación de códigos.

## Métricas o eventos

1. Número de cuentas creadas con código.
2. Tasa de registro exitoso.
3. Número de códigos consumidos.
4. Tasa de finalización de lecciones.
5. Porcentaje promedio de acierto.
6. Número medio de reintentos por lección.
7. Uso de ejercicios con audio.
8. Tiempo medio por sesión.

## Consideraciones de accesibilidad

1. Navegación por teclado.
2. Contraste suficiente.
3. Etiquetas claras en formularios.
4. Mensajes de error comprensibles.
5. Reproductores de audio accesibles.

## Criterios de aceptación

### CA-001 Registro con código válido
**Dado** un visitante sin cuenta  
**Cuando** completa el formulario con datos válidos y un código válido  
**Entonces** el sistema crea la cuenta  
**Y** vincula el acceso de por vida a esa cuenta.

### CA-002 Rechazo por código inválido
**Dado** un visitante sin cuenta  
**Cuando** intenta registrarse con un código inválido o inexistente  
**Entonces** el sistema rechaza el registro  
**Y** muestra el motivo.

### CA-003 Rechazo por código consumido
**Dado** un visitante sin cuenta  
**Cuando** intenta registrarse con un código ya utilizado  
**Entonces** el sistema rechaza el registro.

### CA-004 Completar lección por porcentaje mínimo
**Dado** un estudiante autenticado en una lección  
**Cuando** obtiene un porcentaje igual o superior al mínimo configurado  
**Entonces** el sistema marca la lección como completada.

### CA-005 No completar lección por porcentaje insuficiente
**Dado** un estudiante autenticado en una lección  
**Cuando** obtiene un porcentaje inferior al mínimo configurado  
**Entonces** la lección no se marca como completada.

### CA-006 Reintentos ilimitados
**Dado** un estudiante autenticado  
**Cuando** no alcanza el porcentaje mínimo  
**Entonces** el sistema le permite volver a intentarlo sin límite definido.

### CA-007 Parametrización administrativa
**Dado** un administrador autenticado  
**Cuando** configura el porcentaje mínimo de una lección  
**Entonces** el sistema guarda el nuevo valor  
**Y** lo aplica a futuras evaluaciones.

### CA-008 Auditoría administrativa
**Dado** un administrador autenticado  
**Cuando** crea, modifica, desactiva o elimina contenido, usuarios o códigos  
**Entonces** el sistema registra la acción en auditoría.

## Casos límite

1. Código válido pero ya consumido.
2. Código ingresado con errores de formato.
3. Usuario que abandona una lección a mitad.
4. Audio interrumpido o no disponible.
5. Respuestas parcialmente correctas en traducción o dictado.
6. Múltiples intentos con distintos resultados.
7. Cambio del porcentaje mínimo después de existir intentos previos.
8. Desactivación de una lección ya iniciada por usuarios.

## BDD

```gherkin
Feature: Plataforma privada de aprendizaje de inglés con código de acceso

  Scenario: Registro exitoso con código válido
    Given un visitante sin cuenta
    When introduce sus datos y un código válido
    Then el sistema crea la cuenta
    And marca el código como consumido

  Scenario: Registro rechazado por código ya usado
    Given un visitante sin cuenta
    When introduce un código ya consumido
    Then el sistema rechaza el registro

  Scenario: Completar una lección por porcentaje mínimo
    Given un estudiante autenticado dentro de una lección
    When finaliza los ejercicios con un porcentaje igual o superior al requerido
    Then el sistema marca la lección como completada

  Scenario: Reintentos ilimitados tras no alcanzar el mínimo
    Given un estudiante autenticado dentro de una lección
    When no alcanza el porcentaje mínimo
    Then el sistema no marca la lección como completada
    And le permite volver a intentarlo

  Scenario: Auditoría de cambio administrativo
    Given un administrador autenticado
    When modifica el porcentaje mínimo de una lección
    Then el sistema registra esa acción en auditoría
```

## Mockup ASCII

```text
+------------------------------------------------------+
| LOGO                          [Login] [Crear cuenta] |
+------------------------------------------------------+
| Aprende inglés                                       |
| Acceso privado mediante código                       |
| [Registrarme con código]                             |
+------------------------------------------------------+

Registro
+------------------------------------------------------+
| Nombre                                               |
| Email                                                |
| Contraseña                                           |
| Código de suscripción                                |
| [Crear cuenta]                                       |
+------------------------------------------------------+

Dashboard
+------------------------------------------------------+
| Hola, Usuario                                        |
| Progreso general: 72%                                |
+------------------------------------------------------+
| Lecciones                                            |
| [Gramática] [Listening] [Vocabulario] [Dictado]      |
+------------------------------------------------------+

Admin
+------------------------------------------------------+
| Usuarios | Códigos | Lecciones | Ejercicios | Logs   |
+------------------------------------------------------+
```

## Supuestos

1. El producto estará disponible inicialmente en interfaz en español.
2. La tolerancia parcial en traducción y dictado será configurable o definida por reglas de corrección funcionales.
3. La auditoría será operativa y orientada a trazabilidad interna.

## Preguntas abiertas

1. ¿Los códigos tendrán fecha de expiración o serán permanentes hasta su uso?
2. ¿Los códigos regalados tendrán exactamente las mismas condiciones que los códigos de compra?
3. ¿El estudiante podrá ver detalle de errores por ejercicio o solo resultado final?

## Próximos pasos recomendados

1. Validar esta especificación con stakeholders.
2. Convertirla en backlog funcional por épicas e historias.
3. Definir copy legal de términos y condiciones sobre el uso único del código.
4. Definir reglas exactas de tolerancia parcial para traducción y dictado.

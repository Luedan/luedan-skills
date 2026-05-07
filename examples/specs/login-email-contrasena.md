# Spec: Login con email y contraseña

## Contexto

La plataforma requiere un mecanismo de autenticación mediante credenciales. El usuario debe poder iniciar sesión usando su email y contraseña. No se contemplan métodos alternativos como redes sociales, SSO, magic links, biometría u otros mecanismos de autenticación.

Al iniciar sesión correctamente, el usuario debe ser redirigido a un dashboard. El dashboard queda fuera del alcance funcional de esta especificación y solo debe existir como destino en blanco.

## Historia de Usuario

Como usuario, quiero poder hacer login en la plataforma utilizando email y contraseña, para acceder a mi sesión y ser redirigido al dashboard.

## Objetivo

Permitir que un usuario existente acceda a la plataforma mediante credenciales válidas, usando únicamente email y contraseña.

## Alcance

Incluye:

1. Pantalla de login.
2. Formulario con campos:
   - Email.
   - Contraseña.
3. Validación de campos obligatorios.
4. Validación básica del formato de email.
5. Envío de credenciales para autenticación.
6. Manejo de login exitoso.
7. Manejo de credenciales inválidas.
8. Bloqueo temporal por intentos fallidos.
9. Redirección al dashboard tras autenticación exitosa.
10. Dashboard en blanco únicamente como destino posterior al login.

## Fuera de Alcance

No incluye:

1. Registro de usuarios.
2. Recuperación de contraseña.
3. Login con redes sociales.
4. Login con SSO.
5. Login con email mágico o magic link.
6. Autenticación multifactor.
7. Desarrollo funcional del dashboard.
8. Gestión de perfiles de usuario.
9. Cambio de contraseña.
10. Desbloqueo manual por administrador.
11. Roles o permisos diferenciados posteriores al login.

## Actores

1. **Usuario**: persona que intenta iniciar sesión en la plataforma.
2. **Sistema**: valida las credenciales, gestiona intentos fallidos, bloquea temporalmente el acceso cuando corresponde y permite o rechaza el inicio de sesión.

## Precondiciones

1. El usuario ya existe en la plataforma.
2. El usuario tiene un email y contraseña registrados.
3. La cuenta del usuario está activa.
4. La cuenta del usuario no está bloqueada temporalmente por intentos fallidos.
5. La plataforma está disponible.

## Requisitos Funcionales

### FR-001 - Mostrar pantalla de login

El sistema debe mostrar una pantalla de login cuando el usuario acceda sin sesión activa.

La pantalla debe incluir:

1. Campo de email.
2. Campo de contraseña.
3. Botón “Iniciar sesión”.

### FR-002 - Autenticación solo con email y contraseña

El sistema debe permitir iniciar sesión únicamente mediante email y contraseña.

No deben mostrarse opciones de login con redes sociales, SSO, magic link ni otros métodos alternativos.

### FR-003 - Validar campos obligatorios

El sistema debe validar que el email y la contraseña estén completos antes de intentar autenticar.

Si uno o ambos campos están vacíos, el sistema debe mostrar el mensaje correspondiente y no debe procesar el intento de autenticación.

### FR-004 - Validar formato de email

El sistema debe validar que el valor ingresado en el campo email tenga formato de email.

Si el formato no es válido, el sistema debe mostrar un mensaje de validación y no debe procesar el intento de autenticación.

### FR-005 - Login exitoso

Cuando el usuario ingrese un email y contraseña válidos, el sistema debe iniciar la sesión y redirigir al usuario al dashboard.

### FR-006 - Login con credenciales inválidas

Cuando el usuario ingrese un email o contraseña incorrectos, el sistema debe rechazar el acceso, mostrar un mensaje de error genérico y mantener al usuario en la pantalla de login.

### FR-007 - Contabilizar intentos fallidos

El sistema debe contabilizar los intentos fallidos de autenticación asociados al email ingresado.

Un intento fallido ocurre cuando:

1. El email tiene formato válido.
2. La contraseña fue ingresada.
3. El sistema procesa la autenticación.
4. Las credenciales son inválidas.

No deben contabilizarse como intentos fallidos los errores de validación local, como email vacío, contraseña vacía o email con formato inválido.

### FR-008 - Bloquear cuenta por intentos fallidos

El sistema debe bloquear temporalmente la cuenta durante 30 minutos después de 5 intentos fallidos consecutivos.

Mientras la cuenta esté bloqueada, el sistema no debe permitir iniciar sesión, incluso si el usuario ingresa la contraseña correcta.

### FR-009 - Restablecer contador de intentos fallidos

El sistema debe restablecer el contador de intentos fallidos cuando el usuario inicia sesión correctamente.

### FR-010 - Dashboard en blanco

Después de un login exitoso, el sistema debe redirigir al usuario a un dashboard en blanco.

El dashboard no debe incluir funcionalidades adicionales dentro de esta especificación.

## Reglas de Negocio

BR-001. El único método de autenticación permitido es email y contraseña.

BR-002. El email y la contraseña son obligatorios.

BR-003. El email debe tener un formato válido antes de intentar autenticar.

BR-004. El sistema no debe indicar si el email existe o si la contraseña es incorrecta.

BR-005. El mensaje de error de credenciales inválidas debe ser genérico.

BR-006. La cuenta debe bloquearse temporalmente después de 5 intentos fallidos consecutivos.

BR-007. El bloqueo temporal debe durar 30 minutos.

BR-008. Durante el bloqueo temporal, la cuenta no puede iniciar sesión.

BR-009. Un inicio de sesión exitoso debe reiniciar el contador de intentos fallidos.

BR-010. El dashboard está fuera de alcance y debe mostrarse como pantalla en blanco.

## Validaciones

| Campo | Obligatorio | Validación | Mensaje sugerido |
|---|---:|---|---|
| Email | Sí | No puede estar vacío | El email es obligatorio. |
| Email | Sí | Debe tener formato de email | Ingresa un email válido. |
| Contraseña | Sí | No puede estar vacía | La contraseña es obligatoria. |

## Datos Requeridos

Para iniciar sesión:

1. Email.
2. Contraseña.

Para gestión de bloqueo:

1. Cantidad de intentos fallidos consecutivos.
2. Fecha y hora del último intento fallido.
3. Fecha y hora de inicio del bloqueo temporal.
4. Fecha y hora estimada de finalización del bloqueo temporal.

## Estados

### Pantalla de login

1. Estado inicial.
2. Estado con campos vacíos.
3. Estado con datos ingresados.
4. Estado validando campos.
5. Estado autenticando.
6. Estado con error de validación.
7. Estado con credenciales inválidas.
8. Estado con cuenta bloqueada temporalmente.
9. Estado con error general del sistema.

### Sesión

1. No autenticada.
2. Autenticando.
3. Autenticada.

### Cuenta

1. Activa.
2. Bloqueada temporalmente por intentos fallidos.

## Mensajes

| Situación | Mensaje sugerido |
|---|---|
| Email vacío | El email es obligatorio. |
| Email con formato inválido | Ingresa un email válido. |
| Contraseña vacía | La contraseña es obligatoria. |
| Credenciales inválidas | Email o contraseña incorrectos. |
| Cuenta bloqueada | Tu cuenta está bloqueada temporalmente por intentos fallidos. Inténtalo nuevamente en 30 minutos. |
| Error general | No pudimos iniciar sesión en este momento. Inténtalo nuevamente. |

## Happy Path

1. El usuario accede a la pantalla de login.
2. El sistema muestra el formulario con campos de email y contraseña.
3. El usuario ingresa un email válido y su contraseña.
4. El usuario presiona el botón “Iniciar sesión”.
5. El sistema valida que ambos campos estén completos.
6. El sistema valida que el email tenga formato válido.
7. El sistema verifica las credenciales.
8. Si las credenciales son válidas, el sistema inicia la sesión.
9. El sistema restablece el contador de intentos fallidos del usuario.
10. El sistema redirige al usuario al dashboard.
11. El dashboard se muestra como pantalla en blanco.

## Flujos Alternativos

### A-001 - Credenciales inválidas sin alcanzar bloqueo

1. El usuario ingresa un email con formato válido y una contraseña incorrecta.
2. El sistema procesa la autenticación.
3. El sistema rechaza el acceso.
4. El sistema incrementa el contador de intentos fallidos.
5. El sistema muestra el mensaje “Email o contraseña incorrectos.”
6. El usuario permanece en la pantalla de login.

### A-002 - Credenciales inválidas alcanzando 5 intentos fallidos

1. El usuario realiza el quinto intento fallido consecutivo.
2. El sistema rechaza el acceso.
3. El sistema bloquea temporalmente la cuenta durante 30 minutos.
4. El sistema muestra un mensaje indicando el bloqueo temporal.
5. El usuario permanece en la pantalla de login.

### A-003 - Intento de login con cuenta bloqueada

1. El usuario intenta iniciar sesión con una cuenta bloqueada temporalmente.
2. El sistema identifica que la cuenta sigue bloqueada.
3. El sistema rechaza el acceso sin iniciar sesión.
4. El sistema muestra el mensaje de cuenta bloqueada.

### A-004 - Login después de finalizado el bloqueo

1. El bloqueo temporal de 30 minutos ha finalizado.
2. El usuario ingresa email y contraseña válidos.
3. El sistema permite autenticar nuevamente.
4. El sistema inicia sesión correctamente.
5. El sistema redirige al dashboard en blanco.

## Sad Paths / Casos de Error

### E-001 - Email vacío

1. El usuario presiona “Iniciar sesión” sin completar el email.
2. El sistema muestra “El email es obligatorio.”
3. El sistema no intenta autenticar.

### E-002 - Contraseña vacía

1. El usuario presiona “Iniciar sesión” sin completar la contraseña.
2. El sistema muestra “La contraseña es obligatoria.”
3. El sistema no intenta autenticar.

### E-003 - Email con formato inválido

1. El usuario ingresa un valor que no tiene formato de email.
2. El usuario presiona “Iniciar sesión”.
3. El sistema muestra “Ingresa un email válido.”
4. El sistema no intenta autenticar.

### E-004 - Error general del sistema

1. El usuario ingresa credenciales válidas o inválidas.
2. Ocurre un error inesperado durante la autenticación.
3. El sistema muestra “No pudimos iniciar sesión en este momento. Inténtalo nuevamente.”
4. El usuario permanece en la pantalla de login.

## Criterios de Aceptación

AC-001. Dado que el usuario accede a la plataforma sin sesión activa, cuando entra a la pantalla de login, entonces debe ver un formulario con campos de email y contraseña.

AC-002. Dado que el usuario ve la pantalla de login, entonces no debe ver opciones para iniciar sesión con redes sociales, SSO, magic link u otros métodos alternativos.

AC-003. Dado que el usuario no completa el campo email, cuando intenta iniciar sesión, entonces el sistema debe mostrar “El email es obligatorio.” y no debe autenticar.

AC-004. Dado que el usuario ingresa un email con formato inválido, cuando intenta iniciar sesión, entonces el sistema debe mostrar “Ingresa un email válido.” y no debe autenticar.

AC-005. Dado que el usuario no completa el campo contraseña, cuando intenta iniciar sesión, entonces el sistema debe mostrar “La contraseña es obligatoria.” y no debe autenticar.

AC-006. Dado que el usuario ingresa email y contraseña válidos, cuando presiona “Iniciar sesión”, entonces el sistema debe iniciar sesión y redirigirlo al dashboard.

AC-007. Dado que el usuario ingresa credenciales inválidas, cuando presiona “Iniciar sesión”, entonces el sistema debe mostrar “Email o contraseña incorrectos.” y mantenerlo en la pantalla de login.

AC-008. Dado que el usuario acumula 5 intentos fallidos consecutivos, cuando se procesa el quinto intento fallido, entonces el sistema debe bloquear temporalmente la cuenta durante 30 minutos.

AC-009. Dado que la cuenta está bloqueada temporalmente, cuando el usuario intenta iniciar sesión antes de finalizar los 30 minutos, entonces el sistema debe rechazar el acceso y mostrar un mensaje de bloqueo temporal.

AC-010. Dado que la cuenta estuvo bloqueada temporalmente, cuando han pasado 30 minutos, entonces el usuario debe poder volver a intentar iniciar sesión.

AC-011. Dado que el usuario inicia sesión correctamente, cuando llega al dashboard, entonces debe ver una pantalla en blanco sin funcionalidades adicionales.

AC-012. Dado que el usuario inicia sesión correctamente después de intentos fallidos previos, entonces el contador de intentos fallidos debe reiniciarse.

## BDD

```gherkin
Feature: Login con email y contraseña

  Scenario: Login exitoso
    Given que el usuario está en la pantalla de login
    And tiene una cuenta activa y no bloqueada
    And tiene credenciales válidas
    When ingresa su email y contraseña
    And presiona "Iniciar sesión"
    Then el sistema debe autenticar al usuario
    And debe reiniciar el contador de intentos fallidos
    And debe redirigirlo al dashboard
    And el dashboard debe mostrarse como una pantalla en blanco

  Scenario: Login con credenciales inválidas
    Given que el usuario está en la pantalla de login
    And la cuenta no está bloqueada
    When ingresa un email con formato válido y una contraseña incorrecta
    And presiona "Iniciar sesión"
    Then el sistema debe rechazar el acceso
    And debe incrementar el contador de intentos fallidos
    And debe mostrar el mensaje "Email o contraseña incorrectos."
    And el usuario debe permanecer en la pantalla de login

  Scenario: Bloqueo después de 5 intentos fallidos
    Given que el usuario tiene 4 intentos fallidos consecutivos
    When realiza un quinto intento fallido
    Then el sistema debe bloquear temporalmente la cuenta durante 30 minutos
    And debe mostrar un mensaje de bloqueo temporal
    And no debe iniciar sesión

  Scenario: Intento de login con cuenta bloqueada
    Given que la cuenta del usuario está bloqueada temporalmente
    When el usuario intenta iniciar sesión antes de finalizar los 30 minutos
    Then el sistema debe rechazar el acceso
    And debe mostrar un mensaje indicando que la cuenta está bloqueada temporalmente

  Scenario: Login después de finalizado el bloqueo
    Given que la cuenta del usuario fue bloqueada temporalmente
    And han pasado 30 minutos desde el inicio del bloqueo
    When el usuario ingresa credenciales válidas
    And presiona "Iniciar sesión"
    Then el sistema debe autenticar al usuario
    And debe redirigirlo al dashboard

  Scenario: Login con email vacío
    Given que el usuario está en la pantalla de login
    When deja el campo email vacío
    And presiona "Iniciar sesión"
    Then el sistema debe mostrar "El email es obligatorio."
    And no debe intentar autenticar

  Scenario: Login con contraseña vacía
    Given que el usuario está en la pantalla de login
    When deja el campo contraseña vacío
    And presiona "Iniciar sesión"
    Then el sistema debe mostrar "La contraseña es obligatoria."
    And no debe intentar autenticar

  Scenario: Login con email inválido
    Given que el usuario está en la pantalla de login
    When ingresa un valor sin formato de email en el campo email
    And presiona "Iniciar sesión"
    Then el sistema debe mostrar "Ingresa un email válido."
    And no debe intentar autenticar

  Scenario: No se muestran métodos alternativos de login
    Given que el usuario está en la pantalla de login
    Then no debe ver opciones para iniciar sesión con redes sociales
    And no debe ver opciones de SSO
    And no debe ver magic links
    And no debe ver otros métodos alternativos de autenticación
```

## Mockup ASCII

### Pantalla de login

```text
+--------------------------------------+
|              Plataforma              |
|                                      |
|  Email                               |
|  +-------------------------------+   |
|  | usuario@dominio.com           |   |
|  +-------------------------------+   |
|                                      |
|  Contraseña                          |
|  +-------------------------------+   |
|  | ********                      |   |
|  +-------------------------------+   |
|                                      |
|        [ Iniciar sesión ]            |
|                                      |
|  Email o contraseña incorrectos.     |
+--------------------------------------+
```

### Dashboard en blanco

```text
+--------------------------------------+
|                                      |
|                                      |
|                                      |
|                                      |
|                                      |
|                                      |
|                                      |
+--------------------------------------+
```

## Seguridad Funcional

1. La contraseña no debe mostrarse en texto plano.
2. El mensaje de error de credenciales debe ser genérico.
3. El sistema no debe revelar si el email existe o no.
4. La sesión solo debe iniciarse si las credenciales son válidas y la cuenta no está bloqueada.
5. La cuenta debe bloquearse temporalmente durante 30 minutos después de 5 intentos fallidos consecutivos.
6. Durante el bloqueo temporal, el sistema no debe permitir login aunque las credenciales sean correctas.

## Consideraciones de Accesibilidad

1. Los campos deben tener etiquetas visibles o accesibles.
2. Los errores deben estar asociados al campo correspondiente cuando aplique.
3. El formulario debe poder usarse con teclado.
4. El botón de login debe tener un texto claro.
5. El foco debe indicar visualmente el campo activo.
6. Los mensajes de error deben ser perceptibles para tecnologías de asistencia.

## Métricas o Eventos

Eventos sugeridos para análisis funcional:

1. Visualización de pantalla de login.
2. Intento de login.
3. Login exitoso.
4. Login fallido por credenciales inválidas.
5. Cuenta bloqueada por intentos fallidos.
6. Intento de login con cuenta bloqueada.
7. Error general de autenticación.

## Supuestos Funcionales

1. El usuario ya existe previamente en la plataforma.
2. El campo usuario fue redefinido como email.
3. El login requiere exactamente dos campos: email y contraseña.
4. No existe registro de usuarios dentro de este alcance.
5. No existe recuperación de contraseña dentro de este alcance.
6. No se requiere autenticación multifactor.
7. No se requiere opción “Recordarme” o “Mantener sesión iniciada”.
8. El dashboard es solo una pantalla destino en blanco.
9. El bloqueo por intentos fallidos ocurre después de 5 intentos fallidos consecutivos.
10. La duración del bloqueo temporal es de 30 minutos.
11. No se incluye desbloqueo manual por administrador.
12. No se definen roles o permisos diferenciados para esta especificación.

## Preguntas Abiertas

1. ¿Debe existir opción para mostrar/ocultar contraseña?
2. ¿Qué debe ocurrir si un usuario ya autenticado intenta acceder nuevamente a la pantalla de login?
3. ¿El mensaje de bloqueo debe mostrar el tiempo restante exacto o solo indicar que intente más tarde?
4. ¿Se debe notificar al usuario por email cuando su cuenta sea bloqueada temporalmente?
5. ¿Hay requisitos visuales mínimos para la pantalla de login, como logo, nombre de plataforma o color de marca?

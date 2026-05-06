# Spec: Recuperación de contraseña por email

## Contexto

Los usuarios registrados necesitan una forma segura de recuperar el acceso a su cuenta cuando olvidan su contraseña. El sistema permitirá iniciar un proceso de recuperación usando el email registrado, sin revelar si dicho email existe o no en el sistema.

## Historia de Usuario

Como usuario registrado, quiero recuperar mi contraseña utilizando el email que registré en mi cuenta, para poder restablecer el acceso a mi cuenta de forma segura.

## Objetivo

Permitir que un usuario solicite la recuperación de contraseña mediante su email registrado y pueda definir una nueva contraseña fuerte mediante un enlace temporal y de un solo uso.

## Alcance

1. Formulario para solicitar recuperación mediante email.
2. Validación del formato del email.
3. Envío de email con enlace de recuperación si corresponde.
4. Enlace de recuperación válido durante 15 minutos.
5. Formulario para crear y confirmar una nueva contraseña.
6. Validación de contraseña fuerte.
7. Confirmación visual de cambio exitoso.
8. Cierre de sesiones activas después del cambio de contraseña.
9. Límite de solicitudes por email.
10. Exclusión funcional de la cuenta root del sistema.

## Fuera de Alcance

1. Recuperación de contraseña por SMS o teléfono.
2. Recuperación mediante preguntas de seguridad.
3. Recuperación manual asistida por soporte.
4. Cambio de email durante el proceso.
5. Inicio de sesión automático después de restablecer la contraseña.
6. Recuperación de contraseña para la cuenta root del sistema.

## Actores

1. **Usuario registrado:** solicita restablecer su contraseña.
2. **Sistema:** valida la solicitud, envía instrucciones, permite definir nueva contraseña y cierra sesiones activas.
3. **Cuenta root:** cuenta administrativa especial excluida de este flujo.

## Precondiciones

1. El usuario debe tener una cuenta registrada con email asociado.
2. El usuario debe poder acceder a la bandeja de entrada del email registrado.
3. La cuenta no debe ser la cuenta root del sistema.

## Happy Path

1. El usuario selecciona la opción “Olvidé mi contraseña”.
2. El sistema muestra un formulario para ingresar el email registrado.
3. El usuario ingresa un email con formato válido.
4. El sistema registra la solicitud y muestra un mensaje neutral.
5. Si el email corresponde a una cuenta válida y no root, el sistema envía un email con un enlace de recuperación.
6. El usuario abre el enlace dentro de los 15 minutos de vigencia.
7. El sistema muestra un formulario para ingresar nueva contraseña y confirmación.
8. El usuario ingresa una contraseña fuerte y la confirma correctamente.
9. El sistema actualiza la contraseña.
10. El sistema invalida el enlace utilizado.
11. El sistema cierra las sesiones activas del usuario.
12. El sistema muestra un mensaje de éxito.
13. El usuario puede iniciar sesión con su nueva contraseña.

## Flujos Alternativos

### FA-001: Email no registrado

1. El usuario ingresa un email con formato válido que no está asociado a ninguna cuenta.
2. El sistema muestra el mismo mensaje neutral usado para solicitudes válidas.
3. El sistema no envía enlace de recuperación.

### FA-002: Solicitud para cuenta root

1. Se ingresa el email asociado a la cuenta root.
2. El sistema muestra el mismo mensaje neutral usado para solicitudes válidas.
3. El sistema no envía enlace de recuperación.

### FA-003: Enlace expirado

1. El usuario abre un enlace pasados más de 15 minutos desde su generación.
2. El sistema informa que el enlace ya no es válido.
3. El sistema permite al usuario solicitar un nuevo enlace.

### FA-004: Enlace ya utilizado

1. El usuario intenta usar un enlace que ya fue utilizado.
2. El sistema informa que el enlace no es válido.
3. El sistema permite solicitar un nuevo enlace.

### FA-005: Límite de solicitudes alcanzado

1. El usuario supera el máximo de solicitudes permitidas para un mismo email.
2. El sistema no procesa una nueva solicitud temporalmente.
3. El sistema muestra un mensaje indicando que debe esperar antes de intentarlo nuevamente.

## Sad Paths / Casos de Error

1. **Email vacío:** el sistema indica que el campo email es obligatorio.
2. **Email inválido:** el sistema indica que el formato del email no es válido.
3. **Contraseña vacía:** el sistema indica que la contraseña es obligatoria.
4. **Confirmación vacía:** el sistema indica que la confirmación es obligatoria.
5. **Contraseñas no coinciden:** el sistema informa que ambas contraseñas deben ser iguales.
6. **Contraseña débil:** el sistema muestra las reglas mínimas incumplidas.
7. **Enlace inválido:** el sistema informa que el enlace no es válido y ofrece solicitar uno nuevo.

## Reglas de Negocio

1. El sistema no debe revelar si un email está registrado.
2. Si el email corresponde a una cuenta válida y no root, se envía un enlace de recuperación.
3. Si el email no existe, el sistema muestra el mismo mensaje neutral y no envía enlace.
4. Si el email corresponde a la cuenta root, el sistema muestra el mismo mensaje neutral y no envía enlace.
5. El enlace de recuperación expira a los 15 minutos desde su generación.
6. El enlace de recuperación es de un solo uso.
7. Al usar correctamente el enlace, este debe invalidarse.
8. La nueva contraseña debe ser una contraseña fuerte.
9. Al cambiar correctamente la contraseña, deben cerrarse las sesiones activas del usuario.
10. Debe existir un límite de solicitudes por email.
11. El usuario no debe iniciar sesión automáticamente después del cambio de contraseña.

## Validaciones

### Email

1. El campo email es obligatorio.
2. El email debe tener un formato válido.

### Nueva contraseña

La contraseña debe cumplir todas las siguientes reglas:

1. Mínimo 8 caracteres.
2. Al menos una letra mayúscula.
3. Al menos una letra minúscula.
4. Al menos un número.
5. Al menos un carácter especial.

### Confirmación de contraseña

1. La confirmación es obligatoria.
2. La confirmación debe coincidir exactamente con la nueva contraseña.

## Mensajes de Usuario

### Solicitud recibida

> Si el email ingresado está asociado a una cuenta, recibirás instrucciones para recuperar tu contraseña.

### Email obligatorio

> Ingresa el email asociado a tu cuenta.

### Email inválido

> Ingresa un email válido.

### Límite de solicitudes alcanzado

> Has realizado varias solicitudes recientemente. Espera unos minutos antes de intentarlo nuevamente.

### Enlace expirado

> Este enlace de recuperación ha expirado. Solicita uno nuevo para continuar.

### Enlace inválido o utilizado

> Este enlace de recuperación no es válido. Solicita uno nuevo para continuar.

### Contraseña débil

> La contraseña debe tener al menos 8 caracteres, una mayúscula, una minúscula, un número y un carácter especial.

### Contraseñas no coinciden

> Las contraseñas ingresadas no coinciden.

### Cambio exitoso

> Tu contraseña fue actualizada correctamente. Por seguridad, se cerraron tus sesiones activas. Ya puedes iniciar sesión con tu nueva contraseña.

## Texto del Email de Recuperación

**Asunto:** Restablecimiento de contraseña

**Cuerpo:**

> Hola,
>
> Recibimos una solicitud para restablecer la contraseña de tu cuenta.
>
> Para crear una nueva contraseña, utiliza el siguiente enlace:
>
> [Restablecer contraseña]
>
> Este enlace estará disponible durante 15 minutos.
>
> Si no solicitaste este cambio, puedes ignorar este mensaje. Tu contraseña actual seguirá siendo válida.
>
> Gracias.

## Estados

1. **Solicitud pendiente:** el usuario ingresó el email y el sistema procesó la solicitud.
2. **Email enviado:** el sistema envió instrucciones a una cuenta válida y no root.
3. **Enlace vigente:** el enlace está dentro de los 15 minutos y no ha sido usado.
4. **Enlace expirado:** el enlace superó los 15 minutos.
5. **Enlace utilizado:** el enlace ya fue usado correctamente.
6. **Contraseña actualizada:** la nueva contraseña fue guardada correctamente.
7. **Sesiones cerradas:** las sesiones activas del usuario fueron invalidadas tras el cambio.

## Datos Requeridos

1. Email ingresado por el usuario.
2. Nueva contraseña.
3. Confirmación de nueva contraseña.
4. Fecha y hora de generación del enlace.
5. Estado del enlace: vigente, expirado o utilizado.
6. Registro de cantidad de solicitudes por email para aplicar límites.

## Dependencias

1. Servicio de envío de emails.
2. Módulo de autenticación de usuarios.
3. Mecanismo funcional para invalidar sesiones activas.
4. Configuración de cuenta root excluida del flujo.

## Consideraciones de Seguridad Funcional

1. No revelar si un email existe o no en el sistema.
2. No permitir recuperación de contraseña para la cuenta root.
3. Usar enlaces temporales de 15 minutos.
4. Invalidar enlaces después de su uso.
5. Aplicar límite de solicitudes por email.
6. Exigir contraseña fuerte.
7. Cerrar sesiones activas después del cambio exitoso.
8. Registrar eventos relevantes para auditoría funcional.

## Criterios de Aceptación

### CA-001: Solicitud con email válido

**Dado** que el usuario está en la pantalla de recuperación de contraseña,  
**cuando** ingresa un email con formato válido y envía la solicitud,  
**entonces** el sistema muestra un mensaje neutral indicando que recibirá instrucciones si el email está asociado a una cuenta.

### CA-002: Email inválido

**Dado** que el usuario está en la pantalla de recuperación,  
**cuando** ingresa un email con formato inválido,  
**entonces** el sistema muestra un mensaje de validación y no procesa la solicitud.

### CA-003: Cuenta root excluida

**Dado** que se ingresa el email asociado a la cuenta root,  
**cuando** se solicita la recuperación de contraseña,  
**entonces** el sistema muestra el mensaje neutral,  
**y** no envía un enlace de recuperación.

### CA-004: Enlace válido

**Dado** que el usuario recibió un enlace de recuperación vigente,  
**cuando** abre el enlace dentro de los 15 minutos,  
**entonces** el sistema muestra el formulario para crear nueva contraseña.

### CA-005: Enlace expirado

**Dado** que el usuario abre un enlace después de 15 minutos,  
**cuando** intenta continuar con la recuperación,  
**entonces** el sistema informa que el enlace expiró y permite solicitar uno nuevo.

### CA-006: Contraseña fuerte válida

**Dado** que el usuario está en el formulario de nueva contraseña,  
**cuando** ingresa una contraseña que cumple todas las reglas y la confirma correctamente,  
**entonces** el sistema actualiza la contraseña.

### CA-007: Contraseña débil

**Dado** que el usuario está en el formulario de nueva contraseña,  
**cuando** ingresa una contraseña que no cumple las reglas de seguridad,  
**entonces** el sistema muestra los requisitos incumplidos y no actualiza la contraseña.

### CA-008: Cierre de sesiones activas

**Dado** que el usuario restableció correctamente su contraseña,  
**cuando** el cambio se confirma,  
**entonces** el sistema cierra las sesiones activas del usuario.

### CA-009: Límite de solicitudes por email

**Dado** que un email alcanzó el límite de solicitudes permitido,  
**cuando** se intenta generar una nueva solicitud,  
**entonces** el sistema rechaza temporalmente la solicitud y muestra un mensaje de espera.

## BDD

```gherkin
Feature: Recuperación de contraseña por email

  Scenario: Solicitar recuperación con email válido
    Given el usuario está en la pantalla "Olvidé mi contraseña"
    When ingresa un email con formato válido
    And envía la solicitud
    Then el sistema muestra un mensaje neutral de envío de instrucciones

  Scenario: Restablecer contraseña correctamente
    Given el usuario abrió un enlace de recuperación válido
    When ingresa una nueva contraseña fuerte
    And confirma la contraseña correctamente
    Then el sistema actualiza la contraseña
    And invalida el enlace utilizado
    And cierra las sesiones activas del usuario
    And muestra un mensaje de éxito

  Scenario: Intentar usar un enlace expirado
    Given el usuario tiene un enlace de recuperación generado hace más de 15 minutos
    When abre el enlace
    Then el sistema informa que el enlace ya no es válido
    And ofrece solicitar uno nuevo

  Scenario: Intentar recuperar contraseña de la cuenta root
    Given existe una cuenta root excluida del flujo de recuperación
    When se solicita recuperación usando el email de la cuenta root
    Then el sistema muestra un mensaje neutral
    And no envía enlace de recuperación

  Scenario: Superar el límite de solicitudes por email
    Given un email alcanzó el límite de solicitudes permitido
    When se solicita un nuevo enlace de recuperación para ese email
    Then el sistema rechaza temporalmente la solicitud
    And muestra un mensaje indicando que debe esperar
```

## Mockup ASCII

```text
+----------------------------------+
| Recuperar contraseña             |
+----------------------------------+
| Ingresa el email asociado        |
| a tu cuenta.                     |
|                                  |
| Email                            |
| [ usuario@email.com          ]   |
|                                  |
| [ Enviar instrucciones ]         |
|                                  |
| Volver a iniciar sesión          |
+----------------------------------+
```

```text
+----------------------------------+
| Crear nueva contraseña           |
+----------------------------------+
| Nueva contraseña                 |
| [ ***************            ]   |
|                                  |
| Confirmar contraseña             |
| [ ***************            ]   |
|                                  |
| Requisitos:                      |
| - Mínimo 8 caracteres            |
| - Una mayúscula                  |
| - Una minúscula                  |
| - Un número                      |
| - Un carácter especial           |
|                                  |
| [ Actualizar contraseña ]        |
+----------------------------------+
```

## Supuestos Funcionales

1. El límite de solicitudes por email será de máximo 3 solicitudes cada 15 minutos, salvo ajuste posterior.
2. El sistema cuenta con un mecanismo para identificar la cuenta root.
3. El cierre de sesiones activas aplica solo al usuario que restableció la contraseña.
4. El usuario deberá iniciar sesión manualmente después de restablecer la contraseña.

## Preguntas Abiertas

1. ¿Se confirma el límite exacto de 3 solicitudes por email cada 15 minutos?
2. ¿Se requiere registrar eventos de recuperación en una pantalla de auditoría visible para administradores?
3. ¿El email de confirmación posterior al cambio exitoso es necesario o basta con el mensaje en pantalla?

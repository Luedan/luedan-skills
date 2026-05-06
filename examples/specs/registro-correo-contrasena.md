# Spec: Registro con correo y contraseña

## Historia de Usuario

Como usuario nuevo, quiero poder registrarme en la plataforma usando mi correo electrónico y una contraseña, para crear una cuenta personal y acceder al sistema una vez validada mi cuenta.

## Contexto

La plataforma necesita permitir que nuevos usuarios creen una cuenta propia antes de acceder a funcionalidades privadas. El registro debe confirmar que el usuario tiene acceso al correo electrónico ingresado mediante un código numérico enviado por correo.

## Objetivo

Permitir el registro de usuarios nuevos usando correo electrónico y contraseña, dejando la cuenta pendiente de validación hasta que el usuario confirme el código numérico recibido por correo.

## Alcance

1. Registro mediante correo electrónico y contraseña.
2. Validación de formato del correo electrónico.
3. Validación de campos obligatorios.
4. Verificación de correo electrónico único.
5. Creación de cuenta en estado pendiente de validación.
6. Envío de código numérico al correo registrado.
7. Validación de cuenta mediante el código recibido.
8. Bloqueo de inicio de sesión para cuentas no validadas.
9. Mensajes funcionales para errores esperados.

## Fuera de Alcance

1. Recuperación de contraseña.
2. Inicio de sesión mediante redes sociales.
3. Registro con nombre de usuario.
4. Registro con número telefónico.
5. Captcha o verificación anti-bots.
6. Perfil de usuario o datos personales adicionales.
7. Validación manual por administrador.
8. Cambios de correo electrónico después del registro.

## Actores

1. Usuario nuevo: persona que quiere crear una cuenta en la plataforma.
2. Sistema: valida los datos, crea la cuenta y controla el estado de validación.
3. Servicio de correo: entrega el código numérico al correo registrado.

## Precondiciones

1. El usuario no tiene una cuenta registrada con el correo electrónico ingresado.
2. El usuario tiene acceso al correo electrónico que usará para registrarse.
3. El sistema cuenta con capacidad de enviar correos electrónicos.
4. El formulario de registro está disponible para usuarios no autenticados.

## Criterios de Aceptación

1. El usuario puede acceder a un formulario de registro.
2. El formulario solicita correo electrónico y contraseña.
3. El sistema valida que el correo electrónico tenga un formato válido.
4. El sistema valida que la contraseña sea obligatoria.
5. Si el correo electrónico ya está registrado, el sistema muestra un mensaje de error claro.
6. Si los datos son válidos, el sistema crea la cuenta en estado pendiente de validación.
7. El sistema envía un código numérico al correo electrónico registrado.
8. El usuario debe ingresar el código numérico recibido para validar su cuenta.
9. Si el código es correcto, la cuenta queda validada.
10. Si el código es incorrecto, el sistema muestra un mensaje de error.
11. Una vez validada la cuenta, el usuario puede iniciar sesión con su correo y contraseña.

## Criterios de No Aceptación

1. La historia no se considera completa si permite registrar dos cuentas con el mismo correo electrónico.
2. La historia no se considera completa si permite iniciar sesión con una cuenta pendiente de validación.
3. La historia no se considera completa si no envía un código numérico al correo registrado.
4. La historia no se considera completa si no informa errores de campos obligatorios.
5. La historia no se considera completa si no informa cuando el código ingresado es incorrecto.
6. La historia no se considera completa si crea la cuenta como validada sin confirmar el código.

## Happy Path

1. El usuario accede al formulario de registro.
2. El usuario ingresa un correo electrónico válido y una contraseña.
3. El sistema valida que el correo tenga formato correcto y que no exista previamente.
4. El sistema crea la cuenta en estado pendiente de validación.
5. El sistema envía un código numérico al correo electrónico registrado.
6. El usuario ingresa el código numérico recibido.
7. El sistema valida que el código sea correcto.
8. El sistema marca la cuenta como validada.
9. El usuario puede iniciar sesión con su correo electrónico y contraseña.

## Flujos Alternativos

1. El usuario puede navegar desde el formulario de registro hacia la pantalla de inicio de sesión si ya tiene una cuenta.
2. El usuario puede solicitar el reenvío del código si no lo recibió.
3. El usuario puede volver al formulario de registro antes de completar la validación.
4. El usuario puede intentar validar la cuenta después de haber cerrado la pantalla de registro, siempre que conserve el flujo de validación disponible.

## Sad Path

1. Si el usuario deja el correo electrónico vacío, el sistema informa que el campo es obligatorio.
2. Si el usuario ingresa un correo electrónico con formato inválido, el sistema informa que el correo no es válido.
3. Si el usuario deja la contraseña vacía, el sistema informa que el campo es obligatorio.
4. Si el correo electrónico ya está registrado, el sistema informa que no se puede crear una nueva cuenta con ese correo.
5. Si el sistema no puede enviar el código numérico, informa que la validación no pudo iniciarse.
6. Si el usuario ingresa un código incorrecto, el sistema informa que el código no es válido.
7. Si el usuario intenta iniciar sesión sin validar la cuenta, el sistema informa que debe completar la validación primero.

## Reglas de Negocio

1. El correo electrónico identifica de forma única a cada cuenta.
2. Una cuenta recién registrada queda en estado pendiente de validación.
3. Una cuenta pendiente de validación no puede iniciar sesión.
4. El código numérico se envía únicamente al correo electrónico registrado.
5. El código numérico debe corresponder a la cuenta pendiente de validación.
6. La cuenta solo cambia a validada cuando el usuario ingresa el código correcto.
7. El sistema no debe crear una segunda cuenta con un correo electrónico ya registrado.

## Validaciones

1. Correo electrónico: obligatorio.
2. Correo electrónico: debe tener formato válido.
3. Correo electrónico: debe ser único en la plataforma.
4. Contraseña: obligatoria.
5. Código de validación: obligatorio.
6. Código de validación: debe ser numérico.
7. Código de validación: debe coincidir con el código enviado al correo registrado.

## Mensajes al Usuario

1. `Ingresa un correo electrónico.`
2. `Ingresa un correo electrónico válido.`
3. `Este correo electrónico ya está registrado.`
4. `Ingresa una contraseña.`
5. `Te enviamos un código de validación a tu correo electrónico.`
6. `El código ingresado no es válido.`
7. `Tu cuenta fue validada correctamente.`
8. `Debes validar tu cuenta antes de iniciar sesión.`
9. `No pudimos enviar el código de validación. Intenta nuevamente.`

## Estados

1. Cuenta pendiente de validación: cuenta creada, pero todavía no confirmada por código.
2. Cuenta validada: cuenta confirmada y habilitada para iniciar sesión.
3. Código enviado: código generado y enviado al correo registrado.
4. Código válido: código que puede usarse para validar la cuenta.
5. Código incorrecto: código ingresado que no coincide con el esperado.

## Datos Requeridos

1. Correo electrónico.
2. Contraseña.
3. Código numérico de validación.

## Dependencias

1. Servicio de envío de correos electrónicos.
2. Módulo de autenticación.
3. Pantalla o flujo de inicio de sesión.
4. Persistencia de cuentas y estados de validación.

## Métricas o Eventos

1. Registro iniciado.
2. Registro completado.
3. Código de validación enviado.
4. Validación de cuenta exitosa.
5. Validación de cuenta fallida.
6. Intento de inicio de sesión con cuenta no validada.

## Consideraciones de Seguridad

1. La contraseña no debe mostrarse en texto plano mientras el usuario la escribe.
2. La contraseña no debe exponerse en mensajes, pantallas, logs o respuestas visibles.
3. El inicio de sesión debe estar bloqueado para cuentas pendientes de validación.
4. El reenvío de códigos debe evitar abuso o envíos excesivos.
5. Los mensajes de error deben ser claros sin exponer información sensible innecesaria.

## Consideraciones de Accesibilidad

1. Todos los campos deben tener etiquetas visibles o accesibles.
2. Los errores deben estar asociados al campo correspondiente.
3. Los mensajes de error no deben depender únicamente del color.
4. El flujo debe poder completarse usando teclado.
5. El foco debe guiar al usuario hacia errores o acciones relevantes.

## Escenarios BDD

```gherkin
Feature: Registro con correo y contraseña

  Scenario: Registro exitoso con validación por código
    Given que el usuario no tiene una cuenta registrada
    When ingresa un correo electrónico válido y una contraseña
    And confirma el registro
    Then el sistema crea la cuenta en estado pendiente de validación
    And envía un código numérico al correo electrónico registrado

  Scenario: Validación exitosa de cuenta
    Given que el usuario tiene una cuenta pendiente de validación
    And recibió un código numérico por correo electrónico
    When ingresa el código numérico correcto
    Then el sistema valida la cuenta
    And el usuario puede iniciar sesión con su correo electrónico y contraseña

  Scenario: Registro con correo electrónico inválido
    Given que el usuario está en el formulario de registro
    When ingresa un correo electrónico con formato inválido
    And confirma el registro
    Then el sistema muestra un mensaje indicando que el correo electrónico no es válido
    And no crea la cuenta

  Scenario: Registro con correo electrónico ya registrado
    Given que ya existe una cuenta registrada con el correo electrónico ingresado
    When el usuario intenta registrarse con ese correo electrónico
    Then el sistema muestra un mensaje indicando que el correo electrónico ya está registrado
    And no crea una nueva cuenta

  Scenario: Validación con código incorrecto
    Given que el usuario tiene una cuenta pendiente de validación
    When ingresa un código numérico incorrecto
    Then el sistema muestra un mensaje indicando que el código no es válido
    And la cuenta permanece pendiente de validación

  Scenario: Inicio de sesión con cuenta no validada
    Given que el usuario tiene una cuenta pendiente de validación
    When intenta iniciar sesión con su correo electrónico y contraseña
    Then el sistema muestra un mensaje indicando que debe validar su cuenta primero
    And no permite el acceso al sistema
```

## Asunciones Funcionales

1. El registro se realiza con correo electrónico y contraseña.
2. No se usará nombre de usuario.
3. La cuenta requiere validación antes de considerarse activa.
4. La validación se realiza mediante un código numérico enviado por correo.
5. El correo electrónico debe ser único en la plataforma.
6. El usuario no queda autenticado automáticamente hasta validar la cuenta.
7. No se incluye recuperación de contraseña en esta historia.
8. No se incluye registro mediante redes sociales.
9. No se incluye captcha ni verificación anti-bots.
10. No se solicitan datos personales adicionales durante el registro.

## Preguntas Abiertas

1. ¿Cuántos dígitos debe tener el código numérico?
2. ¿Cuánto tiempo debe permanecer vigente el código?
3. ¿Cuántos intentos fallidos de validación se permiten antes de bloquear o limitar el flujo?
4. ¿Cuántas veces puede reenviarse el código?
5. ¿La contraseña debe tener reglas mínimas de longitud o complejidad?
6. ¿El sistema debe ocultar si un correo ya está registrado por razones de privacidad?
7. ¿Qué debe ocurrir si el usuario inicia el registro, pero nunca valida la cuenta?

## Mockup ASCII

### Registro

```text
+--------------------------------------------------+
|                    PLATAFORMA                    |
+--------------------------------------------------+
|                                                  |
|                  Crear cuenta                    |
|                                                  |
|  Correo electrónico                              |
|  +--------------------------------------------+  |
|  | usuario@correo.com                          |  |
|  +--------------------------------------------+  |
|                                                  |
|  Contraseña                                     |
|  +--------------------------------------------+  |
|  | ************                               |  |
|  +--------------------------------------------+  |
|                                                  |
|  +--------------------------------------------+  |
|  |              Registrarme                   |  |
|  +--------------------------------------------+  |
|                                                  |
|  ¿Ya tienes cuenta? Iniciar sesión              |
|                                                  |
+--------------------------------------------------+
```

### Validación de cuenta

```text
+--------------------------------------------------+
|                    PLATAFORMA                    |
+--------------------------------------------------+
|                                                  |
|              Verifica tu cuenta                  |
|                                                  |
|  Enviamos un código numérico a:                  |
|  usuario@correo.com                              |
|                                                  |
|  Código de verificación                          |
|  +------+  +------+  +------+  +------+          |
|  |  1   |  |  2   |  |  3   |  |  4   |          |
|  +------+  +------+  +------+  +------+          |
|                                                  |
|  +--------------------------------------------+  |
|  |              Validar cuenta                |  |
|  +--------------------------------------------+  |
|                                                  |
|  ¿No recibiste el código? Reenviar código        |
|                                                  |
+--------------------------------------------------+
```

### Error: correo inválido

```text
+--------------------------------------------------+
|                    PLATAFORMA                    |
+--------------------------------------------------+
|                                                  |
|                  Crear cuenta                    |
|                                                  |
|  Correo electrónico                              |
|  +--------------------------------------------+  |
|  | usuario@correo                              |  |
|  +--------------------------------------------+  |
|  ! Ingresa un correo electrónico válido          |
|                                                  |
|  Contraseña                                     |
|  +--------------------------------------------+  |
|  | ************                               |  |
|  +--------------------------------------------+  |
|                                                  |
|  +--------------------------------------------+  |
|  |              Registrarme                   |  |
|  +--------------------------------------------+  |
|                                                  |
+--------------------------------------------------+
```

### Error: código incorrecto

```text
+--------------------------------------------------+
|                    PLATAFORMA                    |
+--------------------------------------------------+
|                                                  |
|              Verifica tu cuenta                  |
|                                                  |
|  Código de verificación                          |
|  +------+  +------+  +------+  +------+          |
|  |  9   |  |  8   |  |  7   |  |  6   |          |
|  +------+  +------+  +------+  +------+          |
|                                                  |
|  ! El código ingresado no es válido              |
|                                                  |
|  +--------------------------------------------+  |
|  |              Validar cuenta                |  |
|  +--------------------------------------------+  |
|                                                  |
|  Reenviar código                                |
|                                                  |
+--------------------------------------------------+
```

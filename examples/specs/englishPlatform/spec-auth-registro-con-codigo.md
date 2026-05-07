# Spec: Auth y registro con código

## Objetivo

Definir el flujo completo de autenticación y creación de cuenta mediante código de acceso de un solo uso, incluyendo validaciones, consumo del código, vínculo vitalicio con la cuenta y control de acceso posterior.

## Alcance

Incluye:

1. Pantalla de login.
2. Pantalla de registro.
3. Registro con email, contraseña y código.
4. Validación y consumo de código.
5. Inicio y cierre de sesión.
6. Restricción de acceso por sesión y rol.

## Fuera de alcance

1. OAuth social.
2. Recuperación de contraseña.
3. Verificación de email por correo, salvo decisión futura.
4. Multi-factor authentication.

## Dependencias

1. `spec-modelo-datos-plataforma.md`
2. `spec-base-app-y-arquitectura.md`
3. `spec-plataforma-aprender-ingles.md`

## Actores y permisos

### visitor
- puede registrarse
- puede iniciar sesión

### student
- puede iniciar/cerrar sesión
- puede acceder a zona privada

### admin
- puede iniciar/cerrar sesión
- puede acceder a zona admin

## Rutas / pantallas

1. `/login`
2. `/register`
3. `/app/dashboard`
4. `/admin`

## Componentes UI

1. Formulario de login.
2. Formulario de registro.
3. Campo de código de acceso.
4. Mensajes inline de validación.
5. Estados de loading y feedback.
6. Botón de logout en zonas autenticadas.

## Server Actions / operaciones servidor

### 1. `registerWithAccessCode`
Responsabilidades:
- validar payload
- verificar existencia del código
- verificar que el código esté activo
- verificar que no haya sido consumido
- crear usuario
- marcar código como consumido
- vincular código a usuario
- iniciar sesión si se define flujo auto-login o redirigir a login

### 2. `loginWithCredentials`
Responsabilidades:
- autenticar usuario
- validar estado de la cuenta
- redirigir según rol

### 3. `logout`
Responsabilidades:
- cerrar sesión
- redirigir a login o home

## Modelo de datos afectado

1. `User`
2. `AccessCode`
3. `AuditLog` opcional para eventos sensibles o fallidos relevantes

## Flujo principal de registro

1. El visitante entra en `/register`.
2. Completa nombre, email, contraseña y código.
3. El sistema valida formato.
4. El servidor valida que el código exista y esté disponible.
5. El sistema crea la cuenta.
6. El código queda consumido.
7. El acceso vitalicio queda vinculado a esa cuenta.
8. El usuario es dirigido al login o entra directamente si así se define.

## Flujo principal de login

1. El usuario entra en `/login`.
2. Introduce email y contraseña.
3. El sistema valida credenciales.
4. Si el usuario es `student`, entra a `/app/dashboard`.
5. Si el usuario es `admin`, entra a `/admin`.

## Reglas de negocio

1. No se puede crear una cuenta sin código válido.
2. Un código solo puede usarse una vez.
3. Un código consumido no puede reutilizarse.
4. El acceso concedido queda vinculado de por vida a la cuenta creada.
5. El email debe ser único.
6. El sistema debe diferenciar rutas por rol.
7. Debe quedar claro en términos y condiciones que el código es de un solo uso.

## Validaciones

### Registro
1. nombre obligatorio
2. email obligatorio y válido
3. contraseña obligatoria
4. código obligatorio
5. email no existente
6. código existente
7. código activo
8. código no consumido

### Login
1. email obligatorio
2. contraseña obligatoria
3. credenciales válidas
4. cuenta en estado permitido

## Estados y errores

1. registro exitoso
2. email ya registrado
3. código inválido
4. código inactivo
5. código ya utilizado
6. login correcto
7. login incorrecto
8. sesión expirada
9. acceso denegado por rol

## Criterios de aceptación

### CA-001 Registro válido
**Dado** un visitante sin cuenta  
**Cuando** envía datos válidos junto con un código activo y no consumido  
**Entonces** el sistema crea la cuenta  
**Y** consume el código.

### CA-002 Rechazo por código consumido
**Dado** un visitante sin cuenta  
**Cuando** intenta registrarse con un código ya consumido  
**Entonces** el sistema rechaza la operación  
**Y** no crea la cuenta.

### CA-003 Redirección por rol estudiante
**Dado** un estudiante autenticado  
**Cuando** inicia sesión correctamente  
**Entonces** el sistema lo redirige a `/app/dashboard`.

### CA-004 Redirección por rol admin
**Dado** un administrador autenticado  
**Cuando** inicia sesión correctamente  
**Entonces** el sistema lo redirige a `/admin`.

### CA-005 Logout
**Dado** un usuario autenticado  
**Cuando** ejecuta logout  
**Entonces** el sistema cierra la sesión  
**Y** bloquea el acceso a rutas privadas hasta un nuevo login.

## Casos límite

1. Doble envío del formulario de registro.
2. Código válido consumido por carrera de concurrencia entre dos intentos.
3. Usuario autenticado visitando `/login` o `/register`.
4. Cuenta bloqueada intentando iniciar sesión.

## Notas técnicas

1. Se recomienda **Auth.js credentials provider** para email/password.
2. La validación del código y su consumo deben ejecutarse dentro de una transacción.
3. Debe evitarse la enumeración innecesaria de emails existentes en mensajes públicos.
4. La validación de formulario debe hacerse con **Zod**.
5. El formulario debe usar componentes **shadcn/ui** y estilos con **Tailwind CSS**.
6. El consumo del código debe ser atómico para evitar reutilización por concurrencia.

## Definición de terminado

Se considerará terminado este módulo cuando:

1. El flujo de registro con código esté especificado de extremo a extremo.
2. Los errores principales estén definidos.
3. Estén claras las rutas, validaciones y redirecciones por rol.
4. Un agente pueda implementar login, registro y guards sin ambigüedad funcional crítica.

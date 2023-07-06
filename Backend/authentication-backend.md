# Reto Técnico Backend: Gestión de Autenticación de Usuarios

## Descripción
En este reto técnico, se te solicita desarrollar una API RESTful para gestionar la autenticación de usuarios. Deberás implementar la funcionalidad de registro, inicio de sesión y autenticación basada en tokens.

## Requerimientos

1. Crear un endpoint de registro (`POST /api/register`) que permita a los usuarios registrarse proporcionando un nombre de usuario y una contraseña. Los usuarios deben almacenarse en una base de datos.

2. Implementar un endpoint de inicio de sesión (`POST /api/login`) que permita a los usuarios autenticarse utilizando su nombre de usuario y contraseña. Deberás generar un token de acceso seguro para el usuario.

3. Crear un middleware de autenticación que verifique la validez del token de acceso en cada solicitud protegida. Si el token no es válido o ha expirado, se debe devolver un código de estado de error.

## Instrucciones

1. Configura el proyecto de Node.js utilizando el framework de tu elección (por ejemplo, Express.js).

2. Implementa los endpoints `/api/register` y `/api/login` para manejar el registro y el inicio de sesión de los usuarios.

3. Utiliza una base de datos para almacenar y recuperar la información de los usuarios registrados. Puedes utilizar cualquier base de datos que prefieras (por ejemplo, MongoDB, MySQL, etc.).

4. Genera un token de acceso seguro cuando un usuario inicia sesión correctamente. El token debe contener la información necesaria para autenticar las solicitudes posteriores del usuario.

5. Implementa un middleware de autenticación que verifique la validez del token de acceso en cada solicitud protegida. Puedes utilizar cualquier biblioteca o método de autenticación de tokens que prefieras (por ejemplo, JWT).

## Requerimientos adicionales (opcional)

Si tienes tiempo adicional, puedes agregar las siguientes funcionalidades adicionales a tu aplicación:

- Implementar endpoints para restablecer la contraseña del usuario y enviar correos electrónicos de confirmación.
- Agregar un mecanismo de autorización para controlar los roles y permisos de los usuarios (por ejemplo, roles de administrador y usuario regular).
- Mejorar la seguridad de la autenticación implementando técnicas como hash de contraseñas y salting.

Recuerda que el objetivo de este reto técnico es evaluar tus habilidades como desarrollador backend, específicamente en la gestión de la autenticación de usuarios. No te preocupes si no logras implementar todos los requerimientos adicionales. Lo más importante es mostrar tu comprensión de los conceptos básicos y crear una API funcional de autenticación de usuarios. ¡Buena suerte!
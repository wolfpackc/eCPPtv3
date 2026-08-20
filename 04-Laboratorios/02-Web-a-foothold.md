# Laboratorio 02: de web a foothold

## Objetivo

Pasar de una IP con HTTP a una identidad o shell del sistema mediante una cadena web autorizada.

## Diseño sugerido

Configura deliberadamente:

- un virtual host no enlazado;
- un backup o comentario que revele tecnología;
- autenticación con una debilidad controlada;
- una SQLi, command injection, LFI o subida insegura;
- credenciales de base de datos distintas de las personales;
- una ruta de retorno alcanzable.

## Tareas

1. Identifica dominio/virtual hosts.
2. Mapea rutas, métodos, parámetros y tecnologías.
3. Captura en Burp una petición base por función.
4. Descubre la vulnerabilidad manualmente.
5. Demuestra impacto mínimo.
6. Si usas automatización, compara su resultado con la prueba manual.
7. Obtén un secreto o ejecución.
8. Consigue acceso inicial y registra el usuario del proceso.
9. Enumera sistema y red sin escalar todavía.

## Variantes

- resuelve una vez con SQLmap y repite sin SQLmap;
- bloquea salida directa a Kali y añade un forward;
- haz que el secreto web sirva para SSH pero no para root;
- ejecuta la aplicación en un contenedor para discutir fronteras.

## Preguntas de control

- ¿Qué frontera de confianza se rompió?
- ¿Qué evidencia confirma la vulnerabilidad sin depender de una shell?
- ¿Por qué el upload se ejecuta o no?
- ¿La credencial pertenece a DB, aplicación o sistema?
- ¿Qué usuario y grupos tiene la shell?

## Aprobado

Puedes repetir la cadena desde cero, explicar cada petición y distinguir claramente vulnerabilidad, payload, canal de retorno y privilegio obtenido.


# Laboratorio 03: postexplotación y PrivEsc

## Objetivo

Escalar en Linux y Windows mediante configuraciones débiles y recuperar una credencial que permita un movimiento lateral.

## Escenario Linux

Incluye dos rutas, solo una intencionada:

- tarea root que ejecuta script modificable;
- permiso sudo limitado o capability mal asignada;
- credencial en archivo de aplicación;
- kernel sin un exploit necesario para evitar el atajo.

Tareas:

1. Enumeración manual.
2. LinPEAS para comparar cobertura.
3. Confirmación de permisos y contexto.
4. Escalada con prueba mínima.
5. Explicación defensiva.

## Escenario Windows

Incluye:

- servicio o tarea con recurso modificable;
- privilegio de token que debe interpretarse;
- credencial en configuración o Credential Manager;
- un hallazgo rojo de WinPEAS que sea falso positivo práctico.

Tareas:

1. `whoami /all` y enumeración nativa.
2. WinPEAS y priorización.
3. Verificar ACL del servicio/archivo.
4. Determinar cuenta de ejecución y capacidad de reinicio.
5. Escalar a SYSTEM en el lab.
6. Extraer únicamente el secreto preparado.

## Movimiento lateral

La credencial encontrada debe autenticar en un segundo host pero con permisos diferentes según SMB, WinRM o RDP. Construye matriz y explica las diferencias.

## Aprobado

Resuelves ambos sistemas sin herramienta automatizada en la segunda vuelta y puedes demostrar la precondición exacta de cada escalada.


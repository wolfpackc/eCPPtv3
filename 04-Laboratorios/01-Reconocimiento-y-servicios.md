# Laboratorio 01: reconocimiento y servicios

## Objetivo

Construir un inventario fiable de una red desconocida sin explotar vulnerabilidades.

## Preparación

Incluye como mínimo:

- un Linux con SSH, HTTP y base de datos;
- un Windows miembro con SMB y WinRM;
- un DC con DNS, Kerberos y LDAP;
- un servicio en puerto no estándar;
- un host que no responda a ICMP;
- al menos un puerto UDP útil.

## Tareas

1. Identifica interfaz y rutas de Kali.
2. Descubre hosts mediante más de una clase de sonda.
3. Escanea todos los puertos TCP.
4. Escanea UDP selectivo.
5. Enumera versiones y protocolos.
6. Determina qué host es DC y justifica con tres evidencias.
7. Enumera DNS, HTTP y SMB manualmente.
8. Construye una tabla de siguientes acciones.

## Restricciones

- no usar `-A` como sustituto de metodología;
- no usar exploits;
- no probar contraseñas;
- guardar salida en formatos reutilizables.

## Preguntas de control

- ¿Qué host estaba vivo pero no apareció en `-sn` y por qué?
- ¿Qué diferencia viste entre banner, fingerprint y comportamiento real?
- ¿Qué puertos sustentan tu inferencia de DC?
- ¿Qué servicio necesitó un nombre de host correcto?
- ¿Qué dato nuevo aportó UDP?

## Aprobado

Has encontrado todos los hosts y servicios intencionados, puedes defender cada inferencia y otra persona puede continuar con tu inventario sin repetir los escaneos.


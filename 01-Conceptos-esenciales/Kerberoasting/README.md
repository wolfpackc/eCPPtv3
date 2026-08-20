# Kerberoasting

## Idea esencial

Un usuario autenticado puede solicitar un ticket para un servicio cuyo SPN conoce. Parte de ese TGS está cifrada con una clave vinculada a la contraseña de la cuenta que ejecuta el servicio. El ticket puede auditarse offline.

Kerberoasting no exige ser administrador y no explota el servidor del servicio. Explota la combinación de una función normal de Kerberos con contraseñas débiles o antiguas en cuentas de servicio.

## Flujo

```mermaid
sequenceDiagram
    participant U as Usuario de dominio
    participant K as KDC
    participant O as Crackeo offline
    U->>K: TGS-REQ para SPN objetivo
    K-->>U: TGS cifrado para la cuenta de servicio
    U->>O: Guarda material del ticket
    O-->>U: Contraseña, solo si un candidato coincide
```

## Qué es un SPN

Un Service Principal Name vincula una instancia de servicio con una cuenta, por ejemplo:

```text
MSSQLSvc/SQL01.corp.local:1433
HTTP/intranet.corp.local
```

Los servicios que funcionan con cuentas de usuario administradas manualmente suelen ser más interesantes que los ejecutados por cuentas de equipo con contraseñas largas y rotadas automáticamente.

## Ejemplo de laboratorio

```bash
GetUserSPNs.py 'corp.local/edu:Laboratorio123!' \
  -dc-ip 10.10.10.10 -request -outputfile tgs.hashes
```

Con un ticket o hash compatible, usa la opción de autenticación correspondiente de la versión instalada. Después, identifica el formato y audita offline:

```bash
hashcat -m 13100 tgs.hashes diccionario.txt
```

El tipo de cifrado puede cambiar el modo. Compruébalo antes.

## Priorización

No ataques todos los SPN sin pensar. Prioriza:

- cuentas de usuario de servicio;
- contraseñas que nunca expiran;
- cuentas antiguas;
- servicios críticos;
- cuentas con membresías o ACL privilegiadas;
- material con cifrado susceptible de auditoría eficiente.

## Resultado y siguiente paso

Si recuperas una contraseña:

1. Identifica exactamente la cuenta.
2. Valida con el menor impacto posible.
3. Enumera sus grupos, sesiones, administración local y ACL.
4. Actualiza BloodHound.
5. No asumas que el nombre «svc» implica privilegios altos.

## Mitigación

Usar gMSA cuando sea posible, contraseñas largas y aleatorias, rotación, mínimo privilegio, cifrado moderno y monitorización de solicitudes TGS inusuales.


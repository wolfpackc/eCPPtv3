# SPN — Service Principal Name

Un **SPN (Service Principal Name)** identifica una **instancia concreta de un servicio registrada en Active Directory para poder ser utilizada con Kerberos**.

No significa simplemente que el servicio exista en Active Directory. El SPN identifica el servicio y queda asociado a una cuenta o *security principal*.

```text
SPN → identifica una instancia de servicio registrada en AD
      → está asociado a una cuenta
                           ├─ usuario de servicio (svc_sql, svc_web...)
                           ├─ cuenta de equipo (SERVER01$)
                           └─ MSA / gMSA

AD → Kerberos preferido
     └─ NTLM disponible como compatibilidad/fallback en ciertos casos

Kerberos → necesita DNS + KDC + hora sincronizada
```

## SPN y Kerberoasting

Para Kerberoasting, lo interesante no es simplemente encontrar SPN, sino saber **a qué cuenta está asociado cada SPN**.

```text
Enumeramos SPN
     ↓
vemos qué servicios existen
y a qué cuentas están asociados
     ↓
filtramos los interesantes
     ↓
¿SPN asociado a usuario de servicio?
     │
     ├── svc_sql       ← interesante
     ├── svc_backup    ← interesante
     └── svc_web       ← interesante
             ↓
      solicitar TGS
             ↓
      cracking offline
```

Ejemplo clásico:

```text
MSSQLSvc/sql01.empresa.local
          ↓
EMPRESA\svc_sql
```

Aquí el SPN está asociado a una cuenta de usuario de servicio. Si la contraseña de esa cuenta es débil, el TGS puede resultar útil para un ataque de cracking offline.

En cambio:

```text
cifs/SERVER01.empresa.local
          ↓
SERVER01$
```

Aquí el SPN está asociado a una **cuenta de equipo**, no a un usuario de servicio tradicional. Las cuentas de equipo suelen usar contraseñas largas, aleatorias y gestionadas automáticamente, por lo que crackearlas normalmente no es práctico.

Lo mismo ocurre normalmente con las cuentas administradas:

- **MSA** = Managed Service Account
- **gMSA** = Group Managed Service Account

Ejemplo:

```text
HTTP/web01.empresa.local
        ↓
EMPRESA\webservice$
        ↓
gMSA
        ↓
contraseña larga + gestionada automáticamente
        ↓
Kerberoasting práctico → poco interesante
```

## Frase para memorizar

> **Los SPN me permiten localizar servicios Kerberos y saber qué cuenta los representa; para Kerberoasting me interesan especialmente los SPN asociados a cuentas de usuario de servicio con contraseñas potencialmente crackeables.**

## Matiz importante

Servicios como **SMB, SQL, HTTP, LDAP, etc.** pueden tener SPN, pero no cualquier servicio, FTP o impresora aparece automáticamente como SPN. Debe tratarse de una instancia de servicio registrada en Active Directory para Kerberos.

Sí, mejor dejarlo bastante más compacto para estudiar rápido:

# ACTIVE DIRECTORY + DOMINIO + DNS

En Active Directory, el **dominio** es el nombre lógico que identifica todo el entorno de la organización.

Ejemplos:

* `empresa.com`
* `corp.empresa.com`
* `empresa.local`

Los usuarios pertenecen a ese dominio:

`edu@corp.empresa.com`

o en formato clásico:

`CORP\edu`

Dentro del dominio tienes:

**usuarios + grupos + equipos + OU + GPO + controladores de dominio**

---

# `corp.empresa.com`

`corp.empresa.com` puede ser perfectamente un dominio de Active Directory.

Por ejemplo:

**Dominio público:** `empresa.com`
**Dominio interno AD:** `corp.empresa.com`

`corp` no hace nada especial. Es simplemente un nombre elegido para organizar el dominio.

También podrían haber usado:

`ad.empresa.com`

`internal.empresa.com`

---

# DNS

DNS funciona de forma jerárquica:

`.`
↓
`.com`
↓
`empresa.com`
↓
`corp.empresa.com`

Los registros **NS** permiten delegar una parte del dominio a otros servidores DNS.

Por ejemplo:

`empresa.com`

puede decir:

> Para `corp.empresa.com`, pregunta a estos otros servidores DNS.

---

# DNS EN ACTIVE DIRECTORY

Active Directory **necesita DNS**.

Normalmente los equipos del dominio utilizan un **DNS interno**, muchas veces instalado en los propios controladores de dominio.

Ese DNS conoce, por ejemplo:

`dc01.corp.empresa.com` → `192.168.1.10`

`fileserver.corp.empresa.com` → `192.168.1.20`

Además tiene registros especiales para localizar servicios como:

* Kerberos
* LDAP
* Controladores de dominio

La idea clave es:

**AD necesita DNS para encontrar sus servicios.**

---

# EJEMPLO NOVATECH

NovaTech instala Windows Server y crea:

**Dominio AD:** `corp.novatech.com`

Después crea:

* usuarios
* grupos
* equipos
* GPO

Los PCs se unen al dominio y utilizan el DNS interno.

Cuando Ana inicia sesión:

**Ana escribe**
`ana@corp.novatech.com`

↓

**DNS localiza el DC**

↓

**Kerberos autentica**

↓

**LDAP consulta usuarios y grupos**

↓

**GPO aplica configuraciones**

↓

**SMB/NTFS usa esos grupos para dar acceso a carpetas**

Por ejemplo:

`Ana`
↓
`Grupo Finanzas`
↓
`\\fileserver\finanzas`
↓
✅ Acceso

---

# RESUMEN PARA MEMORIZAR

**Dominio** → identifica el entorno AD.
**DNS** → encuentra el DC.
**Kerberos** → autentica.
**LDAP** → consulta Active Directory.
**GPO** → configura usuarios/equipos.
**SMB/NTFS** → controla acceso a recursos.

### Cadena mental:

**Dominio → DNS → DC → Kerberos → LDAP → GPO → SMB/NTFS**


<img width="1448" height="1086" alt="c590b3a4-1bb9-4e17-afb9-665574a0cc71" src="https://github.com/user-attachments/assets/386ab288-9f62-4e28-9adb-d3b1d7647d6d" />

Imagina una empresa nueva llamada **NovaTech** con 80 empleados. Hasta ahora cada PC tenía usuarios locales y era un caos: cada ordenador con sus propias cuentas, contraseñas y permisos. Instalan un Windows Server, añaden **AD DS** y lo promueven a controlador de dominio. Al crear el dominio deciden llamarlo **`corp.novatech.com`**. Ese nombre no es porque tengan una web, sino porque quieren que **todo el entorno interno de Active Directory tenga un nombre único y ordenado**.

A partir de ahí crean usuarios como `ana@corp.novatech.com`, `carlos@corp.novatech.com` y grupos como `Finanzas`, `RRHH` o `IT`. También configuran los PCs para usar como DNS interno, por ejemplo, `192.168.10.10`, que es el propio DC. Cuando Ana enciende su ordenador e inicia sesión, el PC sabe que pertenece al dominio `corp.novatech.com`. Primero pregunta al DNS interno: **“¿qué controlador de dominio atiende `corp.novatech.com`?”**. El DNS responde, por ejemplo, `dc01.corp.novatech.com`. Entonces el PC ya habla con ese DC por Kerberos para autenticarse.

Después, si Ana abre una carpeta compartida `\\fileserver\finanzas`, el servidor SMB puede comprobar que Ana pertenece al grupo `Finanzas` de Active Directory y permitirle acceso. Aquí `corp.novatech.com` sirve como **identidad común de todo el dominio**: usuarios, equipos, DCs y servicios saben que pertenecen al mismo entorno.

Lo importante es esto: **`corp` no hace magia**. Podrían haber llamado al dominio `ad.novatech.com` o `empresa.local`. `corp.novatech.com` simplemente es el nombre DNS que eligieron para identificar ese dominio interno de Active Directory y organizarlo de forma limpia.

El escenario completo sería:

**NovaTech instala AD → crea dominio `corp.novatech.com` → crea usuarios/grupos → PCs se unen al dominio → DNS interno localiza el DC → Kerberos autentica → LDAP consulta usuarios/grupos → SMB/NTFS usa esos grupos para dar permisos.**


<img width="1122" height="1402" alt="ChatGPT Image 25 ago 2026, 00_15_29" src="https://github.com/user-attachments/assets/cd8ad2f9-5b71-416e-86e4-7c7d61359693" />

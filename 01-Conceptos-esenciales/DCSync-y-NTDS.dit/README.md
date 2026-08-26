# DCSync y NTDS.dit

`NTDS.dit` es, simplificando mucho, la **base de datos principal de Active Directory** que existe en los controladores de dominio. Ahí se almacenan objetos del dominio y, entre otras cosas, material relacionado con credenciales.

La idea ofensiva importante es:

```text
Comprometo privilegios suficientes
        ↓
Obtengo hashes / secretos del dominio
        ↓
Puedo reutilizarlos para autenticación
o para ataques posteriores
```

Con `NTDS.dit`, el atacante intenta acceder a esa base de datos y extraer secretos. Eso suele requerir ya un nivel de compromiso alto del DC.

**DCSync** es diferente: no necesitas copiar directamente `NTDS.dit`. En vez de eso, abusas del **mecanismo de replicación de Active Directory**.

Un controlador de dominio normalmente dice a otro:

> “Dame los cambios de usuarios, contraseñas y objetos para mantenerme sincronizado.”

DCSync intenta hacerse pasar por una entidad con permisos de replicación y pedir esos secretos al DC.

Conceptualmente:

```text
DC legítimo
   ↕
Replicación AD

Atacante con permisos de replicación
        ↓
Solicita secretos como si fuera un DC
        ↓
Obtiene hashes
```

La diferencia clave:

```text
NTDS.dit
= atacar / extraer la base de datos

DCSync
= pedir los secretos mediante replicación
```

Esto conecta directamente con **Golden Ticket** porque, si mediante DCSync obtienes el material criptográfico de `KRBTGT`, ya tienes la pieza necesaria para poder falsificar TGT.
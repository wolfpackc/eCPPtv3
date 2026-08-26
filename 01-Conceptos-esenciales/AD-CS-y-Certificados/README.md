# AD CS y certificados

**AD CS** significa **Active Directory Certificate Services**.

Es una infraestructura de certificados dentro de un entorno Windows.

En vez de autenticarte solo con:

```text
usuario + contraseña
```

también puede existir autenticación basada en:

```text
usuario + certificado
```

Ese certificado puede utilizarse para demostrar identidad.

El problema aparece cuando las plantillas de certificados o los permisos están mal configurados.

Ejemplo conceptual:

```text
Usuario poco privilegiado
        ↓
Puede solicitar un certificado
con propiedades demasiado permisivas
        ↓
El certificado representa a otro usuario
        ↓
Autenticación como usuario privilegiado
```

Eso es lo que hace que AD CS sea tan importante en pentesting moderno.

Luego aparecen nombres como:

```text
ESC1
ESC2
ESC3
ESC4
...
```

Son categorías de configuraciones abusables relacionadas con AD CS. No hace falta aprenderlas todas de golpe: primero conviene entender bien qué papel tienen los certificados y las plantillas dentro de Active Directory.

## Cómo encaja con el resto

```text
DCSync / NTDS.dit
        ↓
ROBAR SECRETOS

Delegación Kerberos
        ↓
IMPERSONAR IDENTIDADES

AD CS
        ↓
AUTENTICARSE MEDIANTE CERTIFICADOS
Y POSIBLEMENTE ESCALAR PRIVILEGIOS
```

Dentro de una cadena general de Active Directory puede encajar así:

```text
Acceso inicial
        ↓
Enumeración
        ↓
Credenciales
        ↓
Kerberoasting / AS-REP
        ↓
ACL / Delegación / AD CS
        ↓
Escalada
        ↓
DCSync / NTDS.dit
        ↓
Hashes de cuentas privilegiadas
        ↓
KRBTGT
        ↓
Golden Ticket / persistencia
```
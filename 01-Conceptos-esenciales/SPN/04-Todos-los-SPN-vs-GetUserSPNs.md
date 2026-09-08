# Todos los SPN vs GetUserSPNs.py

## 1. Si enumero todos los SPN de Active Directory

Si obtengo **todos los SPN del dominio**, me puedo encontrar ** instancias de servicio ** asociadas a distintos tipos de cuentas o principals de Active Directory.

Por ejemplo:

```text
SPN
│
├─ MSSQLSvc/sql01.empresa.local
│   └─ EMPRESA\svc_sql
│      → cuenta de usuario del dominio usada como cuenta de servicio
│
├─ cifs/server01.empresa.local
│   └─ SERVER01$
│      → cuenta de equipo
│
├─ HTTP/web01.empresa.local
│   └─ EMPRESA\websvc$
│      → gMSA
│
└─ Otro SPN
    └─ EMPRESA\Edu
       → cuenta de usuario normal a la que alguien ha asignado un SPN
```

Por tanto, **un SPN no implica automáticamente que la cuenta asociada sea una cuenta de servicio tradicional**.

El único concepto común es que el SPN identifica una instancia de servicio registrada para Kerberos y está asociado a un principal de Active Directory.

---

## 2. Qué hace GetUserSPNs.py de Impacket

`GetUserSPNs.py` no devuelve todas las cuentas de usuario del dominio.

Lo que busca principalmente son:

```text
CUENTAS DE USUARIO
        +
TIENEN SPN
```

Por ejemplo, podría devolver:

```text
EMPRESA\svc_sql
EMPRESA\svc_backup
EMPRESA\Edu
```

si las tres cuentas tienen al menos un SPN asociado.

Normalmente las cuentas que aparecen serán cuentas de usuario dedicadas a servicios, como `svc_sql` o `svc_backup`, porque es habitual que estas tengan SPN.

Pero también podría aparecer una cuenta de usuario normal como `EMPRESA\Edu` si alguien le hubiera asociado un SPN.

## Frase para memorizar

> **GetUserSPNs.py no significa "dame todas las cuentas de servicio" ni "dame todos los usuarios". Significa, de forma práctica: dame cuentas de usuario del dominio que tengan SPN.**

---

## 3. Relación con Kerberoasting

Para Kerberoasting, el flujo mental es:

```text
GetUserSPNs.py
      ↓
usuarios del dominio con SPN
      ↓
¿qué cuenta es?
      ↓
├─ svc_sql / svc_backup
│    → caso clásico e interesante
│
└─ Edu / Pepe / María con SPN
     → también puede ser interesante si la contraseña es débil
```

Lo importante para Kerberoasting no es el nombre de la cuenta, sino que:

1. sea una cuenta de usuario del dominio,
2. tenga un SPN,
3. se pueda solicitar un TGS para ese servicio,
4. y la contraseña sea lo bastante débil como para que el cracking offline sea viable.

## Resumen final

```text
ENUMERAR TODOS LOS SPN
→ pueden aparecer cuentas de usuario, cuentas de equipo, gMSA, etc.

GetUserSPNs.py
→ filtra hacia cuentas de usuario que tienen SPN
→ normalmente aparecen cuentas de servicio tradicionales
→ pero también puede aparecer un usuario normal si tiene SPN
```

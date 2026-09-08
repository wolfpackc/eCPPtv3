# Una cuenta de dominio de servicio es una cuenta de dominio de usuario usada para un servicio

Las **cuentas de dominio de servicio tradicionales** son, técnicamente, **cuentas de dominio de usuario**.

La diferencia principal no está en su naturaleza como objeto de Active Directory, sino en **la función que se les asigna**.

Ejemplo:

```text
EMPRESA\Edu
→ cuenta de dominio de usuario usada por una persona

EMPRESA\svc_sql
→ cuenta de dominio de usuario dedicada a ejecutar un servicio
```

Por tanto:

> **Cuenta de dominio de servicio tradicional = cuenta de dominio de usuario utilizada con una finalidad de servicio.**

Lo que cambia es su utilidad, no el hecho de que siga siendo una cuenta de usuario del dominio.

> Matiz: esto se refiere a las cuentas de servicio tradicionales basadas en usuarios. Las MSA/gMSA son tipos especiales de cuentas administradas y no deben confundirse con este caso.

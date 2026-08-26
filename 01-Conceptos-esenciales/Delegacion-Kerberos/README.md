# Delegación Kerberos

La delegación existe para resolver una necesidad legítima:

> Un servicio recibe una petición de un usuario y necesita acceder a otro servicio **en nombre de ese usuario**.

Ejemplo:

```text
Eduardo
   ↓
Servidor web
   ↓
Servidor SQL
```

El servidor web necesita consultar SQL usando la identidad de Eduardo.

Ahí entra la delegación.

Hay tres tipos que merece la pena conocer:

## Unconstrained Delegation

El servicio puede delegar de forma muy amplia. Es la modalidad más peligrosa si está mal configurada.

## Constrained Delegation

Solo puede delegar hacia servicios concretos definidos.

## Resource-Based Constrained Delegation (RBCD)

Es el recurso destino el que decide qué cuentas pueden actuar en nombre de otros usuarios ante él.

La idea ofensiva importante es que una mala configuración de delegación puede permitir:

```text
Controlar una cuenta / equipo
        ↓
Abusar de delegación
        ↓
Impersonar a otro usuario
        ↓
Acceder a un servicio
        ↓
Posible escalada de privilegios
```

Aquí Kerberos empieza a ponerse más interesante porque aparecen flujos como:

```text
S4U2Self
S4U2Proxy
```

No es necesario memorizarlos de golpe. Lo importante es entender que son mecanismos que permiten a servicios obtener tickets en nombre de usuarios bajo determinadas condiciones.
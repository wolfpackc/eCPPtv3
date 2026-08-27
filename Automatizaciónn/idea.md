

> Podemos dividir el proceso, de forma simplificada, en **explotación** y **post-explotación**. La fase de explotación consiste en conseguir una posición útil dentro del entorno: por ejemplo, obtener credenciales válidas, aprovechar una vulnerabilidad o encontrar una mala configuración que nos permita autenticarnos o acceder a un servicio. Si ya partimos de un usuario y contraseña válidos, podemos intentar conseguir identidades adicionales mediante técnicas como AS-REP Roasting o Kerberoasting, aunque estas técnicas encajan mejor como **Credential Access** que como una frontera estricta entre explotación y post-explotación.
>
> Si Kerberoasting no proporciona nuevas credenciales, seguimos teniendo la identidad inicial. El siguiente paso es comprobar **dónde funcionan esas credenciales y qué permisos tienen**: SMB, WinRM, MSSQL, SSH, RDP, LDAP, etc. En esta etapa todavía estamos validando nuestro acceso y determinando cuál es nuestra posición real dentro del entorno.
>
> Cuando conseguimos acceso efectivo a un host o servicio y empezamos a aprovechar esa posición para obtener información adicional, entramos claramente en **post-explotación**. Aquí el objetivo ya no es simplemente saber si las credenciales funcionan, sino conocer el sistema comprometido: usuarios, grupos, sesiones activas, procesos, servicios, red, shares, archivos interesantes, configuraciones, credenciales, tickets Kerberos y posibles rutas hacia otros equipos.
>
> La post-explotación no requiere necesariamente herramientas diferentes. Muchas veces las mismas herramientas utilizadas para validar el acceso —por ejemplo NetExec, Evil-WinRM, Impacket o clientes SMB/SQL— también permiten realizar enumeración posterior.
>
> Además, una parte importante de una metodología profesional es la **automatización**. En lugar de ejecutar manualmente decenas de comandos y revisar cada carpeta una por una, es habitual utilizar scripts o herramientas de recopilación que ejecutan automáticamente una serie de comprobaciones y generan resultados estructurados para analizarlos después.
>
> En un laboratorio, por ejemplo, podríamos transferir al host comprometido un script o programa benigno que recopile información del sistema, la guarde en un archivo y posteriormente transfiera ese resultado a nuestra máquina Kali para analizarlo con calma. Lo importante no es “copiar todo el disco”, sino **recoger de forma selectiva la información que realmente puede abrir nuevas rutas de ataque**.
>
> Por tanto, el flujo mental sería:
>
> **Credenciales → comprobar dónde autentican → comprobar permisos → conseguir acceso real → enumeración/post-explotación → recopilar información útil → buscar credenciales o nuevas rutas → movimiento lateral/pivoting → repetir.**



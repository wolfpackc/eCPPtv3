# Glosario conectado

## Active Directory e identidad

| Término | Definición |
|---|---|
| AD DS | Servicio de directorio de Microsoft para identidades, equipos y políticas |
| Principal | Identidad que puede autenticarse o recibir permisos |
| SID | Identificador de seguridad estable de un principal |
| Domain | Conjunto administrativo con directorio y políticas comunes |
| Forest | Conjunto de dominios que comparte esquema/configuración; frontera crítica |
| DC | Domain Controller; aloja AD DS y autentica |
| OU | Contenedor organizativo usado para administración y GPO |
| GPO | Conjunto de configuraciones aplicadas a usuarios/equipos |
| ACL | Lista que define permisos sobre un objeto |
| ACE | Entrada individual dentro de una ACL |
| Trust | Relación que permite reconocer identidades entre dominios/bosques |
| Tier Zero | Activos cuyo control compromete la plataforma de identidad |
| Domain Admin | Grupo altamente privilegiado del dominio |

## Kerberos

| Término | Definición |
|---|---|
| KDC | Servicio que emite tickets Kerberos en el DC |
| TGT | Ticket usado para solicitar tickets de servicio |
| TGS | Ticket para un servicio/SPN concreto |
| SPN | Nombre que identifica una instancia de servicio y su cuenta |
| Preauth | Prueba inicial cifrada que demuestra conocimiento de la clave del usuario |
| AS-REP Roasting | Auditoría offline de AS-REP de cuentas sin preauth |
| Kerberoasting | Auditoría offline de TGS asociados a cuentas con SPN |
| PTT | Reutilización de un ticket Kerberos válido |
| `krbtgt` | Cuenta cuya clave protege los TGT del dominio |

## NTLM y credenciales

| Término | Definición |
|---|---|
| NTLM | Familia challenge-response de autenticación Windows |
| Hash NT | Verificador/clave derivada de la contraseña de Windows |
| NetNTLMv2 | Respuesta challenge-response capturable, no el hash NT directo |
| PTH | Uso del hash NT para autenticarse en protocolos compatibles |
| Password spraying | Pocas contraseñas contra muchos usuarios |
| Brute force | Muchos candidatos contra una cuenta |
| Credential stuffing | Pares usuario/contraseña previamente obtenidos |

## Red y movimiento

| Término | Definición |
|---|---|
| Pivot | Host comprometido utilizado para alcanzar otra red |
| Port forward | Reenvío de un puerto entre extremos |
| SOCKS | Proxy que transporta conexiones de aplicaciones compatibles |
| TUN | Interfaz virtual de capa de red usada por ciertos túneles |
| Lateral movement | Cambio de host/identidad usando acceso ya obtenido |
| PrivEsc | Obtención de un nivel de privilegio superior |
| Foothold | Primer acceso útil dentro del entorno |

## Explotación

| Término | Definición |
|---|---|
| Vulnerabilidad | Debilidad que puede producir un impacto |
| Exploit | Código/procedimiento que materializa una vulnerabilidad |
| Payload | Acción ejecutada después de alcanzar la condición explotable |
| Reverse shell | El objetivo conecta hacia el listener del auditor |
| Bind shell | El objetivo escucha y el auditor conecta |
| Staged | Payload inicial que descarga/recibe otra etapa |
| Stageless | Payload completo en una sola pieza |
| PoC | Prueba de concepto; puede demostrar solo parte del impacto |
| Offset | Distancia hasta el campo que se quiere sobrescribir/controlar |
| Badchar | Byte transformado o no aceptado por el camino de entrada |
| ASLR | Aleatorización de direcciones |
| DEP/NX | Prevención de ejecución en regiones de datos |

## Web

| Término | Definición |
|---|---|
| SQLi | Entrada interpretada como parte de una consulta SQL |
| XSS | Datos no fiables ejecutados como script en navegador |
| Command injection | Entrada interpretada como comando del sistema |
| LFI | Inclusión/lectura local de archivo mediante ruta controlada |
| Path traversal | Escape del directorio previsto mediante manipulación de ruta |
| Vhost | Sitio seleccionado por el valor de Host en la misma IP |
| Session | Estado que vincula peticiones a una identidad autenticada |
| CSRF | Acción autenticada inducida desde otro origen/contexto |

## Herramientas

| Término | Función |
|---|---|
| Nmap | Descubrimiento y enumeración de puertos/servicios |
| NetExec | Operaciones y validación sobre protocolos empresariales |
| Impacket | Librerías/scripts Python para protocolos Windows |
| BloodHound | Análisis de relaciones y rutas de ataque |
| SharpHound | Recolección de datos AD para BloodHound |
| Burp Suite | Intercepción y prueba de HTTP/S |
| Hashcat/John | Auditoría offline de verificadores de contraseña |
| WinPEAS/LinPEAS | Enumeración automatizada de candidatos de PrivEsc |


# Matriz de cobertura oficial

Esta matriz evita dos errores: estudiar temas interesantes que no cubren carencias reales y creer que un objetivo está cubierto porque se menciona una herramienta.

## Information Gathering & Reconnaissance — 10 %

| Objetivo de INE | Teoría | Práctica | Repaso |
|---|---|---|---|
| Descubrimiento de hosts y escaneo de puertos | [Temario](03-Temario/01-Reconocimiento/README.md) | [Lab 01](04-Laboratorios/01-Reconocimiento-y-servicios.md) | [Chuleta](05-Chuletas/Nmap-y-servicios.md) |
| Enumeración de servicios | [Temario](03-Temario/01-Reconocimiento/README.md) | [Lab 01](04-Laboratorios/01-Reconocimiento-y-servicios.md) | [Nmap](02-Herramientas/Nmap/README.md) |

## Initial Access — 15 %

| Objetivo de INE | Teoría | Práctica | Repaso |
|---|---|---|---|
| Enumeración de usuarios | [Temario](03-Temario/02-Acceso-Inicial/README.md) | [Lab AD](04-Laboratorios/04-Active-Directory.md) | [AD](05-Chuletas/Active-Directory.md) |
| Password spraying | [Temario](03-Temario/02-Acceso-Inicial/README.md) | [Lab AD](04-Laboratorios/04-Active-Directory.md) | [NetExec](02-Herramientas/NetExec/README.md) |
| Fuerza bruta de acceso remoto | [Temario](03-Temario/02-Acceso-Inicial/README.md) | [Lab web](04-Laboratorios/02-Web-a-foothold.md) | [Credenciales](01-Conceptos-esenciales/Credenciales-y-hashes/README.md) |

## Web Application Penetration Testing — 15 %

| Objetivo de INE | Teoría | Práctica | Repaso |
|---|---|---|---|
| Enumeración de aplicaciones | [Temario web](03-Temario/03-Pentesting-Web/README.md) | [Lab 02](04-Laboratorios/02-Web-a-foothold.md) | [Chuleta web](05-Chuletas/Web.md) |
| SQLi, XSS, command injection y fallos comunes | [Conceptos web](01-Conceptos-esenciales/Vulnerabilidades-web/README.md) | [Lab 02](04-Laboratorios/02-Web-a-foothold.md) | [Burp](02-Herramientas/Burp-Suite/README.md) |
| Brute force de formularios | [Temario web](03-Temario/03-Pentesting-Web/README.md) | [Lab 02](04-Laboratorios/02-Web-a-foothold.md) | [Burp](02-Herramientas/Burp-Suite/README.md) |
| Componentes vulnerables/obsoletos | [Temario web](03-Temario/03-Pentesting-Web/README.md) | [Capstone](04-Laboratorios/05-Capstone.md) | [Metasploit](02-Herramientas/Metasploit/README.md) |
| Exfiltración de datos/credenciales | [Temario web](03-Temario/03-Pentesting-Web/README.md) | [Lab 02](04-Laboratorios/02-Web-a-foothold.md) | [Credenciales](01-Conceptos-esenciales/Credenciales-y-hashes/README.md) |

## Exploitation & Post-Exploitation — 25 %

| Objetivo de INE | Teoría | Práctica | Repaso |
|---|---|---|---|
| Explotar servicios/misconfiguraciones | [Temario](03-Temario/04-Explotacion-y-Postexplotacion/README.md) | [Lab 03](04-Laboratorios/03-Postexplotacion-y-Privesc.md) | [Metasploit](02-Herramientas/Metasploit/README.md) |
| Escalada de privilegios | [Concepto](01-Conceptos-esenciales/Escalada-de-privilegios/README.md) | [Lab 03](04-Laboratorios/03-Postexplotacion-y-Privesc.md) | [Chuleta](05-Chuletas/PrivEsc.md) |
| Volcado y crackeo de hashes | [Credenciales](01-Conceptos-esenciales/Credenciales-y-hashes/README.md) | [Lab 03](04-Laboratorios/03-Postexplotacion-y-Privesc.md) | [Crackeo](02-Herramientas/Crackeo/README.md) |
| Credenciales locales inseguras | [Temario](03-Temario/04-Explotacion-y-Postexplotacion/README.md) | [Lab 03](04-Laboratorios/03-Postexplotacion-y-Privesc.md) | [PrivEsc](02-Herramientas/PrivEsc/README.md) |

## Exploit Development — 5 %

| Objetivo de INE | Teoría | Práctica | Repaso |
|---|---|---|---|
| Desarrollar/modificar exploit | [Temario](03-Temario/05-Exploit-Development/README.md) | [Capstone](04-Laboratorios/05-Capstone.md) | [Metasploit](02-Herramientas/Metasploit/README.md) |
| Stack/buffer overflow | [Concepto](01-Conceptos-esenciales/Buffer-Overflow/README.md) | [Temario práctico](03-Temario/05-Exploit-Development/README.md) | [Glosario](07-Glosario/README.md) |

## Active Directory Penetration Testing — 30 %

| Objetivo de INE | Teoría | Práctica | Repaso |
|---|---|---|---|
| Enumeración AD | [Temario AD](03-Temario/06-Active-Directory/README.md) | [Lab 04](04-Laboratorios/04-Active-Directory.md) | [Chuleta AD](05-Chuletas/Active-Directory.md) |
| Contraseñas débiles/vacías | [Credenciales](01-Conceptos-esenciales/Credenciales-y-hashes/README.md) | [Lab 04](04-Laboratorios/04-Active-Directory.md) | [NetExec](02-Herramientas/NetExec/README.md) |
| AS-REP Roasting | [Concepto](01-Conceptos-esenciales/AS-REP-Roasting/README.md) | [Lab 04](04-Laboratorios/04-Active-Directory.md) | [Impacket](02-Herramientas/Impacket/README.md) |
| Pass-the-Hash | [Concepto](01-Conceptos-esenciales/Pass-the-Hash/README.md) | [Lab 04](04-Laboratorios/04-Active-Directory.md) | [Chuleta AD](05-Chuletas/Active-Directory.md) |
| Pass-the-Ticket | [Concepto](01-Conceptos-esenciales/Pass-the-Ticket/README.md) | [Lab 04](04-Laboratorios/04-Active-Directory.md) | [Chuleta AD](05-Chuletas/Active-Directory.md) |
| Domain Admin/Access | [Tier Zero](01-Conceptos-esenciales/Domain-Admin-y-Tier-Zero/README.md) | [Lab 04](04-Laboratorios/04-Active-Directory.md) | [BloodHound](02-Herramientas/BloodHound/README.md) |

## Regla de cierre

Una fila no se marca como dominada hasta completar teoría, práctica y repaso sin copiar una cadena de comandos. Si puedes ejecutar pero no explicar, la fila sigue incompleta.


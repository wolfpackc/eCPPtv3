# Impacket

## Qué es

Impacket es una colección de clases Python y scripts de ejemplo para protocolos de red, especialmente del ecosistema Windows. Cada utilidad resuelve una tarea concreta; no es una única «navaja suiza» con una interfaz uniforme.

Los nombres pueden instalarse con o sin sufijo `.py`. Comprueba tu distribución.

## Mapa de utilidades

| Utilidad | Uso principal |
|---|---|
| `GetNPUsers` | Identificar/solicitar AS-REP para cuentas sin preauth |
| `GetUserSPNs` | Enumerar SPN y solicitar TGS para Kerberoasting |
| `secretsdump` | Extraer secretos cuando el acceso y privilegio lo permiten |
| `psexec` | Ejecución mediante servicio y SMB |
| `wmiexec` | Ejecución seminteractiva mediante WMI |
| `smbexec` | Ejecución apoyada en SMB/servicios |
| `atexec` | Ejecución mediante tarea programada |
| `smbclient` | Navegar y transferir por SMB |
| `mssqlclient` | Interactuar con Microsoft SQL Server |
| `ntlmrelayx` | Estudiar relay NTLM en laboratorios preparados |

## Formatos de identidad

```text
dominio/usuario:contraseña@objetivo
dominio/usuario@objetivo
-hashes LMHASH:NTHASH
-k -no-pass
```

Ejemplos:

```bash
smbclient.py 'corp.local/edu:Laboratorio123!@SRV01.corp.local'
wmiexec.py -hashes ':<NTHASH>' 'corp.local/edu@SRV01.corp.local'
```

## Ejecución remota: diferencias

| Técnica | Dependencia/huella aproximada |
|---|---|
| PsExec | SMB, escritura administrativa y creación de servicio |
| WMIExec | WMI/DCOM y salida seminteractiva |
| SMBExec | Servicio/comandos a través de SMB |
| ATExec | Tareas programadas |

Elige por conectividad, privilegios, efecto y detección. Si PsExec falla, no significa automáticamente que las credenciales sean inválidas.

## Kerberos

```bash
export KRB5CCNAME=/ruta/ticket.ccache
klist
smbclient.py -k -no-pass corp.local/edu@SRV01.corp.local
```

Usa nombres que coincidan con SPN y DNS. La IP puede provocar un fallback o un fallo.

## Secretsdump

`secretsdump` puede operar remotamente con privilegios suficientes o sobre ficheros offline. Antes de usarlo, entiende qué fuentes intenta leer (SAM, SECURITY, LSA, NTDS), qué impacto tiene y qué secreto corresponde a una cuenta local o de dominio.

## Buen hábito

Ejecuta siempre `herramienta -h`, anota la versión y empieza por la operación menos invasiva. Impacket cambia y muchas chuletas de internet mezclan sintaxis antigua.


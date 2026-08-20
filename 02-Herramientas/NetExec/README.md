# NetExec

## Qué es

NetExec (`nxc`) automatiza enumeración, validación de credenciales y determinadas acciones sobre protocolos empresariales. Es especialmente útil para SMB, WinRM, LDAP, RDP, SSH, MSSQL y FTP, según la versión instalada.

No reemplaza Nmap. Nmap descubre la superficie; NetExec realiza preguntas específicas al protocolo y permite operar sobre varios objetivos.

## Sintaxis mental

```text
nxc <protocolo> <objetivo> [identidad] [acción o módulo]
```

## Casos base

```bash
# Enumeración SMB sin credenciales o con null session
nxc smb 10.10.10.0/24
nxc smb 10.10.10.20 -u '' -p '' --shares

# Validar una cuenta de dominio
nxc smb 10.10.10.0/24 -u edu -p 'Laboratorio123!' -d CORP

# Validar un hash NT
nxc smb 10.10.10.20 -u edu -H '<NTHASH>' -d CORP

# Comprobar WinRM
nxc winrm 10.10.10.20 -u edu -p 'Laboratorio123!' -d CORP

# Enumerar usuarios mediante LDAP con credenciales
nxc ldap 10.10.10.10 -u edu -p 'Laboratorio123!' -d CORP --users
```

Consulta `nxc <protocolo> --help` porque módulos y opciones evolucionan.

## Interpretar la línea de salida

Normalmente muestra protocolo, IP, puerto, hostname, dominio, información del sistema y resultado de autenticación. Una marca como `Pwn3d!` suele indicar privilegios administrativos sobre el host/protocolo probado. No significa que la cuenta sea Domain Admin.

## Password spraying controlado

```bash
nxc smb dc.corp.local -u usuarios.txt -p 'Verano2026!' -d CORP --continue-on-success
```

Antes debes conocer la política de bloqueo y tener autorización. Usa pocas contraseñas plausibles, separa rondas y registra éxitos. No conviertas spraying en fuerza bruta accidental.

## Cuándo bajar a herramientas específicas

Si el resultado es ambiguo:

- `smbclient` para navegar shares;
- `rpcclient` para RPC;
- `ldapsearch` para consultas LDAP explícitas;
- Impacket para una técnica de autenticación o ejecución concreta;
- cliente RDP/WinRM para validar acceso interactivo.

## Errores frecuentes

- omitir `-d` y autenticar contra la cuenta local equivocada;
- confundir usuario válido con administrador;
- lanzar módulos sin leer efectos;
- probar una matriz masiva de usuarios y contraseñas;
- usarlo como sustituto de entender SMB, NTLM, LDAP o WinRM.


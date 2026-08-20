# Chuleta: PrivEsc

## Linux

```bash
id
uname -a
cat /etc/os-release
sudo -l
find / -perm -4000 -type f 2>/dev/null
getcap -r / 2>/dev/null
systemctl list-timers --all
ss -lntup
ps auxf
find / -writable -type f 2>/dev/null
```

Revisar: sudo, SUID, capabilities, cron/timers, servicios, PATH, librerías, grupos, montajes, NFS, credenciales y solo después kernel.

## Windows

```powershell
whoami /all
whoami /priv
systeminfo
ipconfig /all
route print
netstat -ano
tasklist /svc
sc.exe query state= all
schtasks /query /fo LIST /v
cmdkey /list
```

Revisar: servicios, tareas, ACL de archivos/directorios/registro, credenciales, instaladores, privilegios de token, sesiones, grupos y parches.

## Validación de candidato

```text
¿qué controlo? -> ¿quién lo ejecuta? -> ¿con qué privilegio?
-> ¿cuándo se ejecuta? -> ¿puedo activarlo? -> ¿cómo revierto?
```

## PEAS

- ejecutar en entorno autorizado;
- guardar salida;
- priorizar secretos y escritura directa;
- reproducir con comandos nativos;
- descartar falsos positivos;
- no empezar por exploit de kernel.

## Después de escalar

- confirma identidad;
- recoge evidencia mínima;
- busca solo secretos previstos;
- no añadas persistencia;
- registra archivos/procesos creados;
- prepara reversión.


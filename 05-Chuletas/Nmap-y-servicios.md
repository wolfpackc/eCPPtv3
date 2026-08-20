# Chuleta: Nmap y servicios

## Nmap

```bash
nmap -sn 10.10.10.0/24 -oA hosts
sudo nmap -p- -T4 10.10.10.20 -oA alltcp
nmap -sC -sV -p22,80,445 10.10.10.20 -oA services
sudo nmap -sU --top-ports 100 10.10.10.20 -oA udp
proxychains nmap -sT -Pn -n -p80,445 172.16.20.10
```

## DNS

```bash
dig @10.10.10.10 corp.local ANY
dig @10.10.10.10 _ldap._tcp.dc._msdcs.corp.local SRV
dig @10.10.10.10 _kerberos._tcp.corp.local SRV
```

## HTTP

```bash
curl -isk http://10.10.10.20/
curl -isk -H 'Host: app.corp.local' http://10.10.10.20/
whatweb http://app.corp.local
ffuf -u http://app.corp.local/FUZZ -w wordlist.txt -fc 404
```

## SMB/RPC

```bash
nxc smb 10.10.10.20
smbclient -N -L //10.10.10.20
smbclient //10.10.10.20/share -U 'CORP/edu'
rpcclient -U '' -N 10.10.10.20
```

## LDAP

```bash
ldapsearch -x -H ldap://10.10.10.10 -s base namingContexts
ldapsearch -x -H ldap://10.10.10.10 \
  -D 'edu@corp.local' -W -b 'DC=corp,DC=local' '(objectClass=user)' sAMAccountName
```

## FTP/SSH/SMTP

```bash
ftp 10.10.10.20
ssh -v edu@10.10.10.20
nc -nv 10.10.10.20 25
```

## Checklist por host

- IP, FQDN, hostname y dominio.
- SO/rol probable con evidencia.
- TCP completo y UDP selectivo.
- versión y configuración por servicio.
- acceso anónimo/guest.
- nombres, shares, rutas y certificados.
- credenciales probadas y permiso real.
- hipótesis y próximo paso.


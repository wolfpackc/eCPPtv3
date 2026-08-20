# LDAP y DNS en Active Directory

## Dos piezas distintas

**LDAP** es el protocolo con el que se consulta y modifica el directorio. **DNS** permite encontrar los servidores y servicios del dominio. AD depende profundamente de ambos.

Analogía:

- DNS es el índice que dice dónde está cada departamento;
- LDAP es el mostrador donde consultas las fichas del personal, equipos, grupos y políticas.

## Estructura LDAP

Un nombre distinguido describe la posición de un objeto:

```text
CN=Eduardo,OU=Pentesters,DC=corp,DC=local
```

| Componente | Significado |
|---|---|
| `CN` | Common Name del objeto |
| `OU` | Organizational Unit |
| `DC` | Parte del nombre de dominio |

La base del dominio anterior es `DC=corp,DC=local`.

## Qué revela LDAP

Con credenciales válidas, puede revelar usuarios, grupos, equipos, SPN, configuración de preautenticación, ACL, GPO y relaciones de confianza. La información suele ser legible por usuarios autenticados de bajo privilegio; eso hace que una sola credencial de dominio tenga gran valor para enumerar.

Ejemplo de consulta de laboratorio:

```bash
ldapsearch -x -H ldap://10.10.10.10 \
  -D 'edu@corp.local' -w 'Laboratorio123!' \
  -b 'DC=corp,DC=local' '(objectClass=user)' sAMAccountName
```

No pegues contraseñas reales en el historial. En prácticas duraderas, usa variables o mecanismos seguros de la herramienta.

## DNS y registros SRV

AD publica servicios mediante registros SRV:

```bash
dig @10.10.10.10 _ldap._tcp.dc._msdcs.corp.local SRV
dig @10.10.10.10 _kerberos._tcp.corp.local SRV
```

La resolución correcta del FQDN ayuda a Kerberos a construir SPN coherentes. Añadir todo a `/etc/hosts` puede resolver un caso puntual, pero no sustituye un DNS de dominio bien configurado.

## Global Catalog

Los puertos 3268/3269 suelen corresponder al Global Catalog, que permite buscar información parcial de objetos a través del bosque. LDAP normal usa 389 y LDAPS 636.

## Diagnóstico mínimo

Si una herramienta de AD falla:

1. Comprueba ruta al DC.
2. Comprueba DNS y FQDN.
3. Comprueba hora.
4. Confirma el dominio y formato de usuario.
5. Prueba si el puerto LDAP/Kerberos está accesible.
6. Distingue autenticación fallida de autorización insuficiente.


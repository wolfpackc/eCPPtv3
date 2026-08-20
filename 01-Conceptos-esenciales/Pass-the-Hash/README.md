# Pass-the-Hash

## Idea esencial

Pass-the-Hash (PTH) consiste en autenticarse en protocolos compatibles utilizando el hash NT como material criptográfico, sin conocer la contraseña en claro.

La mejor analogía no es «copiar una contraseña», sino usar un molde capaz de producir la respuesta correcta al desafío. El servidor valida la respuesta; no exige que el cliente escriba el texto original de la contraseña.

## Qué necesitas

- hash NT válido;
- identidad correcta y ámbito local o de dominio;
- servicio compatible con autenticación NTLM;
- conectividad;
- autorización suficiente para la acción que pretendes.

## Local frente a dominio

`Administrador` local en `PC01` y `CORP\Administrador` son identidades diferentes. Si reutilizas un hash local, prueba de manera controlada dónde existe la misma cuenta y contraseña. Para una cuenta de dominio, el DC participa en la validación.

## Ejemplos en laboratorio

Validación SMB con NetExec:

```bash
nxc smb 10.10.10.20 -u edu -H '<NTHASH>' -d CORP
```

Ejecución remota con Impacket, solo cuando la cuenta sea administradora del objetivo y el alcance lo permita:

```bash
psexec.py -hashes ':<NTHASH>' 'CORP/edu@10.10.10.20'
wmiexec.py -hashes ':<NTHASH>' 'CORP/edu@10.10.10.20'
```

Las técnicas tienen huellas y requisitos diferentes. `psexec` suele crear un servicio; WMI o SMBExec usan otros mecanismos. No las trates como sinónimos.

## Interpretación de resultados

| Resultado | Interpretación posible |
|---|---|
| Autenticación válida, sin admin | Hash correcto; privilegios limitados |
| Acceso administrativo | Control administrativo sobre ese host, no necesariamente el dominio |
| Logon failure | Hash, usuario, dominio o tipo de cuenta incorrecto |
| Access denied | Identidad válida pero sin permiso para esa operación |
| Bloqueo/restricción | Política, UAC remoto, firewall o protocolo deshabilitado |

## No confundir con

- **Pass-the-Ticket:** reutiliza tickets Kerberos.
- **Overpass-the-Hash:** usa material de clave para obtener tickets Kerberos.
- **Crackeo:** intenta recuperar la contraseña.
- **Relay:** reenvía una autenticación en curso hacia otro servicio.

## Mitigación

Reducir reutilización de credenciales, aplicar LAPS/Windows LAPS para administradores locales, limitar administración remota, proteger credenciales, segmentar, usar tiering y restringir NTLM cuando sea viable.


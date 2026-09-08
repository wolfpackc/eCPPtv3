# Sí: **cuando un servicio de dominio va a autenticarse mediante Kerberos, debe estar asociado a una cuenta de Active Directory —por ejemplo, una cuenta de equipo, una cuenta de usuario de servicio o una gMSA— y tener registrado un SPN que identifique de forma única esa instancia de servicio; ese SPN permite al KDC saber qué cuenta representa al servicio y emitir al cliente el ticket de servicio correspondiente, por lo que, si el SPN falta o es incorrecto, la autenticación Kerberos para ese servicio puede fallar o no utilizarse.**


Los **SPN se guardan en Active Directory**, concretamente en el atributo **`servicePrincipalName`** del objeto que representa la cuenta bajo la que funciona el servicio. Esa cuenta puede ser, por ejemplo, una cuenta de usuario de servicio, una cuenta de equipo o una gMSA.

Por ejemplo, podrías tener:

```text
Cuenta AD:
sqlsvc

servicePrincipalName:
MSSQLSvc/sql01.empresa.local:1433
```

Eso significa:

```text
MSSQLSvc/sql01.empresa.local:1433
            ↓
        pertenece a
            ↓
         sqlsvc
```

Y sobre tu segunda pregunta: **sí, la petición que hace tu cliente al KDC tiene que incluir el SPN concreto que quiere**, pero normalmente **tú como persona no tienes que saberlo ni escribirlo**.

Por ejemplo, tú haces:

```text
Conectarme a:
sql01.empresa.local
puerto 1433
```


<img width="1672" height="941" alt="ChatGPT Image 9 sept 2026, 00_56_48" src="https://github.com/user-attachments/assets/358bd67f-c2fd-478b-959e-915dbce9491e" />


El cliente de SQL / driver sabe que está hablando con Microsoft SQL Server y construye automáticamente algo como:

```text
MSSQLSvc/sql01.empresa.local:1433
```

Después manda al KDC:

```text
Tengo este TGT.

Quiero un ticket para:

MSSQLSvc/sql01.empresa.local:1433
```

Y ahora **Kerberos busca ese SPN dentro de Active Directory**:

```text
Cliente
   │
   │ TGT + "quiero MSSQLSvc/sql01..."
   ▼
KDC / TGS
   │
   │ busca en Active Directory
   ▼
servicePrincipalName
MSSQLSvc/sql01.empresa.local:1433
   │
   ▼
Cuenta: sqlsvc
```

Por tanto, tu forma de pensarlo puede ser:

**El cliente construye/conoce el nombre del destino → el KDC busca quién posee ese nombre en Active Directory.**

No es normalmente:

> «Cliente pregunta a AD qué SPN tiene SQL y luego vuelve a Kerberos».

Sino más bien:

> **«Quiero este SPN concreto» → Kerberos busca en AD qué cuenta tiene registrado ese SPN.**

Ese matiz es bastante importante.

# Kerberos requiere un SPN para identificar el servicio

## Si un servicio de dominio va a ser utilizado mediante **Kerberos**, debe existir un **SPN (Service Principal Name)** correcto que identifique esa instancia de servicio y la relacione con la cuenta de Active Directory que la representa.

El SPN puede:

- registrarse automáticamente por el propio servicio o software,
- ser creado o configurado manualmente por un administrador,
- o ya existir previamente.
- 
<img width="1448" height="1086" alt="ChatGPT Image 9 sept 2026, 00_43_16" src="https://github.com/user-attachments/assets/6cbf9630-5418-4506-963b-e3f359e3ebed" />

Lo importante no es quién lo cree, sino que **exista, sea correcto y esté asociado a la cuenta adecuada**.

Sin un SPN válido para ese servicio, Kerberos no puede identificar correctamente a qué principal debe emitir el ticket de servicio, por lo que la autenticación Kerberos para ese acceso puede fallar o no utilizarse según el escenario.

```text
Servicio
   ↓
SPN correcto
   ↓
Cuenta que representa al servicio
   ↓
Kerberos puede emitir el TGS adecuado
```

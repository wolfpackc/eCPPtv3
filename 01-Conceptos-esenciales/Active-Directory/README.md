# Active Directory

## La idea en una frase

Active Directory Domain Services (AD DS) es el sistema con el que una organización guarda identidades y aplica relaciones de confianza y permisos de forma centralizada.

Una analogía útil es una universidad:

- el **dominio** es la institución;
- el **Domain Controller** es secretaría y control de acceso;
- los **usuarios y equipos** son miembros identificados;
- los **grupos** agrupan funciones;
- las **GPO** son normas aplicadas a colectivos;
- Kerberos y NTLM son formas de demostrar la identidad;
- LDAP es la forma de consultar el directorio;
- DNS es el mapa para encontrar sus servicios.

La analogía tiene límites: AD es distribuido, sus controladores replican datos y los permisos pueden heredarse y encadenarse.

## Objetos fundamentales

| Objeto | Significado práctico |
|---|---|
| User | Identidad humana o de servicio |
| Computer | Identidad propia de un equipo unido al dominio |
| Group | Conjunto de principales al que se asignan permisos |
| Organizational Unit | Contenedor administrativo y destino habitual de GPO |
| Group Policy Object | Configuración aplicable a usuarios o equipos |
| Domain Controller | Servidor que aloja AD DS, autentica y replica |
| Service account | Cuenta usada por una aplicación o servicio |

Un **principal de seguridad** es un objeto que puede autenticarse o recibir permisos. Normalmente tiene un SID. Los nombres pueden cambiar; el SID es la identidad estable dentro del modelo de seguridad.

## Dominio, árbol y bosque

```mermaid
flowchart TD
    F[Forest corp.local] --> D1[Domain corp.local]
    F --> D2[Domain eu.corp.local]
    D1 --> OU1[OU Servidores]
    D1 --> OU2[OU Usuarios]
    D2 --> OU3[OU Equipos EU]
```

- **Dominio:** frontera administrativa y de identidad con una base de datos compartida.
- **Árbol:** dominios con un espacio DNS jerárquico relacionado.
- **Bosque:** conjunto de dominios que comparte esquema, configuración y relaciones de confianza internas. Es una frontera de seguridad crítica.

## Qué contiene un DC

El controlador mantiene la base de datos del directorio (`NTDS.dit`), actúa como KDC para Kerberos, ofrece LDAP y normalmente DNS. También publica recursos mediante SMB y participa en replicación. Por eso un escaneo con 53, 88, 389, 445, 464, 636 y 3268 sugiere fuertemente un DC, aunque debes confirmarlo.

## El motor estándar y el entorno particular

Active Directory Domain Services (**AD DS**) debe instalarse como un **rol de Windows Server**. Después, el servidor se promociona a **controlador de dominio**. Un Windows cliente normal, como Windows 10 u 11, puede unirse al dominio, pero no se convierte por ello en controlador de dominio.

```mermaid
flowchart TB
    SERVER["Servidor con Windows Server"]
    SERVER --> INSTALL["Instalación del rol AD DS"]
    INSTALL --> PROMOTE["Promoción a Domain Controller"]
    PROMOTE --> AD["Active Directory Domain Services"]

    AD --> SOFTWARE["Software y servicios de AD DS"]
    AD --> CUSTOM["Información particular del dominio"]

    SOFTWARE --> CODE["Código estándar proporcionado por Microsoft"]
    CODE --> FUNCTIONS["Autenticación, Kerberos, LDAP y replicación"]
    FUNCTIONS --> ACTIONS["Consultar, crear, modificar o eliminar objetos"]
    ACTIONS --> CUSTOM

    CUSTOM --> NTDS["NTDS.dit"]
    CUSTOM --> SYSVOL["SYSVOL"]

    NTDS --> OBJECTS["Usuarios, equipos, grupos, hashes, atributos y permisos"]
    SYSVOL --> POLICIES["Plantillas de GPO y scripts de inicio o sesión"]

    NOTE1["NOTA 1: En pentesting suele interesar especialmente el entorno particular: identidades, credenciales, permisos, relaciones, GPO y configuraciones"]
    NOTE2["NOTA 2: AD DS es código estándar. Lo habitual no es modificarlo, sino abusar de permisos, protocolos o configuraciones. Aun así, puede tener vulnerabilidades y debe actualizarse"]

    CUSTOM -.-> NOTE1
    SOFTWARE -.-> NOTE2
```

## Idea clave

> **El software de AD DS es el motor estándar que ejecuta las operaciones; `NTDS.dit` y `SYSVOL` contienen el entorno particular de la empresa sobre el que trabaja ese motor.**

Esta separación permite distinguir dos partes:

1. **Motor estándar:** componentes de AD DS instalados en Windows Server. Implementan autenticación, Kerberos, LDAP, replicación y las operaciones para consultar, crear, modificar o eliminar objetos.
2. **Entorno particular del dominio:** usuarios, equipos, grupos, credenciales, permisos, políticas y configuraciones creados por cada organización.

`NTDS.dit` es la base de datos del directorio. Guarda objetos y atributos como usuarios, equipos, grupos, hashes de contraseñas, relaciones y permisos. `SYSVOL` no contiene el software de AD DS: es una carpeta compartida que almacena principalmente plantillas de políticas de grupo y scripts de inicio o sesión.

Cuando se añade otro controlador de dominio, AD DS se instala en el nuevo Windows Server y este se promociona. El software no se copia desde el primer DC: lo que se replica entre controladores son los datos del directorio y el contenido de `SYSVOL`.

En un pentest de Active Directory, lo habitual es estudiar y abusar del entorno particular: credenciales débiles, permisos excesivos, delegaciones, relaciones, GPO y configuraciones inseguras. Atacar directamente el código de AD DS es menos frecuente, aunque el software puede contener vulnerabilidades y debe mantenerse actualizado.

## Permisos y ACL

Los objetos de AD tienen listas de control de acceso. No solo importa quién pertenece a Domain Admins. También importa quién puede:

- cambiar la contraseña de otra cuenta;
- añadir miembros a un grupo;
- modificar la ACL de un objeto;
- escribir un SPN;
- enlazar o modificar una GPO;
- administrar un equipo donde inicia sesión una cuenta privilegiada.

Así surgen rutas indirectas. Un usuario corriente puede no ser administrador, pero quizá controle un grupo que controla un equipo que almacena una sesión privilegiada.

## Flujo mental de un ataque de laboratorio

```mermaid
flowchart TD
    A[Credencial de dominio] --> B[Enumerar objetos y relaciones]
    B --> C[Buscar debilidades de autenticación]
    B --> D[Buscar ACL abusables]
    B --> E[Buscar administración local y sesiones]
    C --> F[Nueva identidad]
    D --> F
    E --> G[Nuevo equipo]
    F --> B
    G --> B
```

## Lo que debes dominar

Debes poder diferenciar cuenta local y de dominio, usuario y equipo, dominio y bosque, grupo y OU, autenticación y autorización, administrador local y Domain Admin. También debes comprender SID, SPN, ACL, GPO, trust, DC, DNS, LDAP, NTLM y Kerberos.

## Error frecuente

Tratar AD como una lista de ataques. El enfoque correcto es construir un grafo de control: qué identidad controlas, qué puede leer o modificar, dónde puede autenticarse y qué nueva identidad aparece después.


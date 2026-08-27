
```mermaid
flowchart TD

    A["YA TENEMOS CREDENCIALES VÁLIDAS<br/><br/>Usuario: EMPRESA\\jaime<br/>Contraseña: ********"]:::good

    A --> B["¿Cómo las conseguimos?<br/><br/>• AS-REP Roasting<br/>• Credenciales filtradas<br/>• Post-it<br/>• Reutilización de contraseña<br/><br/>El origen ya no es lo importante"]:::neutral

    B --> C["Primer paso habitual en AD:<br/><br/>ENUMERAR CUENTAS CON SPN<br/><br/>Ejemplo conceptual:<br/>GetUserSPNs"]:::action

    C --> D{"¿Aparecen cuentas de usuario<br/>con SPN interesantes?"}:::decision

    D -- "Sí" --> E["Existe una posible ruta<br/>de KERBEROASTING<br/><br/>Ejemplo:<br/>sqlsvc → MSSQLSvc/SQL01"]:::kerb

    D -- "No" --> F["NO aparecen cuentas de usuario<br/>con SPN kerberoasteables"]:::failed

    F --> G["PERO sabemos que existen servicios:<br/><br/>• SMB / CIFS<br/>• SQL<br/>• HTTP<br/>• otros"]:::failed

    G --> H["Esto puede indicar que sus SPN<br/>están asociados a otras identidades:<br/><br/>• Cuenta de equipo → FILES01$<br/>• gMSA → gmsa-sql$<br/>• otras identidades administradas"]:::failed

    H --> I["Intentar crackear TGS asociados<br/>a estas identidades suele ser<br/>MUY POCO RENTABLE<br/><br/>Secretos largos, aleatorios<br/>y normalmente gestionados automáticamente"]:::failed

    I --> J["KERBEROASTING<br/>NO NOS DA UNA NUEVA<br/>CREDENCIAL ÚTIL"]:::deadend

    J --> K["PERO NO HEMOS PERDIDO<br/>NUESTRAS CREDENCIALES INICIALES"]:::good

    K --> L["Seguimos teniendo:<br/><br/>EMPRESA\\jaime<br/>+<br/>contraseña válida<br/>+<br/>identidad de dominio"]:::good

    L --> M["SIGUIENTE PREGUNTA<br/><br/>¿QUÉ PUEDO HACER REALMENTE<br/>CON ESTE USUARIO Y CONTRASEÑA?"]:::next

    classDef good fill:#123524,stroke:#35d07f,color:#ffffff,stroke-width:3px;
    classDef neutral fill:#252525,stroke:#888888,color:#ffffff,stroke-width:2px;
    classDef action fill:#13293d,stroke:#3ca0ff,color:#ffffff,stroke-width:3px;
    classDef decision fill:#2b2140,stroke:#b084ff,color:#ffffff,stroke-width:3px;
    classDef kerb fill:#3a2d00,stroke:#ffc107,color:#ffffff,stroke-width:3px;
    classDef failed fill:#3b0d0d,stroke:#ff4d4d,color:#ffffff,stroke-width:3px;
    classDef deadend fill:#111111,stroke:#ff0000,color:#ffffff,stroke-width:4px;
    classDef next fill:#0b3d4d,stroke:#00d4ff,color:#ffffff,stroke-width:4px;
```

La idea visual queda así: **intentamos Kerberoasting → esa puerta no da frutos → la rama se pone roja/negra → volvemos a nuestra posición inicial fuerte: seguimos teniendo una identidad válida del dominio**.

El siguiente bloque natural del diagrama sería precisamente abrir desde `¿QUÉ PUEDO HACER CON JAIME?` hacia **SMB, SQL, WinRM, shares, LDAP, hosts y permisos reales**.



```mermaid
flowchart TD

    A["KERBEROASTING FALLIDO<br/><br/>No conseguimos nuevas credenciales"]:::failed

    A --> B{"¿Qué hacemos ahora?"}:::decision

    B -->|"Opción 1"| C["RENDIRNOS 😭"]:::dead

    B -->|"Opción 2"| D["USAR LAS CREDENCIALES<br/>QUE YA TENEMOS<br/><br/>EMPRESA\\jaime<br/>+ contraseña válida"]:::good

    D --> E["DESCUBRIR HOSTS Y SERVICIOS<br/><br/>Nmap / enumeración de red"]:::recon

    E --> F["PROBAR DÓNDE FUNCIONAN<br/>LAS CREDENCIALES"]:::action

    F --> G["NXC<br/><br/>nxc &lt;protocolo&gt; &lt;IP / rango / lista&gt;<br/>-u jaime -p contraseña"]:::nxc

    G --> SMB["SMB<br/>445"]:::service
    G --> WINRM["WinRM<br/>5985 / 5986"]:::service
    G --> MSSQL["MSSQL<br/>1433"]:::service
    G --> LDAP["LDAP<br/>389 / 636"]:::service
    G --> RDP["RDP<br/>3389"]:::service
    G --> SSH["SSH<br/>22"]:::service
    G --> WMI["WMI"]:::service

    SMB --> R
    WINRM --> R
    MSSQL --> R
    LDAP --> R
    RDP --> R
    SSH --> R
    WMI --> R

    R{"RESULTADO DE LA PRUEBA"}:::decision

    R --> NO["NI DE PUTA COÑA ❌<br/><br/>No autentica / acceso denegado"]:::failed

    R --> USER["SÍ ✅<br/><br/>Credenciales válidas<br/>pero permisos normales o limitados"]:::normal

    R --> ADMIN["PWN3D! 🔥<br/><br/>Autentica y además<br/>tenemos privilegios elevados"]:::pwned

    USER --> NEXT1["ENUMERAR QUÉ PUEDE HACER<br/>ESA IDENTIDAD"]:::next
    ADMIN --> NEXT2["POST-EXPLOTACIÓN<br/><br/>Enumerar host, sesiones,<br/>credenciales, tickets y rutas"]:::next

    classDef failed fill:#3b0d0d,stroke:#ff4d4d,color:#ffffff,stroke-width:3px;
    classDef dead fill:#111111,stroke:#ff0000,color:#ffffff,stroke-width:4px;
    classDef decision fill:#2b2140,stroke:#b084ff,color:#ffffff,stroke-width:3px;
    classDef good fill:#123524,stroke:#35d07f,color:#ffffff,stroke-width:3px;
    classDef recon fill:#2c2c2c,stroke:#aaaaaa,color:#ffffff,stroke-width:2px;
    classDef action fill:#13293d,stroke:#3ca0ff,color:#ffffff,stroke-width:3px;
    classDef nxc fill:#071f2b,stroke:#00d4ff,color:#ffffff,stroke-width:4px;
    classDef service fill:#252525,stroke:#5ea9ff,color:#ffffff,stroke-width:2px;
    classDef normal fill:#3a2d00,stroke:#ffc107,color:#ffffff,stroke-width:3px;
    classDef pwned fill:#123524,stroke:#00ff88,color:#ffffff,stroke-width:4px;
    classDef next fill:#0b3d4d,stroke:#00d4ff,color:#ffffff,stroke-width:3px;
```

Así queda mucho más claro visualmente:

**NXC → muchos servicios → un único punto de decisión → 3 resultados posibles.**

Y además te deja preparado el siguiente bloque: qué hacer cuando **autentica pero no eres admin** frente a qué hacer cuando sale **PWN3D!**.
1. Sí puedes pasar rangos a NXC. Por ejemplo, conceptualmente 10.10.10.0/24, una IP concreta o una lista de objetivos. Pero para estudiar metodología, tiene más sentido primero descubrir servicios con Nmap y después probar solo donde corresponda, porque lanzar credenciales contra todo el rango y todos los protocolos es más ruidoso.

2. Pwn3d! no significa exactamente lo mismo en todos los protocolos. NetExec lo usa como indicador de que ha detectado capacidad relevante de ejecución o privilegios altos; en SMB suele apuntar a administrador local, mientras que WinRM, RDP, LDAP, etc. tienen criterios distintos. La propia documentación de NetExec advierte de esa diferencia.

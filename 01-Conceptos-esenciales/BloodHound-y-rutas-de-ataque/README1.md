Sí, exactamente. Y hay un matiz importante: **SharpHound y `bloodhound-python` son dos colectores alternativos**. No necesitas usar los dos necesariamente. Ambos recopilan información para luego cargarla/analizarla en **BloodHound**.

```mermaid
flowchart TD

    A["PARTIMOS SIN CREDENCIALES<br/>Conocemos dominio / DC"]:::start

    A --> B["ENUMERAR USUARIOS<br/><br/>Kerbrute"]:::recon

    B --> C["USUARIOS VÁLIDOS ENCONTRADOS<br/><br/>Eduardo<br/>Mari Carmen<br/>José Luis"]:::users

    C --> D{"¿KERBEROS PREAUTH<br/>DESHABILITADA?"}:::decision

    D -->|"Mari Carmen: NO"| MC["Preautenticación activada<br/>No AS-REP Roast"]:::failed
    D -->|"José Luis: NO"| JL["Preautenticación activada<br/>No AS-REP Roast"]:::failed

    D -->|"Eduardo: SÍ"| E["EDUARDO ES<br/>AS-REP ROASTEABLE"]:::attack

    E --> F["AS-REP ROASTING<br/><br/>Solicitar AS-REP<br/>Obtener material crackeable"]:::attack

    F --> G["CRACKING OFFLINE<br/><br/>Hashcat / diccionario"]:::attack

    G --> H["CREDENCIALES OBTENIDAS ✅<br/><br/>EMPRESA\\Eduardo<br/>Password: 123"]:::good

    H --> I["AHORA YA TENEMOS<br/>UNA IDENTIDAD VÁLIDA<br/>DEL DOMINIO"]:::good

    I --> J{"RECOLECTAR INFORMACIÓN DE AD"}:::decision

    J -->|"Windows"| K["SharpHound"]:::collector
    J -->|"Linux"| L["bloodhound-python"]:::collector

    K --> M["DATOS DE ACTIVE DIRECTORY"]:::data
    L --> M

    M --> N["BLOODHOUND"]:::blood

    N --> O["MAPA DEL 'HORMIGUERO' 🐜<br/><br/>Usuarios<br/>Grupos<br/>Equipos<br/>Membresías<br/>Sesiones<br/>ACL / DACL<br/>Administradores locales<br/>GPO<br/>Relaciones y rutas de ataque"]:::map

    O --> P["PREGUNTA CLAVE:<br/><br/>¿DESDE EDUARDO<br/>HASTA DÓNDE PUEDO LLEGAR?"]:::next

    classDef start fill:#111111,stroke:#ffffff,color:#ffffff,stroke-width:3px;
    classDef recon fill:#13293d,stroke:#3ca0ff,color:#ffffff,stroke-width:3px;
    classDef users fill:#252525,stroke:#aaaaaa,color:#ffffff,stroke-width:2px;
    classDef decision fill:#2b2140,stroke:#b084ff,color:#ffffff,stroke-width:3px;
    classDef failed fill:#3b0d0d,stroke:#ff4d4d,color:#ffffff,stroke-width:3px;
    classDef attack fill:#3a2d00,stroke:#ff9800,color:#ffffff,stroke-width:3px;
    classDef good fill:#123524,stroke:#35d07f,color:#ffffff,stroke-width:4px;
    classDef collector fill:#071f2b,stroke:#00d4ff,color:#ffffff,stroke-width:3px;
    classDef data fill:#252525,stroke:#5ea9ff,color:#ffffff,stroke-width:2px;
    classDef blood fill:#3b001e,stroke:#ff3d8d,color:#ffffff,stroke-width:4px;
    classDef map fill:#1f2933,stroke:#8ab4f8,color:#ffffff,stroke-width:3px;
    classDef next fill:#0b3d4d,stroke:#00d4ff,color:#ffffff,stroke-width:4px;
```

Tu idea del **“esqueleto del hormiguero”** es bastante buena:

> **Kerbrute descubre qué hormigas existen. → AS-REP Roasting puede darte la contraseña de una. → SharpHound/bloodhound-python recopilan cómo está construido el hormiguero. → BloodHound te lo dibuja y te enseña por dónde podría avanzar Eduardo.**

Ese es un salto importante respecto a limitarte a probar usuarios uno a uno con NXC.

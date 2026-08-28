Claro. Te pongo otro, esta vez con **dos OUs, una OU hija y dos GPOs**, para que tenga un poco más de trampa.

```mermaid
flowchart TD

    AD["🏢 empresa.local"]

    AD --> OUTEC["📁 OU Tecnología"]
    AD --> OURRHH["📁 OU RRHH"]

    OUTEC --> OUSOP["📁 OU Soporte"]

    OUTEC --> ANA["👤 Ana"]
    OUTEC --> PCDEV["💻 PC-DEV01"]

    OUSOP --> EDU["👤 Eduardo"]
    OUSOP --> PCSUP["💻 PC-SOPORTE01"]

    OURRHH --> MARIA["👤 María"]

    HELP["👥 HelpDesk"]
    DEV["👥 Developers"]

    EDU -. "MemberOf" .-> HELP
    MARIA -. "MemberOf" .-> HELP
    ANA -. "MemberOf" .-> DEV

    GPO1["📜 GPO-Tecnología"]
    GPO2["📜 GPO-Soporte"]

    GPO1 ==>|"Linked to"| OUTEC
    GPO2 ==>|"Linked to"| OUSOP

    FILTER["🔎 Security Filtering<br/>HelpDesk"]
    FILTER -.-> GPO2
```

Intenta decirme:

1. ¿Quién entra en el ámbito de `GPO-Tecnología`?
2. ¿Quién entra en el ámbito de `GPO-Soporte`?
3. ¿A quién terminaría afectando `GPO-Soporte` con el filtro `HelpDesk`?
4. ¿María recibe `GPO-Soporte` por pertenecer a `HelpDesk`?
5. ¿Eduardo recibe también `GPO-Tecnología` por estar dentro de una OU hija de `OU Tecnología`?

Aquí ya entra una idea nueva importante: **las GPO pueden heredarse hacia OUs hijas**.

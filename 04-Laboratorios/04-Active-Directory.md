# Laboratorio 04: Active Directory

## Objetivo

Partir de información mínima, obtener una cuenta de dominio, enumerar relaciones y encadenar técnicas hasta un objetivo privilegiado.

## Diseño sugerido

Dominio `corp.local` con:

- `DC01`, `SRV01` y `CLIENT01`;
- usuarios estándar y de servicio;
- una cuenta sin preautenticación con contraseña débil de laboratorio;
- un SPN en una cuenta de servicio;
- una ACL abusable y reversible;
- administración local delegada;
- una sesión privilegiada de prueba;
- subred interna alcanzable mediante pivot opcional.

## Fase A: descubrimiento

1. Identifica DC, dominio, DNS y hora.
2. Enumera candidatos de usuario.
3. Obtén la primera cuenta mediante la pista prevista o spray seguro.
4. Registra política de bloqueo.

## Fase B: enumeración autenticada

1. Usuarios, grupos, equipos y SPN.
2. Cuentas sin preauth.
3. Shares y secretos.
4. ACL, administración local y sesiones.
5. Recolección BloodHound.

## Fase C: ataques de Kerberos

1. Solicita AS-REP de la cuenta configurada.
2. Solicita TGS de la cuenta de servicio.
3. Identifica formatos.
4. Audita offline.
5. Compara precondiciones y resultados.

## Fase D: movimiento lateral

1. Valida nueva cuenta por SMB.
2. Comprueba administración local.
3. Accede mediante una técnica justificada.
4. Recupera el hash/ticket preparado en el escenario.
5. Demuestra PTH y PTT en objetivos distintos.

## Fase E: ruta privilegiada

1. Marca identidades controladas en BloodHound.
2. Encuentra una ruta a Domain Admin/Tier Zero.
3. Explica cada arista.
4. Ejecuta solo los cambios reversibles del diseño.
5. Demuestra el objetivo con un flag.
6. Revierte membresías/ACL.

## Preguntas de control

- ¿Qué diferencia hay entre el AS-REP y el TGS obtenido?
- ¿Qué cuenta cifra cada material?
- ¿Por qué PTH funcionó en SMB pero no necesariamente en otro servicio?
- ¿Qué SPN necesitó PTT?
- ¿Qué paso otorgó admin local y cuál control de dominio?
- ¿Qué dato de BloodHound verificaste manualmente?

## Aprobado

Puedes reconstruir toda la ruta sin consultar comandos, explicar cada identidad ganada y repetir el laboratorio después de cambiar nombres, IP y una relación del grafo.


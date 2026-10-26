# Trazabilidad de evidencias

Las capturas se conservan como imágenes del laboratorio. El índice completo figura en [evidencias](../evidencias/README.md).

| Hallazgo o afirmación | Imágenes | Alcance de la prueba |
|---|---|---|
| Identificación de red y host | E01–E02 | Descubrimiento |
| Puertos TCP/UDP | E03–E04 | Enumeración |
| Servicios y SO | E05–E08 | Identificación remota |
| SMTP VRFY | E09–E15 | Respuestas y enumeración |
| Acceso SSH | E16 | Sesión válida |
| Acceso a archivos y NFS | E18, E21 | Lectura y permisos |
| PostgreSQL | E17, E19–E20 | Datos y operación SQL |
| Privilegios finales | E22 | UID 0 observado |

Las capturas no prueban por sí solas que todas las acciones se ejecutaron bajo la misma sesión ni resuelven las inconsistencias de credenciales. El orden del índice sigue la extracción documental, no una cadena temporal forense validada.

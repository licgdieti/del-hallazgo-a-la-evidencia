# Matriz de hallazgos

| ID | Área | Evidencia | Estado | Riesgo cualitativo | Acción recomendada |
|---|---|---|---|---|---|
| H-01 | NFS | E18, E21 | Exportación amplia observada; alcance de permisos por precisar | Alto, condicionado | Restringir exportación y verificar UID/permisos |
| H-02 | SMTP | E12–E15 | Enumeración observada | Medio | Revisar VRFY y monitoreo |
| H-03 | SSH | E16 | Autenticación válida con algoritmos heredados | Medio, contextual | Actualizar configuración criptográfica |
| H-04 | PostgreSQL | E17, E20 | Acceso a archivos observado; credenciales inconsistentes | Alto, condicionado | Revisar privilegios y exposición de archivos |
| H-05 | Privilegios | E21–E22 | UID 0 observado; causalidad no aislada | Alto, condicionado | Auditar permisos y delegaciones |

**Nota:** la prioridad es una estimación argumentada, no un CVSS calculado ni un hallazgo independiente de una herramienta automática.

# Validaciones pendientes

| Prioridad | Pregunta | Evidencia adicional necesaria |
|---|---|---|
| Alta | ¿Cuáles eran las opciones exactas de exportación NFS? | Configuración de exportación, mapeo de UID y pruebas con identidades separadas |
| Alta | ¿Qué originó la sesión final con UID 0? | Línea temporal, usuario activo, permisos antes/después y registro de comandos |
| Alta | ¿Cuál era la credencial PostgreSQL válida en cada momento? | Historial de cambios y prueba autenticada fechada |
| Media | ¿Cuál era la versión local exacta del kernel y paquetes? | Inventario del sistema y parches |
| Media | ¿Cómo cambia el comportamiento SMTP tras endurecimiento? | Comparación antes/después con consultas equivalentes |
| Media | ¿Se eliminó el riesgo tras mitigaciones? | Re-test documentado |
| Futura | ¿Qué resultados produce fuzzing sobre un componente aislado? | Harness, corpus, cobertura y fallos reproducibles |

**No se completan estas celdas con datos supuestos.**

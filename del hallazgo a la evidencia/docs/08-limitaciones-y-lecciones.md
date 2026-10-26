# Limitaciones y lecciones aprendidas

## Limitaciones documentales

1. **Credenciales PostgreSQL:** aparecen dos contraseñas distintas atribuidas a la cuenta administrativa en momentos narrativos diferentes. No existe una cronología de cambio suficiente para reconciliarlas.
2. **Privilegios:** una captura acredita una shell `root` y UID 0, pero no demuestra de forma aislada que una modificación previa de permisos haya sido la causa única.
3. **Versiones:** la identificación remota de sistema operativo es una estimación; los banners no prueban por sí solos vulnerabilidad o parche exacto.
4. **UDP:** `open|filtered` es un estado ambiguo, no confirmación de servicio abierto.
5. **Impacto:** no se ejecutó un re-test documentado tras mitigaciones; las recomendaciones siguen pendientes de comprobación.
6. **0days:** no se realizaron campañas de fuzzing, reversing ni descubrimientos originales de vulnerabilidades desconocidas.
7. **Capturas:** muestran instantes de terminal y no sustituyen logs completos, configuraciones, marcas temporales y hashes de evidencia.

## Lecciones

- El acceso a un recurso depende de identidades, permisos y configuración, no solo del servicio visible.
- La enumeración de usuarios es un hallazgo de información, no una prueba de compromiso.
- La explotación de una condición de configuración debe distinguirse de un fallo en código.
- El contexto de privilegios puede cambiar por completo el significado de una misma operación.
- La honestidad sobre incertidumbres mejora la utilidad del informe y la reproducibilidad.

## Trabajo futuro

Aislar cada hipótesis en una instantánea nueva, registrar configuración de NFS y cuentas, verificar permisos efectivos, confirmar versiones locales, aplicar mitigaciones y repetir pruebas comparables. Para investigación de software, preparar un pequeño parser de laboratorio y medir cobertura y reproducibilidad de fallos sin afirmar resultados no obtenidos.

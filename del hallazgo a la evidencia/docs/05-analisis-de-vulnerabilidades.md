# Análisis de vulnerabilidades y patrones

## De la enumeración a la hipótesis

La presencia de servicios constituye información de superficie de ataque; para convertirla en hallazgo se necesita una propiedad insegura observable. En este caso aparecen cuatro patrones: **exposición de recursos**, **divulgación de identidades**, **dependencia de criptografía heredada** y **separación insuficiente de privilegios o acceso a archivos**.

## NFS: superficie de mayor interés

La exportación de `/` hacia clientes amplios, observada mediante `showmount`, merece prioridad porque incluye potencialmente rutas del sistema. La captura documenta lectura denegada sin privilegios locales y lectura posterior con `sudo` en el cliente. Este contraste obliga a analizar mapeo de UID, permisos, opciones de exportación y controles del servidor. No es correcto deducir de una sola lectura que cualquier usuario remoto tiene acceso irrestricto.

## SMTP: fuga de información

La diferencia entre respuestas a usuarios existentes e inexistentes permite inferir identidades locales. La enumeración no implica acceso a las cuentas. La mitigación debe considerar respuestas uniformes, limitación de consultas, monitoreo y necesidad operativa de VRFY.

## SSH: compatibilidad heredada

El acceso documentado requirió habilitar desde el cliente algoritmos heredados `ssh-rsa` y `diffie-hellman-group1-sha1`. Es una señal de deuda criptográfica y compatibilidad antigua. El uso de credenciales conocidas en laboratorio es un vector de autenticación, no prueba de explotación de SSH.

## PostgreSQL: datos y privilegios

Se observó lectura de un registro de autenticación en archivos expuestos y una prueba posterior de lectura de archivo desde el contexto del motor. La narración original atribuye a la cuenta administrativa dos contraseñas diferentes en secciones distintas. Sin registro de cambios o verificación reproducible, **no se afirma cuál era la contraseña vigente en cada momento**. Tampoco debe confundirse una capacidad SQL legítima de un rol privilegiado con una vulnerabilidad del motor.

## Encadenamiento de condiciones

La combinación de enumeración, acceso válido, exportaciones amplias y privilegios puede elevar el impacto respecto de cada observación aislada. Para sostener una cadena causal completa se necesitarían marcas temporales, identidad efectiva en cada operación, configuración de exportación y prueba de permisos antes/después.

## Priorización razonada

1. Restringir exportaciones NFS y verificar permisos.
2. Revisar cuentas, contraseñas y exposición de datos de autenticación.
3. Eliminar algoritmos SSH heredados cuando sea viable.
4. Reducir divulgación de identidades mediante SMTP.
5. Revisar segmentación y exposición de servicios innecesarios.

La priorización es cualitativa y contextual, no una puntuación CVSS calculada.

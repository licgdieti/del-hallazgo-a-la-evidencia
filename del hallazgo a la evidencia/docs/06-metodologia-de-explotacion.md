# Metodología de explotación y validación controlada

## Qué significa validar

En un laboratorio autorizado, validar un hallazgo significa comprobar si una condición permite una consecuencia específica bajo precondiciones documentadas. **No toda validación requiere explotación**: un cambio de respuesta, un archivo accesible o una política de exportación visible pueden bastar para demostrar la configuración insegura.

## Flujo de trabajo

1. **Hipótesis:** redactar el comportamiento esperado y el riesgo asociado.
2. **Precondiciones:** registrar identidad, red, versión, privilegios y configuración relevante.
3. **Prueba mínima:** escoger una acción no destructiva que permita observar el efecto.
4. **Controles:** contrastar con un caso que no cumpla la condición o con permisos distintos.
5. **Resultado:** guardar salida, evidencia y fecha; distinguir observación de inferencia.
6. **Causa raíz:** comprobar si el resultado depende de configuración, diseño, credenciales o defecto de implementación.
7. **Corrección y re-test:** repetir la misma condición tras aplicar una medida defensiva en un entorno controlado.

## Aplicación al laboratorio

- **SMTP:** respuesta diferenciada a consultas VRFY: evidencia de enumeración, no de toma de cuentas.
- **NFS:** exportación amplia y acceso a archivos: evidencia de exposición condicionada por permisos.
- **SSH:** inicio de sesión con credenciales válidas: evidencia de autenticación, no de un exploit del protocolo.
- **PostgreSQL:** acceso a un archivo desde el contexto del motor: evidencia de capacidad bajo un contexto de privilegios; requiere analizar permisos antes de atribuir vulnerabilidad.
- **Sesión privilegiada:** se observa UID 0, pero falta aislar el mecanismo exacto de elevación en una prueba independiente.

## Cuándo detenerse

Detener una prueba si existe riesgo de pérdida de datos, indisponibilidad, propagación, alteración de evidencia o salida del alcance. Usar instantáneas, registros y restauración. Evitar convertir una prueba de concepto en persistencia, extracción innecesaria o afectación de terceros.

## Diferencia entre PoC y explotación operacional

Una prueba de concepto debe mostrar una propiedad acotada con impacto mínimo. El desarrollo de exploits complejos exige controles adicionales y no es necesario para todos los hallazgos. En este trabajo se explica la metodología y se analizan resultados de laboratorio existentes; no se declara haber desarrollado un exploit original.

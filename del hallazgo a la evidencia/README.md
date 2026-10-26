# Metodología de análisis de vulnerabilidades y explotación

**Autor:** Gabriel Alejandro Dieticiewicz  
**Actividad:** Módulo 6 · Metodología de análisis de vulnerabilidades y explotación  
**Versión:** 1.0 · octubre de 2026  
**Estado:** Documentación preparada para revisión y publicación en GitHub.

## Propósito

Este proyecto propone una metodología técnica para estudiar vulnerabilidades desde el descubrimiento hasta la validación, la explicación de la causa raíz, la mitigación y la comunicación. Se apoya en un laboratorio virtual autorizado con un servidor Linux deliberadamente vulnerable y complementa los casos observados con un **plan de investigación de vulnerabilidades desconocidas**. El plan de 0days es metodológico: no se atribuye ningún descubrimiento original ni se afirma haber ejecutado fuzzing o reversing.

## Recorrido recomendado

1. [Introducción y objetivos](docs/01-introduccion-y-objetivos.md)
2. [Metodología de análisis](docs/02-metodologia-de-analisis.md)
3. [Herramientas y entorno](docs/03-herramientas-y-entorno.md)
4. [Caso práctico](docs/04-caso-practico.md)
5. [Análisis de vulnerabilidades](docs/05-analisis-de-vulnerabilidades.md)
6. [Metodología de explotación y validación](docs/06-metodologia-de-explotacion.md)
7. [Investigación de 0days](docs/07-investigacion-de-0days.md)
8. [Limitaciones y lecciones](docs/08-limitaciones-y-lecciones.md)
9. [Conclusiones y referencias](docs/09-conclusiones-y-referencias.md)

## Casos técnicos

- [NFS: exportación de raíz y permisos](casos/nfs/analisis-nfs.md)
- [SMTP: enumeración de usuarios](casos/smtp/analisis-smtp.md)
- [SSH: algoritmos heredados y autenticación](casos/ssh/analisis-ssh.md)
- [PostgreSQL: autenticación y lectura de archivos](casos/postgresql/analisis-postgresql.md)

## Evidencia y trazabilidad

- [Índice de capturas](evidencias/README.md)
- [Matriz de hallazgos](registros/matriz-hallazgos.md)
- [Trazabilidad de evidencias](registros/trazabilidad-evidencias.md)
- [Validaciones pendientes](registros/validaciones-pendientes.md)
- [Mapa metodológico](diagramas/metodologia.md)
- [Fuentes consultadas](referencias/fuentes-consultadas.md)

## Criterio de evidencia

**Observado** significa que la captura o el registro refleja la condición; **interpretado** es la explicación técnica sustentada; **pendiente** identifica afirmaciones que requieren pruebas adicionales. La presencia de un puerto abierto no demuestra por sí sola una vulnerabilidad. Una sesión privilegiada observada tampoco prueba automáticamente cuál fue el único mecanismo causal.

## Alcance y ética

Todo el contenido práctico corresponde a un entorno de laboratorio. Las rutas, IP privadas y usuarios son elementos de contexto y no deben extrapolarse a sistemas ajenos. No se incluyen instrucciones para atacar sistemas no autorizados ni se publican credenciales reales de terceros. Los procedimientos de validación se presentan como razonamiento defensivo y controlado.

## Publicación

Antes de hacer público el repositorio, revisar imágenes, nombres de usuario, metadatos y cualquier dato que pudiera identificar un entorno no destinado a divulgación. La publicación en GitHub **no se ha realizado desde este proyecto**.

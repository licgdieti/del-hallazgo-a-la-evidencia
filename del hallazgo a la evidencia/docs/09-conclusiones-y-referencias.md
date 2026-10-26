# Conclusiones y referencias

## Conclusiones

El análisis de vulnerabilidades es un proceso de investigación, no una colección de comandos. El laboratorio mostró cómo la exposición de servicios, las respuestas diferenciadas, la autenticación y los permisos sobre archivos pueden combinarse en escenarios de riesgo. El hallazgo más instructivo fue la exportación NFS excesivamente amplia: obliga a considerar identidades, controles de acceso y la diferencia entre leer un archivo y modificar una configuración.

La principal enseñanza es metodológica: un resultado debe poder explicarse, contextualizarse y verificarse. Por eso se distinguen hallazgos observados, interpretaciones y validaciones pendientes. También se evita atribuir a una herramienta una explotación que en realidad dependió de credenciales válidas o de un contexto privilegiado.

El flujo propuesto para 0days amplía esta lógica a software desconocido: estudiar superficies de entrada, observar comportamientos, investigar fallos, comparar versiones y comunicar hallazgos con responsabilidad. No se presenta como trabajo experimental realizado, sino como una estrategia técnica fundada en referencias públicas.

## Referencias principales

- NIST, *SP 800-115: Technical Guide to Information Security Testing and Assessment* (2008).
- OWASP, *Web Security Testing Guide*, versión estable 4.2.
- Google Project Zero, *Effective Fuzzing: A Dav1d Case Study* (2024).
- GitHub Docs, *About the repository README file* y documentación de enlaces relativos.

El detalle y los enlaces de consulta figuran en [fuentes consultadas](../referencias/fuentes-consultadas.md).

# Metodología de análisis de vulnerabilidades

## Principios

**Autorización, alcance, mínima intrusión, reproducibilidad, corroboración, reversibilidad y trazabilidad.** Cada hallazgo debe asociarse a una hipótesis, una prueba, una interpretación y un límite de certeza. La metodología toma como referencia la planificación, evaluación, análisis y mitigación descritos por NIST SP 800-115, sin presentarse como una implementación formal certificada.

## Fase 0. Definición de alcance

Identificar activos, ventana de trabajo, autorizaciones, exclusiones, riesgos operativos y mecanismo de recuperación. En un laboratorio, registrar versiones, instantáneas y configuración de red. **Entregable:** ficha de alcance.

## Fase 1. Descubrimiento

Identificar hosts alcanzables y distinguir las respuestas de red de la identidad real del activo. En el caso se utilizó `nmap -sn` para observar hosts activos. **Decisión:** continuar solo con activos identificados y autorizados.

## Fase 2. Enumeración

Levantar puertos, protocolos, banners, versiones y servicios auxiliares. Las respuestas `open|filtered` se registran como ambiguas. **Entregable:** inventario de superficie expuesta.

## Fase 3. Formulación de hipótesis

Transformar un dato en una pregunta contrastable: “¿permite la configuración NFS acceder a archivos que deberían estar restringidos?”, “¿responde SMTP de forma diferenciada a identidades existentes?”. Priorizar hipótesis por exposición, sensibilidad del recurso y probabilidad de impacto.

## Fase 4. Validación controlada

Seleccionar una comprobación mínima, no destructiva y con criterios de éxito definidos antes de ejecutarla. Conservar entrada, respuesta, hora, herramienta y contexto. Si el resultado depende de privilegios locales o credenciales previas, declararlo.

## Fase 5. Análisis de causa raíz

Distinguir síntoma y mecanismo: la enumeración SMTP es un síntoma observable; su causa puede estar en políticas de respuesta del servicio. El acceso por NFS puede depender de exportaciones amplias y del tratamiento de identidades y permisos. No asumir causalidad sin pruebas.

## Fase 6. Evaluación de impacto

Separar confidencialidad, integridad y disponibilidad. La severidad es contextual: exposición de archivos del sistema, posibilidad de lectura de datos, modificación de configuraciones y privilegios potenciales. Una puntuación CVSS no debe inventarse sin vector, producto y condiciones confirmadas.

## Fase 7. Mitigación y re-test

Proponer una corrección concreta, documentar el cambio y repetir una comprobación equivalente. Un resultado negativo solo es concluyente si el servicio y el camino de prueba siguen siendo comparables.

## Fase 8. Comunicación

Entregar resumen ejecutivo, matriz de hallazgos, evidencia, limitaciones, prioridades y plan de validación. Evitar afirmar “vulnerabilidad explotada” cuando solo se identificó una condición susceptible de abuso.

## Regla de clasificación

| Nivel | Criterio |
|---|---|
| Observado | Respuesta, archivo o estado visible en evidencia |
| Confirmado en laboratorio | Comportamiento comprobado dentro del alcance documentado |
| Inferido | Explicación plausible sustentada, pero no aislada causalmente |
| Pendiente | Falta una prueba para concluir |

## Diagrama

Ver [mapa metodológico](../diagramas/metodologia.md).

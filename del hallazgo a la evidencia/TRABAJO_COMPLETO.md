# Trabajo completo: metodología de análisis de vulnerabilidades y explotación

**Autor:** Gabriel Alejandro Dieticiewicz

> Versión de lectura continua. Los enlaces a imágenes se consultan desde los capítulos individuales en `docs/`.


---

# Introducción y objetivos

## Planteamiento

Analizar vulnerabilidades no equivale a ejecutar un escáner ni a reproducir un exploit. Consiste en construir hipótesis verificables, delimitar precondiciones, interpretar evidencia, estimar consecuencias y proponer controles comprobables. Un buen informe distingue el dato técnico del juicio del analista.

El caso de estudio utiliza dos máquinas virtuales: un equipo de análisis Kali Linux (`192.168.1.48`) y un servidor Linux de laboratorio (`192.168.1.49`). La topología pertenece a una red privada `192.168.1.0/24`. Los hallazgos abarcan servicios de red, enumeración de identidades, exposición de archivos y control de privilegios.

## Objetivo general

Proponer y aplicar una metodología reproducible de análisis de vulnerabilidades, con énfasis en la causa raíz, la validación prudente y la documentación profesional.

## Objetivos específicos

1. Establecer fases y puntos de decisión que permitan repetir el análisis.
2. Documentar ejemplos observados sin confundir exposición, vulnerabilidad e impacto confirmado.
3. Explicar el papel de herramientas de descubrimiento, enumeración y análisis.
4. Relacionar configuraciones inseguras con posibles cadenas de impacto.
5. Proponer un flujo de investigación para fallos desconocidos mediante fuzzing, diffing y reversing.
6. Preservar evidencias y registrar incertidumbres y pruebas pendientes.

## Tesis técnica

La evidencia más valiosa no es necesariamente el resultado más llamativo. En este laboratorio, una exportación NFS excesivamente amplia explica más sobre la superficie de riesgo que la mera cantidad de puertos abiertos. Sin embargo, la causalidad exacta de determinadas acciones posteriores requiere documentación adicional. Por ello se adopta una lectura conservadora de las pruebas.

---

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

Ver [mapa metodológico](diagramas/metodologia.md).

---

# Herramientas y entorno

## Entorno

- Equipo de análisis: Kali Linux, dirección privada `192.168.1.48`.
- Objetivo: máquina virtual Linux deliberadamente vulnerable, `192.168.1.49`.
- Red: `192.168.1.0/24`.
- Evidencias: capturas de terminal conservadas en [`evidencias/capturas`](evidencias/README.md).

## Herramientas observadas

| Herramienta | Uso documentado | Límite |
|---|---|---|
| Nmap | Descubrimiento, TCP SYN, UDP, servicios y SO | Identificación probabilística y estados ambiguos |
| Netcat | Comprobaciones de conectividad y diálogo SMTP | No caracteriza por sí solo el impacto |
| smtp-user-enum | Enumeración de respuestas VRFY | Los resultados dependen de la política SMTP |
| SSH | Acceso interactivo con una cuenta del laboratorio | El acceso válido no es por sí mismo explotación |
| showmount / cliente NFS | Comprobación de exportaciones y lectura | El efecto depende de permisos e identidades |
| psql | Interacción con PostgreSQL | Autenticación y acceso a archivos dependen del contexto |
| Metasploit / Meterpreter | Sesión documentada en el laboratorio | Una sesión no demuestra por sí sola un fallo de software |

## Ejemplos de comandos de descubrimiento ya documentados

```bash
sudo nmap -sn 192.168.1.0/24
sudo nmap -sS -p- 192.168.1.49
sudo nmap -sU --top-ports 100 192.168.1.49
sudo nmap -sV -p 21,25,80,3306 192.168.1.49
sudo nmap -O 192.168.1.49
```

Estos comandos se citan para interpretar un laboratorio previamente autorizado. No constituyen una invitación a ejecutarlos sobre redes ajenas.

## Resultados relevantes

- TCP abiertos: `22, 25, 53, 111, 139, 445, 2049, 2121, 3306, 5432, 36622, 43692, 44621`.
- UDP abiertos: `53, 111, 137, 2049`; estados `open|filtered` en `68` y `138`.
- Servicios identificados: Postfix SMTP, MySQL `5.0.51a-3ubuntu5`, NFS compatible con versiones `2–4`.
- Estimación remota de SO: Linux `2.6.X`, con rango estimado de kernel `2.6.9–2.6.33`; **no es una verificación local de la versión exacta**.
- Puertos `21` y `80` cerrados en las comprobaciones descritas.

## Herramientas de investigación propuestas, no ejecutadas

Ghidra para reversing, sanitizers para diagnóstico de fallos, libFuzzer/AFL++ para fuzzing y herramientas de comparación de binarios o código para diffing. Se presentan como opciones de metodología futura y no como evidencia de uso.

---

# Caso práctico: servidor Linux en laboratorio

## Escenario

Se estudió un servidor Linux deliberadamente vulnerable desde una estación Kali Linux en red privada. El trabajo avanzó desde la identificación de hosts y servicios hasta la revisión de controles de acceso y observación de una sesión con privilegios elevados.

## Secuencia de observaciones

1. **Descubrimiento:** se identificó `192.168.1.49` como objetivo de laboratorio entre los hosts activos.
2. **Superficie:** se registraron 13 puertos TCP abiertos y respuestas UDP que incluían NFS y RPC.
3. **Servicios:** se detectaron Postfix SMTP, MySQL y NFS; los puertos 21 y 80 aparecieron cerrados.
4. **Identidades:** SMTP VRFY diferenció respuestas para nombres conocidos e inexistentes; la herramienta de enumeración informó 30 coincidencias tras 175 consultas.
5. **Acceso:** se observó inicio de sesión SSH con una cuenta del laboratorio, utilizando compatibilidad con algoritmos heredados.
6. **Archivos:** la exportación NFS del directorio raíz permitió acceder a rutas del sistema desde el cliente del laboratorio; una lectura inicial sin privilegios fue denegada y otra con privilegios locales mostró el contenido de un archivo de prueba.
7. **Base de datos:** se inspeccionó información de autenticación PostgreSQL y posteriormente se documentó lectura de un archivo de prueba mediante el contexto de la base de datos. La documentación conserva **dos contraseñas distintas** atribuidas a la misma cuenta; no se da por resuelta esa discrepancia.
8. **Privilegios:** se observó una consola `root` con UID 0. No queda demostrado de forma inequívoca el vínculo causal entre todos los pasos previos y esa sesión final.

## Evidencias representativas






## Lectura crítica

La secuencia muestra por qué un análisis debe considerar configuraciones e interacciones entre servicios, no solo CVE. La exportación NFS amplia es una condición de alto interés, pero la atribución de todos los efectos posteriores a un único fallo requeriría pruebas aisladas y registro de permisos, configuración y sesión.

## Alcance de la conclusión

El caso demuestra exposición de superficie, enumeración, lectura de recursos y observación de una sesión privilegiada en laboratorio. No demuestra descubrimiento de un 0day, explotación de una vulnerabilidad de memoria ni una remediación ejecutada.

---

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

---

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

---

# Aproximación a vulnerabilidades desconocidas y 0days

## Objetivo y alcance

Un 0day es una vulnerabilidad desconocida o sin corrección disponible para el responsable en el momento relevante, según el contexto de uso del término. **Aquí se describe un flujo de investigación propuesto; no se afirma haber descubierto uno.** El propósito es convertir incertidumbre técnica en hipótesis reproducibles, minimizar daños y facilitar corrección responsable.

## 1. Selección del objetivo de investigación

Elegir un componente de código abierto, pequeño, mantenido y ejecutable de forma aislada, idealmente un parser de archivos o protocolo con corpus de pruebas disponible. Registrar licencia, versiones, entradas admitidas, dependencias y límites de autorización. Evitar servicios productivos o datos reales.

## 2. Construcción del entorno

Preparar compilaciones reproducibles, contenedores o máquinas virtuales, pruebas unitarias y un corpus de entradas benignas. Conservar hashes de binarios y versiones. Instrumentar el proceso para observar cierres anómalos, errores y consumo de recursos, sin exposición a terceros.

## 3. Comprensión de superficie y modelo de amenazas

Identificar rutas de entrada, formatos, parsers, validaciones de tamaño, conversiones de tipo, límites de confianza y componentes que consumen datos no fiables. Priorizar rutas alcanzables y funciones de alto riesgo por complejidad, no por fama del producto.

## 4. Reversing y análisis estático

Cuando hay código fuente, comenzar por revisión de funciones, llamadas y manejo de errores. Si solo hay binario, utilizar un desensamblador o decompilador para reconstruir flujo de control, límites y dependencias. Registrar hipótesis, nunca presentar pseudocódigo reconstruido como fuente original.

## 5. Fuzzing orientado a cobertura

Diseñar un *harness* que invoque una función concreta con entradas controladas, un corpus inicial válido y límites de tiempo y memoria. Medir cobertura y calidad de entradas; no basta con contar ejecuciones. Separar fallos reproducibles de timeouts o fallos del entorno. Priorizar la seguridad y el aislamiento del proceso.

## 6. Triage y causa raíz

Agrupar fallos por firma, reproducirlos en una compilación limpia y reducir la entrada que los provoca. Revisar registros de sanitizers y pila de llamadas; distinguir bug de disponibilidad, error lógico y posible corrupción de memoria. Un crash **no equivale automáticamente** a ejecución arbitraria de código.

## 7. Diffing de versiones

Comparar versiones anteriores y corregidas, revisar cambios en validaciones y pruebas de regresión y formular hipótesis sobre la clase de defecto. El diffing sirve para entender cambios y mejorar cobertura; no se utiliza aquí para derivar un exploit operativo.

## 8. Validación segura y comunicación

Documentar entrada mínima, versión afectada, condición observable, causa raíz propuesta, alcance del impacto y corrección sugerida. Coordinar la notificación privada al mantenedor, acordar plazos razonables y evitar publicar material que incremente el riesgo antes de la remediación.

## 9. Criterios de salida

Una investigación se considera suficientemente documentada cuando puede repetirse la observación, distinguirse del ruido, localizarse la condición causal y proponerse una prueba de regresión. Si no hay reproducción, el resultado debe quedar como hipótesis no confirmada.

## Caso real de referencia bibliográfica

La investigación publicada por Google Project Zero sobre el decodificador AV1 **dav1d** describe el hallazgo de un desbordamiento entero identificado como **CVE-2024-1580**, relacionado con una escritura fuera de límites. Se utiliza como ejemplo de cómo la cobertura, los caminos de ejecución y el análisis de causa raíz complementan el fuzzing. **Es un caso publicado por terceros, no una práctica ejecutada en este proyecto.**

## Herramientas candidatas

| Necesidad | Opciones a evaluar | Evidencia esperada |
|---|---|---|
| Revisión estática | Código fuente, Ghidra | Mapa de funciones y entradas |
| Instrumentación | Sanitizers y depurador | Traza del fallo |
| Fuzzing | libFuzzer o AFL++ | Corpus, cobertura, fallos reproducibles |
| Diffing | Control de versiones y comparadores | Cambios de comportamiento y regresión |
| Gestión | Registro de hipótesis y pruebas | Bitácora verificable |

Estas herramientas **no fueron ejecutadas** para este trabajo; representan un diseño de investigación futuro.

---

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

---

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

El detalle y los enlaces de consulta figuran en [fuentes consultadas](referencias/fuentes-consultadas.md).

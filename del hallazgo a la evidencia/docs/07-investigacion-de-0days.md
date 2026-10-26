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

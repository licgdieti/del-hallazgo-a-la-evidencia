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

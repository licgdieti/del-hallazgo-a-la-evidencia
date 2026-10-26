# Herramientas y entorno

## Entorno

- Equipo de análisis: Kali Linux, dirección privada `192.168.1.48`.
- Objetivo: máquina virtual Linux deliberadamente vulnerable, `192.168.1.49`.
- Red: `192.168.1.0/24`.
- Evidencias: capturas de terminal conservadas en [`evidencias/capturas`](../evidencias/README.md).

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

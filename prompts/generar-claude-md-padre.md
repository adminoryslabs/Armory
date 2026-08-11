---
name: generar-claude-md-padre
description: Genera el CLAUDE.md raíz de un ecosistema multirepo, con mapa de repos, contratos compartidos y diagrama de arquitectura.
type: prompt
tags: [multirepo, claude-md, arquitectura]
---

Estás en la carpeta raíz de un ecosistema multirepo (esta carpeta NO es un
repositorio git — cada subcarpeta lo es, con su propio remoto).

Explora cada repo hijo lo suficiente para entender:
- Su rol dentro del ecosistema (qué hace, para quién).
- De qué otros repos depende y quién depende de él (contratos de datos,
  endpoints que consume o expone, formatos compartidos).
- Dónde se despliega cada uno, si aplica.

Con eso, genera un CLAUDE.md en esta carpeta raíz que sirva de mapa general
del ecosistema: qué es, tabla de repos y su rol, contratos compartidos entre
repos, arquitectura de alto nivel (quién es cliente de quién).

Incluye también un diagrama de arquitectura (Mermaid) que muestre los repos,
sus relaciones y el flujo de datos entre ellos.

No repitas aquí el detalle interno de cada repo (stack, comandos, gotchas
propios) — eso va en el CLAUDE.md de cada hijo, no en este.

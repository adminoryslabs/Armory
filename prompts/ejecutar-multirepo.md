---
name: ejecutar-multirepo
description: Arma y ejecuta un plan de arranque ordenado para un ecosistema multirepo, leyendo los CLAUDE.md ya documentados de cada repo.
type: prompt
tags: [multirepo, claude-md, ejecucion]
---

Tienes un ecosistema multirepo ya documentado (CLAUDE.md padre + CLAUDE.md por cada
repo hijo, con la sección de cómo levantar cada uno en local).

Léelos y arma un plan de arranque único y ordenado para todo el ecosistema:
- Qué servicios dependen de otros (por ejemplo, una base de datos que un backend
  necesita antes de arrancar) y en qué orden deben levantarse.
- Qué prerequisitos hacen falta antes de arrancar cualquier cosa (herramientas
  instaladas, puertos libres, variables de entorno).
- Los comandos exactos para levantar cada repo, tomados de su propio CLAUDE.md —
  no inventes comandos que no estén documentados.

Si algún repo no documenta cómo levantarse en local, dilo explícitamente en vez de
adivinar.

Antes de ejecutar nada, muéstrame el plan completo y pídeme confirmación. Después,
ejecuta paso a paso y avisa si algo falla, explicando qué intentaste y por qué crees
que falló.

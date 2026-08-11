---
name: generar-claude-md-hijo
description: Genera el CLAUDE.md de un repo hijo dentro de un ecosistema multirepo, enlazado con el contexto del CLAUDE.md padre.
type: prompt
tags: [multirepo, claude-md, arquitectura]
---

Estás dentro de un repo que forma parte de un ecosistema multirepo mayor.
Si existe un CLAUDE.md en la carpeta padre (un nivel arriba), léelo primero
— ahí está el contexto global del ecosistema, no lo repitas aquí.

Genera un CLAUDE.md para este repo puntual, cubriendo:
- Qué hace y su stack técnico.
- Cómo se levanta en local (comandos reales, no genéricos).
- Decisiones de arquitectura propias de este repo.
- Gotchas o comportamientos no obvios que ya se descubrieron trabajando aquí.
- Cómo se relaciona con el resto del ecosistema (de quién depende, quién
  depende de él) — un párrafo corto, el detalle completo ya está en el
  CLAUDE.md padre.

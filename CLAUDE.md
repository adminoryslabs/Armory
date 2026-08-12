# adminoryslabs-skills

Repo público de contenido: `github.com/adminoryslabs/Armory`. No es un
servicio — no tiene build, no corre en local, no expone HTTP. `registry-api`
lo indexa vía la GitHub Trees API + `raw.githubusercontent.com` (ver contexto
del ecosistema en el `CLAUDE.md` padre).

## Qué hace y stack técnico

Contiene dos tipos de entradas, cada una en su propia convención de carpeta:

- **Skills** (`skills/<name>/SKILL.md`) — instrucciones completas para un
  agente, instalables vía `npx skills add adminoryslabs/Armory --skill
  <name>` ([skills.sh](https://www.skills.sh/)). Pueden traer `assets/` o
  `references/` junto al `SKILL.md`.
- **Prompts** (`prompts/<name>.md`) — texto corto pensado para pegarse tal
  cual en un agente, sin instalación. Consumidos por `launcher-desktop` vía
  `registry-api`.

No hay código fuente, package.json, ni tests — es contenido Markdown puro
más frontmatter YAML.

## Cómo se trabaja en local

No hay "levantar el repo" — es edición de archivos Markdown. El único ciclo
de trabajo real:

```bash
# Ver qué skills ya existen antes de crear una nueva (evitar duplicados)
ls skills/

# Instalar una skill puntual desde el propio repo para probarla end-to-end
npx skills add adminoryslabs/Armory --skill <name>

# Instalar todas (selector interactivo si no se pasa --skill)
npx skills add adminoryslabs/Armory
```

No hay CI de validación de frontmatter en este repo (no hay `.github/`) — la
única validación real ocurre en `registry-api` al sincronizar (parseo con
`gray-matter`) o al correr `npx skills add`. Si el frontmatter está mal
formado, el fallo aparece río abajo, no acá.

## Decisiones de arquitectura propias

- **Prompts sin instalación**: a diferencia de las skills (que requieren
  `npx skills add`), los prompts están pensados para copiar/pegar directo —
  de ahí que `launcher-desktop` los use para pegado inmediato con hotkey, sin
  pasar por un flujo de instalación.
- **`.atl/skill-registry.md`**: es un índice auto-generado (por
  `gentle-ai skill-registry refresh`) para que agentes delegadores elijan
  skills relevantes y pasen paths exactos a subagentes. Escanea múltiples
  fuentes locales del usuario (no solo este repo) — no confundirlo con el
  catálogo público que expone `registry-api`. No editar a mano.

## Gotchas descubiertos

- **El contrato de frontmatter del CLAUDE.md padre no se cumple
  uniformemente todavía**: el padre documenta `type: skill | prompt` como
  campo obligatorio, pero de las 6 skills actuales solo
  `descomponer-multirepo` declara `type: skill` explícitamente. Las demás
  (`generar-prd`, `generar-tech-design`, `revision-adversarial`,
  `generar-backlog`, `generar-prd-conversacional`) no tienen `type` en su
  frontmatter. Si `registry-api` depende de ese campo para clasificar,
  revisar cómo lo infiere quiere decir que probablemente hace fallback por
  la carpeta contenedora (`skills/` vs `prompts/`) y no solo por el campo —
  confirmar ahí antes de asumir que falta backfill acá.
- **`README.md` desactualizado respecto a `skills/`**: lista 5 skills pero
  hay 6 carpetas (falta `generar-prd-conversacional`, agregada en
  `bd7dc23`). Actualizar el README al agregar una skill nueva no es
  automático — hay que tocarlo a mano.

## Relación con el ecosistema

Este repo es la única fuente de contenido: `registry-api` lo sincroniza
(webhook `push` sobre `skills/` o `prompts/`) y lo expone por HTTP;
`registry-web` y `launcher-desktop` lo consumen indirectamente a través de
`registry-api`, nunca clonando este repo. Ver el `CLAUDE.md` padre para el
contrato completo de frontmatter y el flujo de sync.

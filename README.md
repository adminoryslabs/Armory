# Skills

Skills de agente instalables vía [skills.sh](https://www.skills.sh/).

## Instalación

Este repo tiene más de una skill. `npx skills add adminoryslabs/Armory` abre un selector
interactivo para elegir cuál(es) instalar — no instala todas automáticamente.

Para instalar una skill puntual sin el selector:

```bash
npx skills add adminoryslabs/Armory --skill generar-prd
npx skills add adminoryslabs/Armory --skill generar-prd-conversacional
npx skills add adminoryslabs/Armory --skill generar-tech-design
npx skills add adminoryslabs/Armory --skill revision-adversarial
npx skills add adminoryslabs/Armory --skill generar-backlog
npx skills add adminoryslabs/Armory --skill descomponer-multirepo
npx skills add adminoryslabs/Armory --skill create-harness
npx skills add adminoryslabs/Armory --skill security-pass
```

Para instalar todas:

```bash
npx skills add adminoryslabs/Armory --skill '*'
```

## Skills disponibles

- **`generar-prd`** — genera un PRD (Product Requirements Document) liviano a partir de una idea de producto, listo para revisar y pulir.
- **`generar-prd-conversacional`** — genera el PRD sección por sección conversando con el usuario, profundizando con preguntas y presentando alternativas cuando hay una decisión real, en vez de generarlo de una sola pasada.
- **`generar-tech-design`** — genera el Technical Design Document + ADRs (formato MADR) + criterios de aceptación, entrevistando al usuario decisión por decisión a partir de su PRD y Design.md.
- **`revision-adversarial`** — revisa de forma adversarial un Technical Design Document y sus ADRs, buscando activamente huecos y decisiones débiles en vez de validarlos. Mejor desde una conversación nueva.
- **`generar-backlog`** — despieza un PRD + Technical Design Document en un backlog ordenado de specs implementables, cada una lista para arrancar un ciclo de Spec-Driven Development.
- **`descomponer-multirepo`** — analiza un requerimiento sobre un ecosistema multirepo y lo descompone en specs por repo más los contratos de comunicación entre ellos.
- **`create-harness`** — guía al usuario, paso a paso y con preguntas, para diseñar su propio harness (combinación de skills, reglas o hooks) para una tarea recurrente de su proyecto.
- **`security-pass`** — corre una revisión de seguridad adaptada al proyecto real (producto, diseño, specs, código, tests), no un checklist fijo. Produce un reporte de findings priorizado y clasificado por tipo de acción requerida. Nunca modifica el proyecto.

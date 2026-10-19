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
npx skills add adminoryslabs/Armory --skill deploy-pass
npx skills add adminoryslabs/Armory --skill auditar-drift
npx skills add adminoryslabs/Armory --skill generar-readme
npx skills add adminoryslabs/Armory --skill init-agent
npx skills add adminoryslabs/Armory --skill invoke-agent
npx skills add adminoryslabs/Armory --skill playwright-e2e-test
npx skills add adminoryslabs/Armory --skill preparar-sprint0
npx skills add adminoryslabs/Armory --skill veredicto
npx skills add adminoryslabs/Armory --skill crear-guardrail
npx skills add adminoryslabs/Armory --skill escribir-contrato
npx skills add adminoryslabs/Armory --skill contextualizar-repo
npx skills add adminoryslabs/Armory --skill contextualizar-negocio
npx skills add adminoryslabs/Armory --skill escribir-hus
npx skills add adminoryslabs/Armory --skill audit-agent-context
```

Para instalar el **harness de construcción** (equipo LEAD · TESTER · DEV) en un solo paso:

```bash
npx skills add adminoryslabs/Armory --skill init-agent invoke-agent preparar-sprint0 veredicto crear-guardrail escribir-contrato -y
```

Para **contextualizar un sistema existente** (brownfield) con ARCH y BA:

```bash
npx skills add adminoryslabs/Armory --skill init-agent invoke-agent contextualizar-repo contextualizar-negocio audit-agent-context -y
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
- **`deploy-pass`** — analiza el proyecto real y diseña el sistema de deployment que necesita (build, artifact, config & secrets, infraestructura, estrategia de release, data & migrations, deploy gates, verify & observe, recovery), en vez de asumir una plataforma fija. Produce `DEPLOY-PLAN.md` antes de tocar nada, y nunca ejecuta una acción irreversible sin autorización explícita del usuario en ese momento.
- **`auditar-drift`** — compara lo prometido en `PRD.md` y las ADRs contra lo que el código realmente implementa: features fantasma, reglas omitidas, decisiones de arquitectura violadas y deuda técnica. Produce `DRIFT.md` con cada hallazgo evidenciado y sus dos opciones (corregir código o actualizar la doc), sin resolver la decisión por sí misma.
- **`generar-readme`** — genera un `README.md` pensado para un evaluador externo: quickstart real en 3 comandos, diagrama de arquitectura y stack, decisiones clave enlazadas a las ADRs, demo/video, y resumen ejecutivo del problema y los aprendizajes. Nunca inventa comandos ni links de demo — los verifica contra el proyecto real o los pide.
- **`init-agent`** — carga en la sesión actual el playbook de un rol de equipo ágil aumentado por IA (PM, BA, UX, ARCH, LEAD, TESTER, DEV) y adopta ese rol para el resto de la conversación, sin crear ni invocar ningún agente nuevo.
- **`invoke-agent`** — busca en el workspace actual de herdr un agente ya despierto para el rol pedido (PM, BA, UX, ARCH, LEAD, TESTER, DEV); si no hay ninguno, crea un pane nuevo, levanta un agente en él y le carga ese rol. Soporta el aviso de vuelta (`ENTREGA <ROL>`) para que el invocador sepa cuándo terminó.
- **`playwright-e2e-test`** — escribe y corre un test e2e de Playwright para un único flujo crítico, para que el agente verifique su trabajo en un navegador real en vez de afirmar que funciona. Propone el plan en lenguaje natural antes de escribir código, reutiliza el setup existente o instala uno mínimo, demuestra que el test puede fallar y, si falla, reporta con evidencia sin tocar el test para que pase: el humano decide.
- **`preparar-sprint0`** — prepara el Sprint 0 (walking skeleton) a partir del Tech Design y los ADRs: separa ADRs transversales de los de historia, define qué rutas son de cada rol según el stack, escribe la Definition of Done del esqueleto y un plan, y se detiene para aprobación humana. No construye nada.
- **`veredicto`** — cuando TESTER y DEV entregan, corre las pruebas del TESTER contra el código del DEV, verifica la spec punto por punto con evidencia (o `NO VERIFICADO`), comprueba que cada rol tocó solo sus rutas y arbitra cada rojo con el contrato en la mano (se apartó el código, se apartó la prueba o el contrato no lo decía), produciendo la matriz de trazabilidad para el PR.
- **`crear-guardrail`** — crea un guardrail o hook en el agente que se esté usando (Claude Code, OpenCode, Codex, Antigravity u otro): detecta qué agente es, consulta su documentación vigente, propone, espera aprobación, lo configura de forma nativa y lo prueba provocando el evento.
- **`contextualizar-repo`** — contextualiza un sistema existente (brownfield) para trabajar AI-first sobre él: arranque y pruebas verificados, índice de CodeGraph, mapa de capas, decisiones heredadas como ADR, catálogo de patrones (canon vs. legado), mapa de deuda y reglas de construcción verificables, todo con evidencia. La usa el ARCH. Nunca modifica código.
- **`audit-agent-context`** — audita si `AGENTS.md` y/o `CLAUDE.md` siguen reflejando la realidad del repo (estructura, comandos, convenciones, alcance) y reporta discrepancias sin editarlos.
- **`escribir-hus`** — convierte un PRD en Historias de Usuario construibles sin adivinar: una HU por funcionalidad del alcance, Como/Quiero/Para con al menos dos escenarios Gherkin, conflictos del PRD documentados y puntos abiertos como preguntas exactas al negocio. La usa el BA.
- **`contextualizar-negocio`** — documenta el contexto funcional de un sistema existente: mapa de capacidades, reglas de negocio escondidas en el código y glosario negocio ↔ código, en coordinación con el ARCH. La usa el BA. Nunca modifica código.
- **`escribir-contrato`** — escribe el contrato observable de una HU (entrada, salida por caso, errores, nombres que no se negocian, origen de cada línea) para que TESTER y DEV trabajen al mismo tiempo sin verse; lista las decisiones que la HU no tomaba y se detiene para aprobación humana.

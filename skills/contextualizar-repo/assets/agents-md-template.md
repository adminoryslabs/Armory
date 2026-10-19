# AGENTS.md — <Nombre del proyecto>

> Índice para agentes y personas. El detalle vive en `docs/contexto/`: abrir solo lo que la tarea
> necesite.

## Qué es

<Una o dos líneas: qué hace el sistema y para quién.>

## Cómo se arranca y se prueba

| Acción | Comando | Verificado |
|---|---|---|
| Instalar | `<comando>` | ✅ <fecha> / ⚠️ NO VERIFICADO |
| Arrancar | `<comando>` | |
| Pruebas | `<comando>` | |

## Mapa del contexto

| Necesito… | Leer |
|---|---|
| Entender cómo está construido | `docs/contexto/ARQUITECTURA.md` |
| Saber qué patrón imitar | `docs/contexto/PATRONES.md` |
| Saber dónde está la deuda antes de tocar algo | `docs/contexto/DEUDA.md` |
| Saber qué hace hoy el sistema | `docs/contexto/CAPACIDADES.md` |
| Entender un término del negocio | `docs/contexto/GLOSARIO.md` |
| Conocer las decisiones heredadas | `adrs/` |

## Reglas de construcción

| # | Regla | Origen | Cómo se verifica |
|---|---|---|---|
| R1 | <enunciado> | <ADR / patrón / doc del proyecto> | <comando, herramienta o prueba> |
| R2 | <enunciado> (regla blanda) | | <propuesta de verificación> |

## Antes de tocar código

1. Revisar si el cambio pasa por una zona de `docs/contexto/DEUDA.md`.
2. Imitar el **canon** de `docs/contexto/PATRONES.md`, nunca el legado aunque esté más cerca.
3. Usar los términos de `docs/contexto/GLOSARIO.md`.

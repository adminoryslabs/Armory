# Patrones — <Nombre del proyecto>

> Un agente imita el código que tiene más cerca. Este catálogo le dice cuál imitar.

## <Tema: ej. Cómo se agrega un endpoint>

| | Forma | Ejemplo | Usos aprox. |
|---|---|---|---|
| ✅ Canon | <descripción> | `archivo:línea` | |
| ⛔ Legado | <descripción> | `archivo:línea` | |

**Criterio para elegir el canon:** <documentación del proyecto / código más reciente / mayoría en
código nuevo>. **Estado:** Confirmado por <quién> · Pendiente de confirmar.

## <Tema: Dependencias>

| | Forma | Ejemplo | Consecuencia para probar |
|---|---|---|---|
| ✅ Canon | Inyectada | `archivo:línea` | Se prueba aislada |
| ⛔ Legado | Importada directo | `archivo:línea` | Hace falta el sistema entero |

## Preguntas abiertas

- <tema donde conviven varias formas sin criterio claro para elegir>

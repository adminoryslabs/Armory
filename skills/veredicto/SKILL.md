---
name: veredicto
description: Dicta el veredicto de una unidad de trabajo cuando el TESTER y el DEV ya entregaron - corre las pruebas del TESTER contra el código del DEV, arbitra cada rojo con la spec en la mano (se apartó el código, se apartó la prueba, o la spec no lo decía), comprueba que cada rol tocó solo sus rutas y produce la matriz de trazabilidad para el PR. En un refactor exige además "igual y mejor" - pruebas de caracterización verdes antes y después, pruebas existentes en verde y la mejora medida. Use when the LEAD has received both deliveries (ENTREGA TESTER and ENTREGA DEV) for a unit of work, or when asked to verify, judge, arbitrate, or give the verdict on a team delivery.
type: skill
tags: [lead, veredicto, arbitraje, trazabilidad, evidencia, refactor]
---

# Veredicto (v3)

## Goal

Responder con evidencia **"¿esta unidad de trabajo cumple su spec?"**, y cuando no la cumple,
**quién se apartó**. Cada resultado lleva una prueba, un comando o un `archivo:línea`, o queda
`NO VERIFICADO`.

> **Novedades v3:** veredicto de refactor ("igual y mejor"); las rutas de cada rol pueden venir
> de `AGENTS.md` o del contrato.

La spec depende del tipo de tarea:

| Tipo | Spec | Pregunta del veredicto |
|---|---|---|
| Arranque | `SPRINT0.md` | ¿Se cumple cada punto de la Definition of Done? |
| Feature | `contratos/CONTRATO-<ID>.md` | ¿Se cumple cada caso del contrato? |
| Refactor | `contratos/CONTRATO-<ID>.md` | ¿Quedó **igual** por fuera y **mejor** por dentro? |

## Input

- La spec en su versión vigente.
- La tarea de origen (HU, pedido o ítem de deuda), para la matriz.
- Los informes de `ENTREGA TESTER` y `ENTREGA DEV`.
- Las rutas de cada rol (`AGENTS.md` o el contrato).
- La rama de trabajo y su punto de partida.
- **En un refactor:** la salida de la corrida de las pruebas de caracterización contra el código
  actual, que el LEAD guardó antes de invocar al DEV.

Si falta una entrega, **no se dicta veredicto**: se espera.

## Workflow — común

1. **Verificar la propiedad de rutas.** `git log <base>..HEAD --name-only --format="%s"`, agrupar
   por prefijo (`[TESTER]` / `[DEV]` / `[LEAD]`) y comparar contra las rutas de cada rol. Archivos
   fuera de las rutas del rol o commits sin prefijo son **hallazgos**, aunque todo pase.

2. **Verificar que los dos trabajaron sobre la misma versión de la spec.** Si no, el veredicto no
   vale: se avisa a quien quedó atrasado.

3. **Correr las pruebas del TESTER** contra el código del DEV. Guardar la salida.

4. **Resultado por cada caso de la spec:** 🟢 / 🔴 / `NO VERIFICADO`, con la prueba que lo cubre.
   Un caso sin prueba es `NO VERIFICADO`, no verde.

5. **Arbitrar cada 🔴 con la spec en la mano.** Leer qué dice la spec del caso, qué espera la
   prueba y qué hace el código:
   - **Se apartó el código**: la prueba coincide con la spec y el código no → vuelve al **DEV**
     con el caso, la cita de la spec y la evidencia.
   - **Se apartó la prueba**: el código coincide con la spec y la prueba no (nombre, formato, caso
     que la spec no pide) → vuelve al **TESTER** con lo mismo.
   - **La spec no lo decía**: los dos son lecturas razonables de una spec incompleta o ambigua →
     **no corrige ningún agente**: se prepara la pregunta para el humano y, con su respuesta, el
     LEAD emite la nueva versión y avisa a ambos.
   **Corrige quien se apartó de la spec.**

6. **Vigilar trampas** y reportarlas como hallazgos aunque todo esté en verde:
   - pruebas que pasan sin verificar nada;
   - código con casos especiales que solo sirven para pasar una prueba puntual;
   - pruebas modificadas entre vueltas sin una nueva versión de la spec que lo justifique.

7. **Contar vueltas.** El mismo desacuerdo no se devuelve una tercera vez: se escala al humano.

## Workflow — adicional en un refactor

8. **IGUAL — antes y después.**
   - **Antes:** la corrida guardada por el LEAD muestra todas las pruebas de caracterización en
     verde contra el código original. Si no existe, se reproduce: se vuelve al commit base con las
     pruebas del TESTER (`git stash`/`git worktree`) y se corren. Sin "antes", el caso es
     `NO VERIFICADO`.
   - **Después:** las mismas pruebas en verde contra el código del DEV (paso 3).
   - **Pruebas existentes del proyecto:** los comandos que lista el contrato, en verde.
   Una prueba de caracterización en rojo **después** del cambio significa que el refactor cambió
   algo observable: casi siempre **se apartó el código**.
   - **Si el contrato describía mal el comportamiento de hoy** (la foto del TESTER muestra otra
     cosa), manda la foto: el LEAD corrige el contrato, sube la versión y la lleva al gate humano.
     No corrige ningún agente.
   - **Casos `NO VERIFICADO`** (no se podían provocar desde afuera): revisar en el diff cómo se
     comportaba ese caso antes y después. Si cambió algo observable, por chico que sea (un
     mensaje, un campo), **se escala al humano** con dos opciones: aceptarlo en un contrato v2 o
     restaurar el comportamiento anterior.

9. **MEJOR — la mejora medida.** Repetir la medición que define el contrato (comando, búsqueda,
   CodeGraph, diff) y comparar con el valor de "hoy". Se mide, no se acepta lo que dice el informe
   del DEV.

10. **Desenlace del refactor:**

    | Igual | Mejor | Desenlace |
    |---|---|---|
    | ✅ | ✅ | Refactor terminado |
    | ✅ | ❌ | Se movió código sin resolver la deuda → vuelve al DEV |
    | ❌ | — | Se cambió comportamiento: no es un refactor → vuelve al DEV |

11. **Revisar el cierre del DEV:** comentario del porqué en el código tocado (si falta, es un
    hallazgo menor) y deuda nueva reportada en el informe.

## Matriz de trazabilidad

- **Feature:**

  | Criterio de aceptación | Caso del contrato | Prueba | Resultado |
  |---|---|---|---|

  Criterio sin caso o sin prueba → algo no se verificó. Caso sin criterio → el negocio debe
  confirmar que lo quiere.

- **Refactor:**

  | Caso que no cambia | Prueba | Antes | Después |
  |---|---|---|---|

  Más una fila por la mejora medible: qué se midió, valor de hoy, valor después, comando.

- **Arranque:** tabla de la Definition of Done (punto → evidencia → resultado). Las pruebas que el
  TESTER marcó `NO EJECUTADAS` se corren ahora que el runner existe.

## Informe

Escribir `informes/VEREDICTO-<ID>.md`: matriz, hallazgos de rutas, arbitrajes (desenlace +
destinatario + vuelta N), trampas detectadas, `NO VERIFICADO`, versión de la spec usada y, en un
refactor, el desenlace "igual y mejor".

**Si todo está en verde** (o los `NO VERIFICADO` fueron aceptados por el humano), el LEAD abre el
PR con la matriz y la versión final de la spec en la descripción.

## Constraints

- No se modifica código ni pruebas para "hacer pasar" el veredicto.
- No se confía en lo que dicen los informes: se verifica.
- El LEAD no decide lo que la spec no dice: lo escala.
- No se publica nada (ni push a la rama principal, ni merge).

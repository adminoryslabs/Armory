# Ejemplos aplicados — escribir-hus

Casos resueltos en proyectos anteriores. Cada proyecto nuevo se agrega aquí como un caso más, sin modificar el proceso de la skill.

### Proyecto: Clínica Salud Total (salud digital) — Digitalización de la experiencia del paciente

PRD → `HU-01-agendar-cita.md`, `HU-02-reprogramar-cita.md`, `HU-03-cancelar-cita.md`:

- La "notificación de confirmación" (ítem separado en el Alcance) se integró como criterio de aceptación dentro de cada HU en vez de convertirse en una HU propia, por no ser una acción que el paciente invoque por separado.
- `HU-03-cancelar-cita.md` se construyó pese a que "cancelación" no aparece en el listado de "Incluye" del PRD, porque el Objetivo y el Criterio de éxito #3 la exigen — se documentó el conflicto en vez de omitir la HU o inventar que estaba aprobada.
- Los Puntos Abiertos del PRD (canal digital exacto, política de cancelación a último minuto) se replicaron literalmente como preguntas a negocio en cada HU afectada, sin asumir una respuesta.
- Al recibir wireframes del Diseñador/UX, se detectó un elemento ("copago") que violaba el "No incluye" del PRD y se rechazó explícitamente en vez de dejarlo pasar como mejora de diseño; también se detectó un Punto Abierto nuevo (SMS vs. WhatsApp para notificaciones) que no estaba registrado y se agregó a las 3 HUs.

### Proyecto: Reserva de Canchas para Complejo Deportivo

PRD → `HU-01-ver-disponibilidad.md`, `HU-02-reservar-cancha.md`, `HU-03-cancelar-reserva.md`, `HU-04-calendario-administrador.md` (carpeta `reservas-complejo-deportivo/`):

- El PRD no traía una sección "Objetivo" explícita ni "Supuestos" separada de "Puntos Abiertos" — se adaptó el proceso sin cambiarlo: el objetivo se extrajo de la última frase de "Problema", y los Puntos Abiertos se trataron igual que en el playbook general.
- A diferencia de Clínica Salud Total, aquí "cancelación" sí está explícitamente en el Alcance — Incluye, sin conflicto de alcance en esa HU.
- Se detectó un conflicto distinto: "Actores" describe al Administrador como alguien que "gestiona canchas", pero el Alcance — Incluye solo dice que "ve las reservas del día", y ningún Criterio de éxito exige gestión de canchas. A diferencia del caso de cancelación en Salud Total, aquí NO se construyó una HU de gestión de canchas — el propio PRD ya traía esta ambigüedad marcada como Punto Abierto, así que se dejó como tal (no como HU) en las HUs que dependen de esos datos (Ver disponibilidad y Calendario del administrador).
- Los 6 Puntos Abiertos del PRD se repartieron solo en las HUs donde aplican literalmente (ej. "Identificación del jugador" no se repitió en la HU de "Ver disponibilidad" porque la pregunta original del PRD está acotada a "reservar", no a "ver"), en vez de copiarlos todos en todas las HUs por defecto.

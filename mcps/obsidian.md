---
name: obsidian
description: API REST local de Obsidian para que el agente lea/escriba notas del vault
type: mcp
tags: [obsidian, notes, mcp, plugin]
repo_url: https://github.com/coddingtonbear/obsidian-local-rest-api
docs_url: https://coddingtonbear.github.io/obsidian-local-rest-api/
---

Este MCP le da al agente acceso de lectura y escritura a tu vault de
Obsidian — puede buscar notas, leer contenido, y crear o modificar archivos
del vault sin que tengas que copiar y pegar manualmente entre Obsidian y el
chat.

Antes de instalar nada, revisá `repo_url` y `docs_url` para confirmar el
método de configuración vigente — tanto el plugin de Obsidian como el MCP
server que lo consume pueden cambiar de versión o de forma de configurarse,
y este prompt puede haber quedado desactualizado.

A diferencia de otros MCPs, este no se instala con `npx`/`npm`: la mitad de
la instalación pasa por Obsidian mismo. Antes de seguir, PARÁ y confirmá
conmigo estos tres puntos — no asumas ninguno:
- ¿Tenés el plugin `Local REST API` instalado y habilitado dentro de
  Obsidian (Community Plugins, o vía BRAT si lo instalaste como plugin beta)?
- ¿Ya generaste el bearer token desde la pestaña de configuración de ese
  plugin? Si no lo tenés, pedime que te guíe a esa pantalla antes de
  continuar — nunca lo voy a inventar.
- ¿Vas a usar HTTP (puerto 27123, hay que habilitarlo explícitamente en la
  config del plugin) o HTTPS (puerto 27124, es el default, usa un
  certificado autofirmado que tu cliente HTTP puede necesitar aceptar
  manualmente)?

Con esos tres datos confirmados, configurá el servidor MCP en tu cliente
(Claude Code, Claude Desktop, etc.) apuntando al puerto y protocolo que
elegiste, pasando el bearer token como variable de entorno o campo de
configuración según indique `docs_url` para la versión vigente — usá
`<TU_TOKEN_AQUI>` como placeholder en cualquier ejemplo que compartas, y
reemplazalo por el valor real solo en tu configuración local.

Para verificar que quedó funcionando, pedile al agente que liste las notas
de una carpeta del vault o lea el contenido de una nota puntual — si
devuelve datos reales de tu vault, la conexión está bien configurada.

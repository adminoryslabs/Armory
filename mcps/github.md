---
name: github
description: MCP oficial de GitHub — issues, PRs, repos y búsqueda desde el agente
type: mcp
tags: [github, git, mcp]
repo_url: https://github.com/github/github-mcp-server
---

Este MCP conecta al agente con la API de GitHub: puede leer y crear issues,
revisar y abrir pull requests, buscar código y repos, y en general operar
sobre GitHub sin que vos tengas que salir del editor o la terminal.

Antes de instalar nada, revisá el README de `repo_url` para confirmar el
método de instalación vigente — este prompt puede quedar desactualizado si el
proyecto cambia su forma de distribución (por ejemplo, si pasa de un binario
descargado a un paquete `npx`, o cambia el nombre del servidor en la config
del cliente MCP).

Este MCP necesita un access token de GitHub para autenticarse. Si no lo
tenés a mano, PARÁ acá y pedímelo antes de seguir — no asumas un scope ni
generes un token en mi nombre. Cuando lo tengas, usá el placeholder
`<TU_TOKEN_AQUI>` en la configuración, nunca un valor de ejemplo que parezca
un token real. Los scopes exactos que necesita el token dependen de qué
operaciones vas a usar (lectura de repos, gestión de issues/PRs, etc.) —
confirmalos contra la sección de autenticación del README oficial en el
momento de instalar, no contra lo que diga este prompt.

Con el token ya generado, agregá el servidor MCP a la configuración de tu
cliente (Claude Code, Claude Desktop, etc.) siguiendo el formato que indique
el README de `repo_url` para ese cliente puntual, pasando el token como
variable de entorno o campo de configuración (`GITHUB_PERSONAL_ACCESS_TOKEN`
o el nombre que use la versión vigente) — reemplazá `<TU_TOKEN_AQUI>` por el
valor real solo en tu configuración local, nunca en un archivo versionado.

Para verificar que quedó funcionando, pedile al agente que liste tus
repositorios o busque un issue puntual en un repo al que tengas acceso — si
responde con datos reales de GitHub, la conexión está bien configurada.

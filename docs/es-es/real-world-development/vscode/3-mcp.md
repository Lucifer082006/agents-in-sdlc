---
title: "Ejercicio 3 - Prueba la funcionalidad con el servidor MCP de Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Modo agente][previous-lesson] · [Siguiente lección: Agentes personalizados →][next-lesson]

Acabas de crear la funcionalidad de filtrado con el modo agente. Antes de abrir una solicitud de incorporación de cambios, debes confirmar que funciona en el navegador. En lugar de probar la aplicación manualmente, conectarás el **servidor MCP de Playwright** y dejarás que Copilot controle un navegador real para probar la funcionalidad. Después, publicarás la rama y abrirás la PR.

En este ejercicio:

- aprenderás qué es Model Context Protocol (MCP) y cómo amplían sus servidores las herramientas disponibles en Copilot.
- añadirás el servidor MCP de Playwright al espacio de trabajo.
- pedirás a Copilot que lo utilice para probar manualmente la funcionalidad de filtrado en un navegador.
- publicarás la rama y abrirás una solicitud de incorporación de cambios para el trabajo de filtrado.

## ¿Qué es Model Context Protocol (MCP)?

El modo agente es mucho más potente cuando puede acceder a herramientas externas al editor. Copilot lo consigue mediante Model Context Protocol (MCP), un estándar que permite al agente comunicarse con herramientas y servicios externos.

![Diagrama de cómo funciona el modo agente y cómo interactúa con el contexto, el LLM y las herramientas, incluidas las que aportan los servidores MCP y las extensiones de VS Code](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que los agentes de IA se comuniquen con herramientas y servicios externos en tiempo real. Así pueden acceder a información actualizada mediante recursos y realizar acciones en tu nombre mediante herramientas.

Se accede a estas herramientas y recursos a través de un servidor MCP. Este actúa como puente entre el agente de IA y las herramientas y servicios externos, y gestiona la comunicación entre ellos; por ejemplo, con API existentes o herramientas locales como los paquetes de NPM. Cada servidor MCP proporciona un conjunto distinto de herramientas y recursos a los que puede acceder el agente.

Algunos servidores MCP populares son:

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: proporciona acceso a un conjunto de API para gestionar repositorios de GitHub. Permite al agente crear repositorios, actualizar los existentes y gestionar incidencias y solicitudes de incorporación de cambios.
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: ofrece funciones de automatización del navegador con Playwright. Permite al agente abrir páginas web, rellenar formularios y seleccionar botones.

Hay muchos otros servidores MCP disponibles que proporcionan acceso a diferentes herramientas y recursos. GitHub aloja un [registro de MCP](https://github.com/mcp) para facilitar su descubrimiento y las contribuciones al ecosistema.

> [!CAUTION]
> Desde el punto de vista de la seguridad, trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de utilizar uno, revisa detenidamente su código fuente, verifica quién lo publica y evalúa las implicaciones de seguridad. Utiliza solo servidores MCP de confianza y ten cuidado al conceder acceso a recursos u operaciones sensibles.

## Revisa la configuración de MCP

El archivo `.vscode/mcp.json` configura los servidores MCP disponibles en este espacio de trabajo de VS Code.

1. Abre `.vscode/mcp.json` en el codespace.
2. Verás que ya está configurado un servidor `github`:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

La plantilla del proyecto ya incluye esta entrada `github`, por eso Copilot pudo consultar la lista de incidencias en el ejercicio anterior. Utiliza el [servidor MCP remoto de GitHub][remote-github-mcp-server], así que no hay que instalar nada localmente: VS Code se conecta por HTTP y te autenticas con GitHub la primera vez que Copilot utiliza una de sus herramientas.

## Añade el servidor MCP de Playwright

Ahora añadirás un segundo servidor. El [servidor MCP de Playwright][playwright-mcp-server] proporciona a Copilot un navegador que puede controlar, justo lo que necesitas para probar la funcionalidad.

1. En `.vscode/mcp.json`, añade una entrada `playwright` junto a `github`, de modo que el archivo quede así:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. Guarda el archivo.

A diferencia del servidor remoto `github`, `playwright` es un servidor **local**: VS Code lo inicia en tu máquina ejecutando `npx @playwright/mcp@latest`. La opción `--headless` indica a Playwright que ejecute el navegador sin mostrar una ventana. Es necesario dentro de un codespace, donde no hay un escritorio en el que mostrarla.

> [!NOTE]
> El proyecto Tailspin Toys ya utiliza Playwright para sus pruebas de extremo a extremo, así que normalmente el navegador que necesita Playwright ya está instalado. Si Copilot indica que falta un navegador, pídele que ejecute `npx playwright install chromium` y vuelve a intentarlo.

## Inicia el servidor y confirma que es de confianza

VS Code inicia los servidores MCP **cuando se necesitan**: la primera vez que Copilot requiere las herramientas de un servidor, VS Code lo inicia y te pide que confirmes que es de confianza.

1. En `.vscode/mcp.json` aparecerá una acción **Start** encima de la entrada `playwright`. Selecciónala para iniciar el servidor y confirmar que la configuración es correcta.
2. Si VS Code te pregunta si confías en el servidor, revisa su configuración y confirma que confías en él para que pueda iniciarse.

Este paso es opcional: si lo omites, VS Code iniciará el servidor y te pedirá que confirmes que es de confianza la primera vez que Copilot necesite sus herramientas en el paso siguiente.

## Prueba la funcionalidad de filtrado

El [servidor MCP de Playwright][playwright-mcp-server] proporciona a Copilot un navegador real que puede controlar. En vez de navegar por la aplicación para comprobar el trabajo, el agente puede abrir una página, desplazarse por ella, aplicar filtros y leer los resultados para, después, resumir lo que ha observado. Es la forma más rápida de comprobar que la funcionalidad se comporta como esperas, sin salir de la conversación.

Internamente, el servidor MCP de Playwright utiliza el [árbol de accesibilidad de la página][playwright-mcp-server] en lugar de capturas de pantalla. Así, el agente analiza elementos estructurados y etiquetados —botones, enlaces y elementos de listas— del mismo modo que las tecnologías de asistencia. Por tanto, una comprobación funcional rápida también sirve como verificación básica de accesibilidad.

Con el servidor conectado y la aplicación en marcha, pide a Copilot que pruebe la funcionalidad de filtrado que acabas de crear:

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot abrirá un navegador mediante el servidor MCP de Playwright, seguirá los pasos y te comunicará lo que encuentre. Compara el informe con los criterios de aceptación de la incidencia. Si detectas algún problema, haz preguntas de seguimiento o pide a Copilot que corrija el código antes de abrir la PR.

> [!NOTE]
> La aplicación debe estar en marcha en `http://localhost:4321` para esta prueba. Si has detenido el servidor de desarrollo, vuelve a iniciarlo antes de enviar el prompt. La primera vez que Copilot utilice el servidor MCP de Playwright, quizá tenga que descargar un navegador. Si indica que falta, pídele que ejecute `npx playwright install chromium` y vuelve a intentarlo.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publica la rama y crea una solicitud de incorporación de cambios

Ya has confirmado que la funcionalidad funciona y puedes abrir una solicitud de incorporación de cambios (PR) para que el equipo la revise. El primer paso es publicar la rama `filtering-vscode`.

1. Abre el panel **Source Control** del codespace y revisa los cambios de Copilot.
2. Prepara los cambios seleccionando el icono **+**.
3. Genera un mensaje de confirmación con el botón **Sparkle**.

   ![Captura de pantalla del panel Source Control con los cambios realizados](../../../_images/ex3-source-control-changes.png)

4. Selecciona **Publish** para enviar la rama al repositorio.

## Crea la solicitud de incorporación de cambios

Hay varias formas de crear una solicitud de incorporación de cambios, por ejemplo, en github.com o mediante la interfaz de línea de comandos (CLI) de GitHub. Como ya estás trabajando con GitHub Copilot, deja que cree la PR por ti. Puede encontrar la incidencia pertinente y vincularla a la solicitud.

1. Abre el panel Copilot Chat y selecciona **New Chat** para iniciar una sesión nueva.
2. Confirma que **Agent** está seleccionado en el selector de agentes para que Copilot pueda utilizar las herramientas de GitHub.
3. Pide a Copilot que cree la PR:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Cuando sea necesario, selecciona **Continue** para permitir que Copilot recopile información y realice las operaciones necesarias. La primera vez que utilice una herramienta de GitHub, quizá tengas que autenticarte; sigue los pasos para permitirlo.
5. Observa cómo Copilot busca entre las incidencias, encuentra la correcta y crea la PR.
6. Selecciona el enlace que genera Copilot para revisar la solicitud, pero **no la combines todavía**.

## Resumen y pasos siguientes

¡Enhorabuena! En este ejercicio:

- has aprendido qué es Model Context Protocol (MCP) y cómo amplían sus servidores las herramientas disponibles en Copilot.
- has añadido el servidor MCP de Playwright al espacio de trabajo.
- lo has utilizado para probar manualmente la funcionalidad de filtrado en un navegador antes de publicar los cambios.
- has publicado la rama y abierto una solicitud de incorporación de cambios para el trabajo de filtrado.

Has utilizado un servidor MCP para *probar* una funcionalidad, pero MCP es solo una forma de ampliar Copilot. A continuación, [crearás un agente personalizado][next-lesson] para agilizar tareas específicas como las revisiones de accesibilidad.

## Recursos

- [¿Qué es MCP y por qué todo el mundo habla de ello?][mcp-blog-post]
- [Servidor MCP de Microsoft Playwright][playwright-mcp-server]
- [Servidor MCP de GitHub][github-mcp-server]
- [Registro de GitHub MCP][mcp-registry]
- [Servidores MCP en VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration

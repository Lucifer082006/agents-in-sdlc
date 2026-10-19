---
title: "Ejercicio 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot te permiten crear asistentes de IA especializados para tareas o dominios concretos de tu flujo de desarrollo. Puedes definirlos mediante archivos Markdown en la carpeta `.github/agents` del repositorio y proporcionar instrucciones centradas en una tarea, prácticas recomendadas, patrones de programación y conocimientos del dominio para ayudar a Copilot a realizarla con mayor eficacia. Los equipos pueden plasmar su experiencia en agentes reutilizables: por ejemplo, un agente de accesibilidad que aplique las directrices de [WCAG][wcag], uno de seguridad que siga prácticas de programación segura o uno de pruebas que mantenga patrones coherentes.

Los agentes personalizados se definen mediante archivos Markdown en la carpeta `.github/agents` del proyecto o de forma global en `~/.copilot/agents`. Cada archivo tiene una cabecera YAML con, como mínimo, los campos `name` y `description`, seguidos de un prompt Markdown que define el comportamiento, la experiencia y las instrucciones del agente.

### Diferencias entre agentes personalizados y habilidades de agente

Los agentes personalizados y las [habilidades de agente][agent-skills-concept] tienen algunos puntos en común. Ambos se definen principalmente mediante archivos Markdown e indican a la IA cómo realizar operaciones. La diferencia más clara es que un **agente personalizado** es quien trabaja y las **habilidades** son las herramientas.

Los agentes personalizados tienen su propia ventana de contexto y están diseñados para coordinar habilidades e incluso otros agentes como parte del trabajo. En este taller, el agente personalizado de accesibilidad revisa y actualiza el sitio de acuerdo con las directrices de accesibilidad. Como parte de esa tarea, podría invocar habilidades, por ejemplo, una para seguir el flujo de trabajo de las solicitudes de incorporación de cambios u otra para ejecutar y gestionar pruebas.

> [!NOTE]
> No existe una única manera correcta de crear un agente personalizado. Como ocurre con cualquier herramienta de IA, prueba distintas opciones e itera hasta encontrar la que funcione en tu entorno y escenario.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Escenario

Tailspin Toys quiere que su plataforma de financiación colectiva sea accesible para todas las personas, independientemente de su capacidad visual o sus preferencias. Los comentarios recientes indican que a algunas personas les cuesta leer el tema oscuro actual porque el contraste entre el texto y el fondo es insuficiente. Para abordar este problema de accesibilidad, el equipo de diseño ha solicitado un modo de alto contraste que se pueda activar y desactivar.

La accesibilidad es fundamental, así que quieres implementar esta funcionalidad cuanto antes. Para generarla, utilizarás un agente personalizado.

En este ejercicio:

- revisarás un agente personalizado de accesibilidad existente.
- utilizarás el agente de accesibilidad en Copilot Chat para implementar un modo de alto contraste.

## Revisa el agente personalizado de accesibilidad

Ya se ha creado un agente personalizado para accesibilidad. Revisemos su contenido para entender cómo orientará a Copilot.

Vuelve al codespace, abre un terminal y crea una rama nueva a partir de `main` para el trabajo de accesibilidad. Así mantendrás separado el trabajo de filtrado del ejercicio 3:

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abre `.github/agents/accessibility.md`.
2. Observa los campos `name` y `description` de la cabecera YAML.

> [!CAUTION]
> Los campos `name` y `description` de la cabecera son obligatorios para los agentes personalizados.

3. Examina las secciones siguientes, que describen:
   - las responsabilidades principales al generar código para un sitio web accesible.
   - las prácticas recomendadas de accesibilidad.
   - ejemplos de código HTML, CSS y JavaScript.
   - errores habituales y cómo evitarlos.

## Utiliza el agente personalizado en Copilot Chat

VS Code muestra todos los agentes personalizados definidos en `.github/agents` en el selector de agentes, situado en la parte inferior de la vista Copilot Chat. Puedes seleccionar un agente personalizado para limitar una sesión de chat a sus instrucciones y herramientas.

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve al codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación limpia. El modo y la selección del modelo varían según el ejercicio; cada paso lo indica cuando es pertinente.

1. Si aún no está seleccionado, elige **Agent** en el selector de agentes de la vista Chat.

   ![Captura de pantalla del selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Abre el selector de agentes situado en la parte inferior de la vista Chat. Muestra el agente activo, que de forma predeterminada es **default**.
3. Selecciona **Accessibility agent** en la lista de agentes disponibles.
4. Envía este prompt al agente de accesibilidad:

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat empezará a trabajar: explorará el código base, propondrá cambios y aplicará ediciones a los archivos del proyecto. Cada cambio aparecerá en el chat, junto con la ruta del archivo y una diferencia para que puedas revisarlo.
6. A medida que se apliquen cambios, se actualizará el indicador **Files changed** para mostrar el conjunto de archivos modificados por el agente.

> [!NOTE]
> Este proceso probablemente tardará unos minutos. Copilot hará cambios reales en el repositorio y editará archivos existentes, como componentes Astro, CSS y pruebas relacionadas.

En el siguiente ejercicio revisarás y guiarás este trabajo mientras sigue en curso.

## Resumen y pasos siguientes

En esta lección has explorado los [agentes personalizados][custom-agents] de GitHub Copilot, asistentes especializados en tareas y dominios concretos. Con ellos, puedes plasmar la experiencia y los estándares del equipo en agentes reutilizables que orienten a Copilot.

Has explorado estos conceptos:

- cómo se definen los agentes personalizados.
- cómo utilizar un agente personalizado en el modo agente de Copilot Chat.

A continuación, [supervisarás y guiarás el trabajo del agente][next-lesson]: revisarás los cambios mientras se realizan y añadirás a la misma sesión una opción para el modo claro.

## Recursos

- [Información sobre los agentes personalizados][custom-agents]
- [Crear agentes personalizados en el IDE][creating-custom-agents-ide]
- [Agentes personalizados en VS Code][custom-agents-vscode]
- [Configuración de agentes personalizados][custom-agents-config]
- [Agentes personalizados en awesome-copilot][awesome-copilot-agents]

---

| [← Lección anterior: Prueba la funcionalidad con el servidor MCP de Playwright][previous-lesson] | [Siguiente lección: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents

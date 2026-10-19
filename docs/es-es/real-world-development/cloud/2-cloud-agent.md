---
title: "Ejercicio 2 - Agente de GitHub Copilot en la nube"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que haya muy pocas organizaciones, si es que hay alguna, que no tengan problemas con la deuda técnica. Puede tratarse de problemas de seguridad sin resolver, código heredado que necesita actualizarse o solicitudes de funcionalidades que llevan tiempo en la lista de trabajo porque no ha habido ocasión de implementarlas. El agente de GitHub Copilot en la nube está diseñado para realizar tareas como actualizar código y añadir funcionalidades de forma autónoma. Cuando termina el trabajo, genera una solicitud de incorporación de cambios en borrador para que la revise un desarrollador. Así puedes delegar tareas tediosas, acelerar el desarrollo y centrarte en cuestiones de mayor alcance.

En este ejercicio explorarás lo siguiente con el agente de GitHub Copilot en la nube:

- personalizar el entorno de generación de código.
- garantizar que las operaciones se realicen de forma segura.
- comprender la importancia de delimitar bien las incidencias.
- asignar incidencias a Copilot.

## Escenarios

Tailspin Toys quiere reducir su deuda técnica. Los contratistas que crearon la primera versión del sitio dejaron la documentación en un estado poco satisfactorio: comprobarás que no hay documentación. Como primer paso, quieren añadir comentarios TSDoc a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere mejorar la forma de descubrir juegos. En la página de detalles de cada juego, quieren mostrar otros títulos de la misma categoría para que la gente pueda seguir explorando. Todavía no necesitan una implementación pulida; quieren algo que les permita probar la experiencia de usuario. Aunque esta tarea es un bloqueo, hay otras incidencias que tienen más prioridad por ahora.

Estas son tareas que pueden quedar relegadas con facilidad y que son buenas candidatas para asignarlas al agente de GitHub Copilot en la nube. El agente podrá trabajar en ellas de forma asíncrona mientras tú te ocupas de otras tareas; después, podrás volver para revisar su trabajo.

## Presentación del agente de GitHub Copilot en la nube

El [agente de GitHub Copilot en la nube](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede realizar tareas en segundo plano de forma parecida a un desarrollador. Al igual que cuando trabajas con otra persona, puedes colaborar de distintas maneras, por ejemplo, [asignándole una incidencia de GitHub a Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Cuando se le asigna una incidencia, Copilot crea una solicitud de incorporación de cambios en borrador para registrar su progreso, configura un entorno y empieza a trabajar. Puedes consultar la sesión de Copilot mientras sigue en curso o cuando haya terminado. Cuando la solución propuesta esté lista para que la revises, Copilot te mencionará en la solicitud.

## La importancia de delimitar bien las instrucciones

GitHub Copilot no hace magia: no puedes dejar de pensar. Incluso las tareas que parecen sencillas se vuelven complejas al analizarlas en detalle. Por eso, [delimita bien las tareas que asignas al agente de GitHub Copilot en la nube][cloud-agent-best-practices]. Trátalo como a un compañero de programación con IA: trabaja por etapas, aprende, experimenta y adapta el enfoque. Los fundamentos del desarrollo de software no cambian por añadir IA generativa.

## Instrucciones personalizadas en este repositorio

En ejercicios anteriores has visto cómo guían a Copilot las instrucciones personalizadas. Si ya has completado un ejercicio práctico sobre ellas, habrás visto `.github/copilot-instructions.md` en acción; si no, este es un buen momento para echarle un vistazo. Antes de asignar trabajo al agente en la nube, revisa sin editar los archivos de instrucciones incluidos en el repositorio para poder identificar su efecto más adelante.

Abre los archivos siguientes en la interfaz web de GitHub o en un codespace si ya tienes uno abierto:

- `.github/copilot-instructions.md`: revisa la sección **Code standards**, sobre todo las convenciones de TypeScript y los requisitos para los comentarios TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observa el campo `applyTo` de la cabecera, que limita estas instrucciones a los archivos `**/*.test.ts`.

Cuando asignes al agente en la nube la incidencia `Code lacks documentation` en la siguiente sección, fíjate en los comentarios TSDoc, las convenciones de TypeScript y las cabeceras de comentarios de la solicitud de incorporación de cambios resultante. Esos elementos proceden de los archivos de instrucciones que acabas de revisar. Volveremos a señalarlos al revisar las solicitudes en un ejercicio posterior.

## Configura el entorno de desarrollo del agente de GitHub Copilot en la nube

El desarrollo suele requerir configurar un entorno concreto y ejecutar scripts para instalar las herramientas y dependencias necesarias, independientemente de quién realice el trabajo. Lo mismo ocurre cuando asignas tareas a Copilot, que trabaja de forma parecida a un ingeniero de software.

El agente en la nube utiliza [GitHub Actions][github-actions] como entorno de trabajo. Puedes personalizarlo con un [flujo de trabajo de configuración especial][setup-workflow], definido en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que el agente empiece a trabajar. Así puede acceder a las herramientas y dependencias necesarias. Este flujo se ha configurado con antelación para facilitar el taller y permitirte aprender. Proporciona a Copilot Node.js, las dependencias del proyecto, los binarios del navegador para las pruebas de extremo a extremo y la base de datos SQLite migrada y con datos iniciales para la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña "Actions" del repositorio
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Concede solo los permisos mínimos necesarios para estos pasos. Copilot recibe su propio token para sus operaciones.
    permissions:
      # Si los pasos de configuración clonan el repositorio, por ejemplo, para instalar dependencias, necesitarán el permiso `contents: read`. Si no lo clonan, Copilot lo hará automáticamente cuando terminen los pasos.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración de la aplicación: Node.js (la aplicación completa ahora es Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra y carga la base de datos SQLite local para que las compilaciones y pruebas dispongan de datos.
      - name: Set up the database
        run: npm run db:setup
```

El archivo se parece a cualquier otro flujo de trabajo de GitHub, pero tiene algunos detalles importantes:

- Incluye un único trabajo, `copilot-setup-steps`, que se ejecuta en GitHub Actions antes de que Copilot empiece a trabajar en la solicitud de incorporación de cambios.
- El activador `workflow_dispatch` permite ejecutar manualmente el flujo desde la pestaña **Actions** del repositorio. Resulta útil para comprobar que funciona sin esperar a que Copilot lo ejecute.

## Añade documentación

Aunque todo el mundo reconoce la importancia de la documentación, muchos proyectos tienen información obsoleta o carecen de ella. Esta deuda técnica puede reducir la productividad, complicar el mantenimiento del código y dificultar la incorporación de nuevos desarrolladores. Copilot destaca en la creación de documentación, así que esta es una incidencia ideal para asignar al agente en la nube. Trabajará en segundo plano para generar la documentación necesaria. En un ejercicio posterior volverás para revisar el resultado.

1. Abre tu repositorio en github.com en una pestaña nueva del navegador.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de creación.
4. Selecciona **Blank issue**.
5. Escribe `Code lacks documentation` en el campo **Title**.
6. Escribe lo siguiente en el campo **Description**:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de asignación.

    ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.
10. Selecciona la pestaña **Pull Requests**.
11. Abre la nueva solicitud de incorporación de cambios, que tendrá un título parecido a `[WIP]: Code lacks documentation`. Si no aparece, espera uno o dos minutos y actualiza la página.
12. Al cabo de unos minutos, Copilot debería haber creado una lista de tareas.

> [!NOTE]
> La lista de tareas puede tardar varios minutos en aparecer en la solicitud de incorporación de cambios. Copilot está creando su entorno —ejecutando el flujo de trabajo anterior—, analizando el proyecto y determinando cómo abordar el problema.

13. Revisa la lista y las tareas que va a completar.
14. Desplázate hacia abajo por la cronología de la solicitud. Deberías ver un aviso de que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona el botón **View session**.

    ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Puede que tengas que actualizar la ventana para ver el indicador actualizado.

16. Desplázate por la sesión en directo y observa cómo Copilot resuelve el problema: explora el código, analiza el estado, se detiene a pensar y decidir el plan adecuado y crea código.

Es probable que esto tarde varios minutos. Una de las principales ventajas del agente de GitHub Copilot en la nube es que puede trabajar de forma asíncrona y dejarte tiempo para otras tareas. Aprovecharemos esta característica: asignaremos otra tarea al agente y, mientras tanto, añadiremos funcionalidades a la aplicación.

## Añade una sección de juegos relacionados a la página de detalles

Como ya se ha explicado, una gran ventaja del agente de GitHub Copilot en la nube es que permite repartir el trabajo para que tú puedas centrarte en unas tareas mientras el agente se ocupa de otras. Aunque añadir una sección de juegos relacionados para el equipo de diseño no lleve mucho tiempo, podrías aprovecharlo en otra tarea. ¡Asígnasela al agente!

1. Vuelve a tu repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de creación.
4. Selecciona **Blank issue**.
5. Escribe `Show related games on the game details page` en el campo **Title**.
6. Escribe lo siguiente en el campo **Description**:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de asignación.

    ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Poco después, verás un símbolo 👀 en el primer comentario de la incidencia. Indica que Copilot ya está trabajando.

![Copilot utiliza el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en tu segunda solicitud. Como el agente trabaja de forma parecida a un ingeniero de software, no hace falta que supervises activamente el proceso; puedes revisar el resultado cuando termine. Ahora, centrémonos en crear y utilizar agentes personalizados.

## Resumen y pasos siguientes

En esta lección has explorado el [agente de GitHub Copilot en la nube][copilot-agents]. Puedes asignarle incidencias para que trabaje de forma asíncrona. Copilot puede ayudarte a abordar deuda técnica, crear funcionalidades o migrar código de un framework a otro.

Has explorado estos conceptos:

- personalizar el entorno de generación de código.
- garantizar que las operaciones se realicen de forma segura.
- comprender la importancia de delimitar bien las incidencias.
- asignar incidencias a Copilot.

Mientras el agente trabaja en segundo plano, puedes centrarte en crear y utilizar agentes personalizados. [El agente en la nube también puede utilizar servidores MCP][cloud-agent-mcp] y dispone de instrucciones personalizadas, que ya exploramos en módulos anteriores.

## Recursos

- [Información sobre el agente de GitHub Copilot en la nube][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]

| [Siguiente lección: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions

---
title: "Ejercicio 2 - Añadir funcionalidades con el modo agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Hasta las actualizaciones más sencillas de una aplicación suelen requerir cambios en varios archivos y operaciones como ejecutar pruebas. El flujo habitual de desarrollo puede consistir en buscar todos los archivos necesarios, hacer los cambios, ejecutar las pruebas, depurar, averiguar qué archivo se ha pasado por alto, volver a actualizarlo… y así sucesivamente.

El modo agente de Copilot permite simplificar este trabajo.

El modo agente de Copilot está diseñado para actuar de forma más autónoma en el IDE. Se comporta de manera parecida a un desarrollador: primero explora la estructura existente del proyecto, después realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los problemas que encuentra. Veamos cómo utilizar el modo agente para añadir funcionalidades al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo agente y el agente en la nube están diseñados para experiencias distintas. El modo agente trabaja en el IDE para ofrecer ciclos de respuesta e interacción rápidos. El agente en la nube está pensado para actuar como otro miembro del equipo: trabaja de forma asíncrona e interactúa contigo mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás cómo:

- el modo agente de Copilot puede explorar el proyecto, identificar archivos pertinentes y coordinar los cambios.
- el modo agente de GitHub Copilot puede implementar funcionalidades en la interfaz y la capa de datos.
- revisar los cambios y las pruebas generados por el modo agente de Copilot antes de combinar el código en el repositorio.

## Escenario

Al crecer la lista de juegos, quieres que los usuarios puedan filtrarlos por categoría y editorial. En el ejercicio anterior ya añadiste una función auxiliar para las editoriales. Ahora completarás con el modo agente de Copilot el trabajo restante en la capa de datos, la interfaz y las pruebas.

## Ejecuta el sitio web de Tailspin Toys

Antes de hacer cambios, exploremos el sitio web de Tailspin Toys para conocer sus funcionalidades actuales.

El sitio es una plataforma de financiación colectiva de juegos de mesa con temática de desarrollo de software. Permite consultar una lista de juegos y ver sus detalles. Es una única aplicación Astro que genera las páginas como HTML estático durante la compilación. Las páginas consultan una base de datos SQLite local directamente desde su frontmatter mediante Drizzle ORM; no hay una API de backend independiente ni un framework de interfaz de cliente. Las funciones auxiliares reutilizables de acceso a datos están en `src/lib/`, y la interactividad se implementa con un pequeño `<script>` de Astro delimitado que utiliza las API estándar del DOM.

### Inicia el sitio web

Para facilitar la ejecución del sitio, se ha proporcionado una tarea `npm run dev` que inicia el único servidor de desarrollo Astro. Puedes ejecutarla en GitHub Codespaces así:

1. Vuelve al codespace. Después del ejercicio anterior, deberías estar de nuevo en `main`.
2. Abre un terminal nuevo en el Codespace con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crea una rama nueva para el trabajo de filtrado y cámbiate a ella:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Ejecuta el siguiente comando para iniciar el sitio:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo Astro esté listo, aparecerá un mensaje con una URL parecida a esta:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo que te pregunta si quieres abrir una ventana del navegador para `http://localhost:4321`, ciérralo seleccionando la **x**.

5. Abre el sitio manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando la dirección del servidor Astro `http://localhost:4321` en el terminal.

> [!NOTE]
> En un codespace, al seleccionar el enlace a la URL localhost desde el terminal, se te redirigirá automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Se trata de un túnel privado a tu codespace, que ahora aloja el servidor web.

### Explora el sitio web

Cuando el sitio esté en marcha, podrás explorar sus funcionalidades. Entre ellas:

- **Página de inicio**: muestra una lista de juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre una página con más información, como el título, la descripción, la editorial y la categoría.

## Explora la lista de trabajo con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve al codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación limpia. El modo y la selección del modelo varían según el ejercicio; cada paso lo indica cuando es pertinente.

La implementación inicial del sitio funciona, pero queremos añadir nuevas funcionalidades. Empecemos por revisar la lista de trabajo. Al crear el proyecto a partir de la plantilla, se generó automáticamente una lista de incidencias; pide a GitHub Copilot que te muestre sus elementos.

1. Selecciona **Agent** en el selector de agentes de la vista Chat. El agente **Agent** planifica e implementa cambios de forma autónoma en varios archivos, ejecuta comandos de terminal e invoca herramientas.

   ![Captura de pantalla del selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores de este taller no recomiendan un modelo en particular. Al crear el taller se utilizó Claude Sonnet 4.5 para que las sugerencias de código fueran relativamente coherentes y la experiencia resultara fluida. Sin embargo, como los LLM son probabilísticos, las sugerencias pueden diferir de las indicadas en el taller. Es normal y previsible.

> [!NOTE]
> Debido a la naturaleza probabilística de los LLM, Copilot podría utilizar un comando MCP distinto, pero debería poder completar la tarea.

3. Pide a Copilot que muestre las incidencias pendientes con este prompt:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para ejecutar el comando que enumera todas las incidencias.
5. Revisa la lista generada.

Observa que Copilot incluso ha priorizado los elementos según cuáles considera más útiles para los usuarios.

## Revisa los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que aportará contexto a su trabajo. Utilizarás el archivo de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo abordar la incorporación de funcionalidades al sitio.

1. En el codespace, abre `.github/instructions/ui.instructions.md`.
2. Fíjate en las directrices generales para añadir funcionalidades, entre ellas:
   - una descripción general de la arquitectura.
   - principios de diseño de componentes, capacidad de prueba y accesibilidad.
   - enlaces a archivos de instrucciones específicos para distintos tipos de archivo, como:
     - Astro.
     - Tailwind CSS.
     - Drizzle.

> [!TIP]
> Los archivos de instrucciones permiten hacer referencia a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas al archivo de instrucciones. Así se pueden reutilizar directrices, dividir instrucciones complejas en partes más manejables y proporcionar ejemplos y plantillas.

## Implementa la funcionalidad de filtrado

Para completar el filtrado, habrá que realizar al menos tres cambios distintos en la aplicación:

- añadir o perfeccionar la lógica restante de filtrado en la capa de datos (`src/lib/`).
- añadir o actualizar pruebas del comportamiento de filtrado.
- actualizar la página de lista de juegos para incluir la interfaz de filtrado.

Además, las pruebas deben ejecutarse y superarse antes de combinar los cambios en el código base. El modo agente de Copilot puede encargarse de ello. Añadamos la funcionalidad.

1. Puedes continuar la conversación actual con Copilot o iniciar otra seleccionando **New Chat**.
2. Selecciona **Add Context**, **Instructions** y **ui** como archivo de instrucciones.

   ![Captura de pantalla que muestra cómo seleccionar el archivo de instrucciones de la interfaz de usuario](../../../_images/ex3-select-instructions-file.png)

3. Confirma que **Agent** sigue seleccionado en el selector de agentes de la vista Chat.

   ![Captura de pantalla del selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Confirma que **Claude Sonnet 4.5** sigue seleccionado como modelo.
5. Pide a Copilot que implemente la funcionalidad según la incidencia relacionada de la lista de trabajo:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos relacionados con la funcionalidad. Debería encontrar las funciones auxiliares de la capa de datos, la interfaz y las pruebas. Después, empezará a modificar los archivos y ejecutar las pruebas.

   ![Captura de pantalla de Copilot explorando los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas, como explorar el proyecto, modificar archivos y ejecutar pruebas. El proceso puede tardar unos minutos, según la complejidad de la tarea y del código base. Mientras trabaja, quizá aparezcan los botones **Keep** y **Undo** en el editor. Cuando termine, podrás aceptar o deshacer todos los cambios; no hace falta seleccionarlos mientras el agente sigue trabajando.

7. Cuando Copilot te lo pida, selecciona **Continue** para ejecutar las pruebas.

   ![Captura de pantalla del cuadro de diálogo en Copilot Chat que pide confirmar la ejecución de las pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Es posible que haya pausas e incluso que algunas pruebas fallen durante el proceso. No pasa nada: Copilot alterna entre la generación de código y las pruebas hasta completar la tarea y no detectar errores.

   ![Captura de pantalla de una sesión completa de Copilot Chat en modo agente](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explora el código generado para detectar posibles problemas.

> [!CAUTION]
> Recuerda que siempre es importante revisar el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador con el sitio en marcha y explora la nueva funcionalidad.
11. Cuando hayas confirmado que todo funciona y revisado el código, selecciona **Keep** en la ventana de Copilot Chat.

## Resumen y pasos siguientes

¡Enhorabuena! En este ejercicio has aprendido a utilizar el modo agente de GitHub Copilot para añadir funcionalidades al sitio web de Tailspin Toys. Has aprendido cómo:

- el modo agente de GitHub Copilot puede implementar funcionalidades nuevas en la interfaz y la capa de datos.
- el modo agente de Copilot puede explorar el proyecto, identificar archivos pertinentes y coordinar los cambios.
- revisar los cambios y las pruebas generados por el modo agente de Copilot antes de combinar el código en el repositorio.

Ahora [probaremos la funcionalidad con el servidor MCP de Playwright][next-lesson] y abriremos una solicitud de incorporación de cambios.

### Ejercicio adicional: implementa la paginación

Al crecer la lista de juegos, será necesario habilitar la paginación. Utiliza lo aprendido en este ejercicio y pide a Copilot que añada paginación al sitio. Algunos aspectos que debes tener en cuenta:

- sigue las prácticas recomendadas, incluidos los archivos de instrucciones existentes.
- decide cómo quieres implementar la paginación: por ejemplo, si el usuario puede seleccionar el tamaño de página o si este debe ser fijo.
- al crear el prompt, proporciona a Copilot las directrices necesarias para conseguir la implementación que quieres.
- quizá tengas que iterar con GitHub Copilot y pedir cambios o proporcionar contexto. Este es el flujo habitual de trabajo con Copilot.

## Recursos

- [Modos ask, edit y agent de Copilot: qué hacen y cuándo utilizarlos][choose-mode]
- [Modo agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode

---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los últimos ejercicios has pedido al agente de GitHub Copilot en la nube que se ocupe de tres tareas distintas para mejorar la experiencia de usuario y añadir funcionalidades. Aunque el agente está diseñado para trabajar de forma autónoma y asíncrona, sigue siendo importante poder supervisar esas tareas.

Hay varias herramientas para gestionar las tareas asignadas al agente en la nube, como la [página de agentes][agents-page] de GitHub.com. Desde este centro de control puedes ver todas las tareas de los agentes que tienen solicitudes de incorporación de cambios abiertas. También puedes explorar las operaciones que han realizado y guiar una sesión en curso.

En esta lección:

- explorarás la página de agentes para supervisar las tareas del agente en la nube.
- guiarás una sesión en curso para solicitar una funcionalidad adicional.

## Escenario

Después de asignar al agente la creación de un modo de alto contraste, el equipo se dio cuenta de que también sería buen momento para añadir un modo claro. Como ya se iba a actualizar el estilo del sitio y añadir una opción para cambiar de modo, parecía lógico incluir esta funcionalidad. Guiarás el trabajo del agente para que añada también el modo claro.

## Revisa las tareas del agente de GitHub Copilot en la nube

Consulta el estado actual de todas las tareas asignadas al agente de GitHub Copilot en la nube.

1. Abre la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa la lista de tareas, tanto en el panel principal como en el panel izquierdo. Deberían aparecer las tareas asignadas a Copilot, entre ellas:
   - actualizar la documentación del código base.
   - generar API para modificar juegos.
   - añadir un modo de alto contraste al sitio web.
3. Selecciona una de las tareas en curso. Revisa las operaciones realizadas por Copilot, que pueden incluir:
   - obtener el código del repositorio.
   - crear el entorno de trabajo de Copilot.
   - configurar servidores MCP.
   - realizar distintos pasos para completar la tarea asignada.

> [!NOTE]
> Los pasos concretos de la lista variarán según el estado del trabajo de Copilot y el enfoque que haya seguido.

4. Observa también el panel de la solicitud de incorporación de cambios que aparece a la derecha. Permite ver la solicitud y los archivos modificados para supervisar el trabajo.

## Guía al agente en la nube

Ahora que has visto las tareas activas, pide a Copilot que añada la opción de modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto dependerá del nombre elegido por Copilot y del estado actual del trabajo.

    ![Sesión de accesibilidad en Mission Control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos, hasta que indique que ha terminado la configuración y ha empezado a trabajar. Lo sabrás cuando empiecen a aparecer mensajes parecidos a los siguientes.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe este prompt:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Captura de pantalla de la tarea del agente en la nube en la página de agentes, con el cuadro de diálogo para guiar la sesión activa mientras Copilot trabaja resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot lo confirma y lo incorpora a su trabajo.

## Deja que Copilot trabaje

Como antes, Copilot empezará a trabajar en la tarea actualizada. Incorporará la nueva solicitud después de terminar el paso concreto que estaba realizando cuando enviaste el mensaje.

Esto también tardará varios minutos, así que puedes aprovechar el momento para hacer un repaso de lo que has aprendido y explorado hasta ahora.

## Resumen y pasos siguientes

En esta lección has explorado la página de agentes de Copilot, el centro de control para supervisar y guiar las tareas del agente de GitHub Copilot en la nube. Desde allí puedes seguir las tareas activas y completadas, revisar el trabajo y redirigir las sesiones en curso para ajustar su alcance o proporcionar indicaciones adicionales.

Has explorado estos conceptos:

- utilizar Mission Control y la página de agentes para supervisar las tareas del agente en la nube.
- redirigir una sesión en curso para pedir funcionalidades adicionales.

Mientras Copilot termina el trabajo de accesibilidad, puedes [iterar sobre las solicitudes de incorporación de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Información sobre los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents

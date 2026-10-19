---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Gestión de agentes][previous-lesson] |
|:--|

## Revisa el trabajo

A lo largo de este taller has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funcionalidades. Le pediste que documentara el código, creara una funcionalidad de juegos relacionados para que el equipo de diseño pudiera probarla e implementara mejoras de accesibilidad, incluidos los modos de alto contraste y claro. Revisemos los cambios de código y, si hace falta, demos indicaciones a Copilot para mejorar el resultado.

### Escenario

Como ya hemos señalado varias veces, los fundamentos del diseño de software y DevOps no cambian al añadir IA generativa. Siempre hay que revisar el código generado y seguir el proceso habitual de DevOps. Revisaremos las propuestas de GitHub Copilot para crear documentación, añadir juegos relacionados e implementar mejoras de accesibilidad antes de habilitar las revisiones para el resto del equipo.

## Seguridad y agente de GitHub Copilot en la nube

Como el agente en la nube trabaja de forma asíncrona y sin supervisión, se han establecido ciertas restricciones de seguridad:

- Copilot solo tiene acceso de lectura al repositorio y de escritura **únicamente** en la rama que utiliza para trabajar.
- El agente en la nube se ejecuta en GitHub Actions, donde crea un entorno independiente y efímero.
- Una persona debe aprobar los flujos de trabajo de GitHub Actions antes de que puedan ejecutarse.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores MCP.

## Revisa la documentación generada

Empecemos por la primera solicitud de incorporación de cambios generada por el agente de GitHub Copilot en la nube: añadir documentación al código. Para ello, utilizarás la interfaz habitual de solicitudes de incorporación de cambios de GitHub.com.

> [!NOTE]
> Al explorar la solicitud, quizá veas un aviso de que el cortafuegos bloquea a GitHub Copilot. Es **lo esperado**, ya que el acceso de Copilot a recursos externos está limitado de forma predeterminada, incluidas las llamadas a servidores MCP externos. Si quieres, puedes [personalizar o deshabilitar el cortafuegos del agente en la nube][agent-firewall].

1. Vuelve a tu repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de solicitudes.
3. Abre la solicitud titulada, por ejemplo, **Add missing documentation** o con una descripción más específica.

> [!NOTE]
> Si Copilot sigue trabajando en la tarea, la solicitud incluirá la marca **[WIP]**. En ese caso, espera a que termine. Puede tardar unos minutos; mientras tanto, puedes descansar o repasar lo que has aprendido.

4. Cuando la solicitud esté lista, selecciona la pestaña **Files changed** y revisa los cambios.

    ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, que incluye comentarios TSDoc y otros elementos de documentación. Los cambios exactos variarán.

   Al revisar los cambios, busca comentarios TSDoc, convenciones de TypeScript y cabeceras de comentarios. Proceden de los archivos de instrucciones personalizadas que revisaste al principio del ejercicio 2.

6. Cuando hayas revisado los cambios y te parezcan correctos, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Debería aparecer un indicador de que algunos flujos de trabajo están pendientes de aprobación.
8. Si hay flujos pendientes, selecciona **Approve and run workflows**.

    ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la solicitud. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro única deberían superarse. El proceso puede tardar unos minutos.

## Solicita cambios a GitHub Copilot

La colaboración con Copilot en una solicitud de incorporación de cambios no tiene por qué ser unidireccional. Puedes mencionar a Copilot en comentarios de la solicitud o en comentarios asociados a líneas de código, igual que harías con cualquier integrante del equipo. Copilot leerá esos comentarios e iniciará otra sesión para resolverlos. Como los resultados son no deterministas, no hay un texto de solicitud prescriptivo. Por ejemplo, puedes pedirle que:

- añada una breve cabecera de comentario al principio de cada archivo de código para describir su función.
- añada comentarios TSDoc a los archivos TypeScript y Astro.
- cree un README que describa la estructura de la aplicación Astro.

1. Añade un comentario para solicitar un cambio en la documentación generada y menciona a **@copilot**, como harías con cualquier usuario. Utiliza una de las ideas anteriores o pide otro cambio de documentación que quieras ver en el código base.
2. Selecciona **View Session** para observar el trabajo de Copilot. Verás que inicia una sesión nueva para aplicar los cambios.
3. Selecciona **Back to pull request** para volver a la solicitud.

    ![Volver a la solicitud de incorporación de cambios](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine los cambios, aparecerá una confirmación nueva en la solicitud.
5. Selecciona la pestaña **Files changed** para revisar los cambios.

Puedes seguir iterando hasta que estés conforme. Después, puedes marcar la solicitud como lista para revisión y combinarla.

![Marcar la solicitud como lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisa la funcionalidad de juegos relacionados

Volvamos a la solicitud que Copilot generó para mostrar juegos relacionados en la página de detalles de un juego.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la solicitud cuyo título sea parecido a **Show related games on the game details page**.
4. Selecciona **Files changed** para revisar el código generado.
5. Cuando hayas revisado los cambios, vuelve a **Conversation** y desplázate hacia abajo.
6. Debería aparecer un indicador de que algunos flujos de trabajo están pendientes de aprobación.
7. Si hay flujos pendientes, selecciona **Approve and run workflows**.

    ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

8. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la solicitud. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro única deberían superarse. El proceso puede tardar unos minutos.
9. **Opcional:** puedes cambiar a esta rama en tu codespace para probar manualmente la funcionalidad de juegos relacionados. Abre el terminal del codespace y ejecuta los comandos siguientes. Sustituye `<branch-name>` por el nombre de la rama creada por Copilot, por ejemplo, **copilot/fix-8**:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot ha creado la funcionalidad de juegos relacionados. Como antes, puedes iterar con el agente en la nube y pedirle cambios, por ejemplo, que ajuste el número de juegos relacionados o añada cabeceras de comentario y comentarios TSDoc. Recuerda que esta tarea se asignó **antes** de actualizar las instrucciones personalizadas. Puedes solicitar cambios dejando un comentario nuevo en la pestaña **Conversation**; Copilot lo leerá e iniciará otra sesión.

## Revisa las funcionalidades de accesibilidad

Por último, revisemos las funcionalidades de accesibilidad implementadas con el agente personalizado. Esta solicitud debería incluir el modo de alto contraste asignado en el ejercicio 3 y el modo claro solicitado en Mission Control en el ejercicio 4.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la solicitud cuyo título sea parecido a **Add high contrast mode to website**.

> [!NOTE]
> Si Copilot sigue trabajando en la tarea, la solicitud incluirá la marca **[WIP]**. En ese caso, espera a que termine. Puede tardar unos minutos.

4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Revisa la implementación y presta especial atención a:
   - los componentes de interfaz para cambiar de modo.
   - el uso del almacenamiento local para conservar las preferencias.
   - los cambios de CSS o estilos para los modos de alto contraste y claro.
   - los atributos de accesibilidad, como las etiquetas ARIA y la navegación con teclado.
   - el código JavaScript/TypeScript que gestiona el cambio de modo.

6. Cuando hayas revisado los cambios, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Debería aparecer un indicador de que algunos flujos de trabajo están pendientes de aprobación.
8. Si hay flujos pendientes, selecciona **Approve and run workflows**.

    ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la solicitud. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro única deberían superarse. El proceso puede tardar unos minutos.
10. **Opcional:** puedes cambiar a esta rama en tu codespace para probar manualmente las funcionalidades de accesibilidad. Abre el terminal del codespace y ejecuta los comandos siguientes. Sustituye `<branch-name>` por el nombre de la rama creada por Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    A continuación, inicia la aplicación y prueba los modos de alto contraste y claro en el navegador. Comprueba que funcionan y que las preferencias se conservan después de volver a cargar la página.

Observa cómo el agente personalizado de accesibilidad ha ayudado a Copilot a implementar estas funcionalidades siguiendo las prácticas recomendadas de accesibilidad. Si detectas algún problema o una posible mejora, puedes mencionar a **@copilot** en un comentario para solicitar cambios, como en las solicitudes anteriores.

## Ejercicio opcional: sigue delegando

El agente en la nube funciona mejor cuando puedes asignarle elementos reales de la lista de trabajo y ocuparte de otras tareas. Para adquirir ese hábito, registra otras incidencias en el repositorio y asígnaselas a Copilot. Algunas ideas:

- crear un formulario para mostrar el interés de quienes apoyan un juego en la página de detalles.
- implementar la paginación en la página de lista de juegos.
- añadir validación de entradas y gestión de errores a las funciones auxiliares de acceso a datos.

## Resumen

Has completado el recorrido del agente en la nube. A lo largo de estas lecciones:

- **Has inspeccionado los archivos de instrucciones personalizadas incluidos en el repositorio** para observar su efecto en el trabajo del agente.
- **Has asignado incidencias al agente de GitHub Copilot en la nube** y has visto cómo prepara el entorno, planifica y trabaja de forma asíncrona.
- **Has creado y utilizado un agente personalizado** para implementar modos de alto contraste y claro.
- **Has utilizado la página de agentes de Copilot como centro de control** para supervisar y guiar la sesión de accesibilidad en curso.
- **Has revisado e iterado sobre las solicitudes de incorporación de cambios del agente**, mencionando a `@copilot` para pedir cambios y aprobando flujos de trabajo.

## Repaso y pasos siguientes

Has completado el recorrido del agente en la nube. Si quieres seguir explorando, los otros recorridos amplían lo que has practicado:

- 🖥️ **[Recorrido de VS Code](../../vscode/)**: explora el modo de agente de Copilot Chat y la integración de MCP directamente desde el IDE.
- 💻 **[Recorrido de CLI](../../cli/)**: entrega dos cambios revisados desde el terminal con los modos Plan y Autopilot, habilidades de agente, un agente personalizado de control de calidad, Playwright MCP, Agent Merge y un puente opcional `/delegate` al agente en la nube.

En tu propio repositorio, prueba estas ideas:

- asignar una incidencia de refactorización o cobertura de pruebas al agente en la nube.
- crear un agente personalizado para otro dominio.
- configurar `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Información sobre los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]
- [Configurar el cortafuegos del agente en la nube][agent-firewall]

---

| [← Lección anterior: Gestión de agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/

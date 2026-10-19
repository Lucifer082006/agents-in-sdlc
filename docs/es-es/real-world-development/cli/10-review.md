---
title: "Lección 10 - Repaso y pasos siguientes"
description: "Repasa el flujo de trabajo con Copilot CLI, las dos solicitudes de incorporación de cambios, las personalizaciones reutilizables y otros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Has utilizado GitHub Copilot CLI en un flujo de trabajo continuo para Tailspin Toys. En concreto:

- has preparado un Codespace, instalado Copilot CLI, explorado el proyecto y encontrado la incidencia de filtrado inicial.
- has añadido valoraciones por estrellas, revisado el resultado en un navegador reenviado y combinado manualmente tu primera solicitud de incorporación de cambios (PR).
- has partido de la incidencia de filtrado, definido el enfoque en el modo Plan, implementado la funcionalidad en modo Autopilot y revisado el resultado en modo Interactive.
- has guiado al agente con instrucciones personalizadas, personalizado la habilidad `quality-checks` existente y la has utilizado para ejecutar pruebas unitarias, lint y comprobaciones de tipos.
- has añadido el servidor Model Context Protocol (MCP) de Playwright y lo has utilizado para explorar el filtrado en un navegador real.
- has creado y seleccionado un agente personalizado de control de calidad (QA) para evaluar los requisitos, la cobertura, los resultados de la habilidad y las pruebas en el navegador.
- has revisado el cambio completo del filtrado y autorizado Agent Merge para combinar esa PR.
- has explorado comandos de barra para gestionar el contexto, los modelos, el uso compartido y la delegación opcional a la nube.

## Lo que has publicado

El taller tiene dos hitos de PR, cada uno en su propia rama basada en `main` actualizado:

1. **Valoraciones por estrellas:** muestra `starRating` existente y un estado explícito para los juegos sin valoración en las tarjetas.
2. **Filtrado y flujo de trabajo de calidad:** implementa el filtrado, actualiza las instrucciones y las aplica a la funcionalidad, personaliza el informe de `quality-checks`, crea un perfil de control de calidad e incluye las pruebas correspondientes.

Desde la planificación del filtrado hasta la apertura de su PR, has utilizado la misma conversación y rama. Hemos combinado todo ese trabajo en una sola PR para simplificar el taller.

## Distintos tipos de verificación

Has comprobado el código de varias maneras: con pruebas automatizadas, con tu propia inspección en el navegador y con la exploración del navegador de Copilot mediante MCP. La habilidad `quality-checks` ha ejecutado pruebas unitarias, lint y comprobaciones de tipos, y ha presentado los resultados en el nuevo formato. El agente de control de calidad ha reunido esos resultados con la revisión de los requisitos y de la cobertura de pruebas antes de abrir la PR.

Las pruebas nuevas deben cubrir deficiencias reales; puede ser correcto que una ejecución de control de calidad no necesite añadir pruebas. Revisa el código y las pruebas antes de autorizar la combinación, y actualiza las pruebas de verificación afectadas después de cualquier cambio.

## Prácticas recomendadas

El contexto y las herramientas que proporcionas a Copilot condicionan su trabajo. En este taller, has actualizado instrucciones, personalizado una habilidad, creado un perfil de control de calidad y configurado un servidor MCP. Reutiliza estas personalizaciones en otras conversaciones y adáptalas a las necesidades del equipo. Las instrucciones definen estándares, las habilidades describen tareas repetibles, los agentes personalizados especifican roles especializados y los servidores MCP conectan herramientas externas. Revisa los cambios y los resultados reales de las herramientas, no solo el resumen del agente.

Adapta el **modo y el modelo** a la tarea. Utiliza **Plan** para definir un enfoque antes de empezar a construir, **Interactive** para seguir de cerca los cambios específicos y **Autopilot** para las tareas bien delimitadas. Elige un modelo rápido para cambios rutinarios y uno más capaz para el trabajo complejo.

El contexto sigue siendo tan importante como la infraestructura. Describir con claridad *qué* quieres crear, *por qué* y *cómo* influye de forma significativa en el resultado.

## Más opciones para explorar

Ya has cubierto el flujo de trabajo principal. Aquí tienes otras funciones de CLI que puedes probar:

- `/review` para pedir al agente de revisión de código que analice los cambios.
- `/rubber-duck` para explicar un problema y obtener otra perspectiva.
- `/fleet` para coordinar subtareas independientes en paralelo.
- `/worktree` para aislar otra tarea.
- `/delegate` para enviar una tarea al agente de Copilot en la nube.

## Pasos siguientes

La mejor manera de mejorar con cualquier herramienta es seguir utilizándola. Aplícala al código de producción, a proyectos personales o a esa pequeña aplicación que llevas años queriendo crear. Comparte lo aprendido con el equipo y aprende de sus experiencias. Y, como siempre, explora la documentación.

Si quieres ampliar Tailspin Toys desde el terminal, continúa con la serie opcional [Foundry Backer Concierge][foundry]. Para comparar otros entornos, explora el [taller de VS Code][vscode], el [taller de la aplicación GitHub Copilot][app] o el [taller del agente de Copilot en la nube][cloud].

## Recursos

- [Información sobre GitHub Copilot CLI][about-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]
- [Personalizar Copilot CLI][customize-cli]
- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests

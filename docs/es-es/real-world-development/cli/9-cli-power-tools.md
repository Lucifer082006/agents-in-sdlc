---
title: "Lección 9 - Comandos de barra en GitHub Copilot CLI"
description: "Explora comandos de barra para gestionar el contexto, elegir modelos, compartir sesiones y delegar tareas opcionalmente a la nube."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como cualquier buena herramienta de CLI, GitHub Copilot CLI incluye muchos comandos de barra para interactuar con ella. Estos comandos ofrecen funciones avanzadas, información interna y opciones de configuración adicionales. Ya has utilizado comandos como `/diff`, `/mcp`, `/skills`, `/agent` y `/pr`. Exploremos otros comandos útiles.

En esta lección:

- utilizarás `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de la conversación.
- utilizarás `/model` para explorar los modelos disponibles.
- aprenderás cómo `/share` puede exportar o compartir una sesión.
- explorarás comandos opcionales para trabajar en paralelo, utilizar worktrees y delegar tareas al agente en la nube.

## Escenario

Has completado el flujo de trabajo principal de CLI. Ahora explorarás otras funciones: gestionar el contexto, cambiar de modelo, compartir sesiones y, de forma opcional, delegar trabajo al [agente de Copilot en la nube][about-cloud-agent].

## Explora el contexto de Copilot CLI

Al trabajar en tareas extensas o complejas, puedes alcanzar el tamaño máximo de contexto del modelo. Copilot CLI compacta automáticamente la conversación cuando hace falta y permite inspeccionarla o compactarla manualmente con comandos de barra.

1. Vuelve al Codespace e inicia Copilot CLI desde la raíz del repositorio, si aún no está abierto.
2. Escribe:

   ```plaintext
   /context
   ```

3. Observa el modelo, el uso actual de tokens y la distribución del contexto entre las instrucciones del sistema, las herramientas, los mensajes y el espacio libre.
4. Compacta la conversación:

   ```plaintext
   /compact
   ```

5. Vuelve a escribir `/context` y compara el resultado. Si la conversación ya era breve, puede que no haya cambiado mucho.

> [!NOTE]
> Copilot CLI compacta el contexto automáticamente cuando se llena la ventana. Utiliza `/compact` si quieres elegir cuándo hacerlo. Utiliza `/clear` o `/new` cuando cambies a una tarea distinta y quieras empezar una conversación nueva.

## Elige un modelo

Los distintos modelos tienen capacidades diferentes y cada desarrollador puede tener sus propias preferencias. Copilot CLI permite ver la lista de modelos y seleccionar el que quieras utilizar.

1. Escribe:

   ```plaintext
   /model
   ```

2. Explora los modelos disponibles y la información de uso.
3. Mantén el modelo actual, elige otro o pulsa <kbd>Esc</kbd> para cerrar la lista.

## Comparte una sesión

Colaborar en equipo y compartir lo aprendido ayuda a mejorar el uso de las herramientas de IA. El comando `/share` puede exportar una sesión a un archivo Markdown o HTML, crear un enlace para compartir o publicar un gist de GitHub.

1. Escribe `/help` y revisa las opciones de `/share` disponibles en tu versión.
2. Si quieres compartir la sesión, elige un destino adecuado, como `/share file` para exportar un archivo Markdown local.
3. Revisa el contenido exportado antes de enviarlo a otras personas o publicarlo. Las exportaciones pueden incluir prompts, respuestas y detalles del proyecto.

Publicar un enlace o un gist es opcional. No publiques contenido del repositorio o de la conversación que el equipo no tenga previsto compartir.

## Opcional: escala o delega

Has completado el taller principal. Copilot CLI también ofrece comandos para tareas de mayor alcance:

- `/fleet` puede distribuir subtareas independientes entre subagentes y ejecutarlas en paralelo.
- `/worktree` puede crear un worktree de Git aislado para otra tarea.
- `/delegate` puede enviar una tarea al agente de Copilot en la nube, que trabaja de forma asíncrona y puede abrir una solicitud de incorporación de cambios.

Estos comandos son opcionales, porque pueden crear worktrees adicionales o trabajo remoto. Antes de probarlos, empieza una tarea nueva y bien delimitada y revisa el resultado con el flujo de trabajo habitual. Si quieres profundizar en el trabajo asíncrono de los agentes, continúa con el [taller del agente en la nube][cloud-workshop].

## Resumen y pasos siguientes

Los comandos de barra de Copilot CLI te permiten configurarlo, compartir sesiones y ver lo que ocurre en segundo plano. En esta lección:

- has utilizado `/context` y `/compact` para explorar cómo gestiona Copilot el contexto.
- has utilizado `/model` para explorar los modelos disponibles.
- has aprendido cómo `/share` puede exportar o compartir sesiones.
- has explorado comandos opcionales para trabajar en paralelo, utilizar worktrees y delegar tareas al agente en la nube.

Copilot CLI incluye muchos otros comandos de barra y funciones por explorar. Para terminar el recorrido, [repasaremos lo aprendido][next-lesson] y veremos los pasos siguientes.

## Recursos

- [Referencia de comandos de Copilot CLI][cli-reference]
- [Gestión del contexto en Copilot CLI][context-management]
- [Información sobre el agente de Copilot en la nube][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/

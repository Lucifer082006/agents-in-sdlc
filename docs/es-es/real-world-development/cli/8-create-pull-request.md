---
title: "Lección 8 - Crea y combina la PR de la funcionalidad"
description: "Revisa conjuntamente el filtrado, las instrucciones, la habilidad actualizada, el perfil de control de calidad y las pruebas; después, crea una PR y utiliza Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las actualizaciones de instrucciones, la actualización de la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una misma rama. Es hora de revisarlos en conjunto y abrir una solicitud de incorporación de cambios. Tú combinaste la PR de valoración por estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente dividiríamos la funcionalidad, las actualizaciones de instrucciones, la habilidad y el agente de control de calidad en varias PR. Para simplificar el taller, has mantenido todo el flujo de trabajo de filtrado y calidad en una conversación y rama, y todo ese trabajo se incluirá en esta PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de combinación.
- inspeccionarás el cambio completo de la funcionalidad y las pruebas de verificación.
- crearás la PR de filtrado.
- habilitarás Agent Merge solo después de la revisión y confirmarás que la PR se ha combinado.

## Escenario

Durante el flujo de trabajo de filtrado, has utilizado Copilot para planificar, implementar y verificar una funcionalidad. Ahora Tailspin Toys quiere automatizar las tareas restantes de la PR, pero mantener el control de la autorización para combinar los cambios en manos del desarrollador.

## Presentación de Agent Merge

**Agent Merge** automatiza el trabajo restante necesario para integrar una solicitud de incorporación de cambios. Cuando lo habilitas, Copilot resuelve los bloqueos de la PR —corrige las comprobaciones de integración continua (CI) que fallan, responde a los comentarios de revisión y reorganiza la rama cuando hace falta— y, después, habilita la combinación automática de GitHub si el repositorio la permite.

Hasta ahora has seleccionado **Merge pull request** manualmente. Agent Merge puede encargarse de ello. Revisa el trabajo y confirma que está listo antes de habilitar Agent Merge.

## Utiliza Agent Merge para gestionar la PR

Ya has creado todo el código. Revisémoslo en conjunto, creemos la PR y dejemos que Agent Merge gestione el resto del proceso.

1. Vuelve al Codespace.
2. Abre el cuadro de diálogo del agente con `/agent`.
3. Selecciona **Default** en la lista y pulsa <kbd>Enter</kbd>.
4. Crea una PR nueva con el comando `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará el proceso de integración continua de la PR y la combinará cuando todas las comprobaciones se completen correctamente.
7. Confirma que Copilot muestra un mensaje parecido a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no omite las aprobaciones obligatorias, la protección de ramas, las colas de combinación, la configuración del repositorio ni los permisos que falten. Si se bloquea, lee el motivo indicado y completa manualmente la combinación revisada cuando el repositorio lo permita.

## Resumen y pasos siguientes

Has automatizado varias partes del proceso de desarrollo: generar código, probarlo y validarlo y, ahora, gestionar la solicitud de incorporación de cambios. En esta lección:

- has aprendido qué es Agent Merge y cómo automatiza el ciclo de combinación.
- has inspeccionado el cambio completo de la funcionalidad y las pruebas de verificación.
- has creado la PR de filtrado.
- has habilitado Agent Merge solo después de la revisión y confirmado que la PR se ha combinado.

A continuación, [explorarás otros comandos de barra útiles de Copilot CLI][next-lesson] para gestionar el contexto, los modelos y el uso compartido, y para delegar tareas opcionalmente al agente en la nube.

## Recursos

- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference

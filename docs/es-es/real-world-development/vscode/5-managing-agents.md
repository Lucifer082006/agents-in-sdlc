---
title: "Ejercicio 5 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Cuando GitHub Copilot trabaja en modo agente, actúa de forma autónoma: explora el código base, propone cambios, edita archivos y ejecuta comandos. Como el trabajo se realiza en tu máquina y en tiempo real, Copilot Chat en VS Code te muestra cada llamada a una herramienta y cada edición de archivo mientras se producen. Puedes revisar las diferencias en línea, aceptar o rechazar cambios concretos y guiar la conversación con prompts de seguimiento para perfeccionar o ampliar el trabajo sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad.
- revisarás y aceptarás las ediciones propuestas en los archivos.
- guiarás al agente durante la sesión para que añada un modo claro junto al modo de alto contraste.

## Escenario

El agente personalizado de accesibilidad que utilizaste en el [Ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño ha solicitado otra funcionalidad: también quiere un modo claro para quienes prefieren una apariencia menos oscura que el tema predeterminado. En lugar de empezar una conversación desde cero, guiarás la sesión existente para ampliar el trabajo.

## Supervisa el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y las ediciones que propone. Cada cambio de archivo aparece en línea en el chat, con una diferencia y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat y observa cómo aparecen las llamadas a herramientas. Verás lecturas y ediciones de archivos y los comandos de terminal que solicite ejecutar.
3. Cuando el agente proponga una edición, la diferencia aparecerá en línea:
   - Selecciona **Keep** para aceptar el cambio.
   - Selecciona **Undo** para revertirlo y permitir que el agente vuelva a intentarlo.
4. También puedes abrir el indicador **Files changed**, situado en la parte superior del chat, para ver todos los archivos modificados por el agente en esta sesión.

> [!NOTE]
> Si una llamada a una herramienta requiere permiso —por ejemplo, para ejecutar un comando de terminal—, el agente se detendrá y mostrará una solicitud **Allow**. Aprueba únicamente los comandos que te parezcan adecuados.

5. Espera a que el agente termine de implementar el modo de alto contraste. Deberías ver un mensaje final con un resumen de los cambios.

## Guía al agente para añadir el modo claro

Ya está implementado el modo de alto contraste. Ahora ampliarás la misma conversación para añadir un modo claro. Como continúas en la misma sesión de chat, el agente conserva el contexto de su trabajo anterior, incluidos los componentes que modificó, el enfoque de estilos y el patrón de almacenamiento local.

1. Envía este prompt de seguimiento en la entrada de Copilot Chat:

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. El agente continuará desde donde lo dejó, explorará los cambios realizados y propondrá las modificaciones necesarias para el modo claro.
3. Revisa cada nueva edición a medida que aparezca, como hiciste con el modo de alto contraste. Utiliza **Keep** o **Undo** en cada diferencia.
4. Si el agente sigue un enfoque que no quieres —por ejemplo, intenta reorganizar la interfaz de los controles de una forma que no te gusta—, detenlo con el botón **Stop**, junto a la entrada de chat, y envía un mensaje aclaratorio.

> [!TIP]
> Guiar una sesión en curso es más rápido que empezar de nuevo. Utiliza prompts de seguimiento para solicitar ajustes («simplifica el CSS y utiliza variables CSS en lugar de duplicar selectores»), corregir el enfoque («no añadas un tercer control; integra el modo claro en el panel de configuración existente») o ampliar el alcance («añade también métodos abreviados de teclado para ambos controles»).

## Revisa el conjunto de cambios

Antes de confirmar el trabajo, revisa rápidamente todo lo que ha modificado el agente.

1. Abre la vista **Source Control** en VS Code.
2. Revisa la lista completa de archivos modificados. Deberías ver cambios en componentes Astro, estilos y pruebas relacionadas.
3. Abre algunos de los archivos modificados y revisa las diferencias. Confirma que se aplican los patrones de accesibilidad del agente personalizado: atributos ARIA, navegación con teclado, HTML semántico y persistencia mediante almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en la [siguiente lección][next-lesson].

## Resumen y pasos siguientes

Has utilizado el modo agente de Copilot Chat para supervisar y guiar una sesión en curso: has revisado cada edición de archivo, aceptado o rechazado cambios y enviado prompts de seguimiento para ampliar el trabajo sin perder el contexto.

En esta lección:

- has supervisado el trabajo del agente en tiempo real.
- has aceptado y rechazado ediciones de archivos individuales.
- has guiado una sesión en curso con prompts de seguimiento.

En la [siguiente lección][next-lesson] publicarás el trabajo de accesibilidad en una solicitud de incorporación de cambios y revisarás los cambios creados por el agente personalizado local durante el taller.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Uso del modo agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode

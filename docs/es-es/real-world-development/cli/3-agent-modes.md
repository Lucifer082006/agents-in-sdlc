---
title: "Lección 3 - Modos de agente: Plan y Autopilot"
description: "Acuerda un enfoque con Plan, crea el filtrado a partir de una incidencia con Autopilot y revisa el resultado en modo Interactive."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Empezamos por añadir una pequeña funcionalidad al proyecto. Sin embargo, los cambios de mayor alcance requieren un proceso más sólido. Por suerte, GitHub Copilot CLI está diseñado para integrarse en el flujo de trabajo existente de una organización y ayudarnos a crear lo adecuado de la manera correcta. Esta es la primera de varias lecciones en las que seguirás un proceso habitual de desarrollo con agentes: partirás de una incidencia para crear una funcionalidad, comprobarás que el código es válido y que la funcionalidad se comporta como se espera y, por último, la combinarás con éxito en el proyecto.

> [!NOTE]
> A lo largo del flujo de trabajo de esta funcionalidad utilizarás la misma conversación y rama. Normalmente se utilizarían ramas o PR distintas para los diferentes tipos de archivo, pero vamos a simplificar el proceso para centrarnos en los conceptos principales.

En esta lección:

- iniciarás una conversación nueva de Copilot a partir de una incidencia de GitHub.
- definirás los requisitos en el modo Plan.
- implementarás la funcionalidad con el modo Autopilot.
- revisarás el código.
- validarás manualmente la funcionalidad en un navegador reenviado.

Mientras completas esta funcionalidad, actualizarás las instrucciones del repositorio, personalizarás la habilidad `quality-checks`, añadirás validación con MCP, crearás un agente de control de calidad y abrirás la PR de la funcionalidad.

## Escenario

El catálogo de Tailspin Toys está creciendo y quienes lo visitan necesitan filtrar los juegos por categoría y editorial. La incidencia de la lista de trabajo describe la funcionalidad, pero hay detalles —como permitir combinar categorías— que deben acordarse antes de programar. Utilizarás el modo Plan para resolver estas cuestiones y, a continuación, autorizarás una implementación acotada con Autopilot.

## Contexto

Incorporar agentes de programación con IA al flujo de desarrollo no cambia los principios básicos. ¡Si acaso, los hace aún más importantes! La mayoría de los desarrolladores siguen un flujo parecido a este:

1. Partir de una incidencia registrada que describa el trabajo necesario.
2. Planificar lo que se va a crear.
3. Crear y revisar el código.
4. Ejecutar pruebas para validar el código.
5. Validar manualmente la nueva funcionalidad.
6. Crear una solicitud de incorporación de cambios (PR).
7. Cuando se haya revisado el código y se completen correctamente las comprobaciones de integración continua, combinarlo.

> [!NOTE]
> Los detalles concretos variarán según el equipo y la organización, pero la mayoría seguirá alguna variante de este proceso.

Seguir este enfoque estándar permite comprobar que el código generado por IA cumple los requisitos y pasa por el mismo proceso de revisión que el código escrito manualmente.

## Modos de conversación

El **modo de conversación** determina el grado de autonomía del agente. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar de modo:

- **Interactive**: trabajas con el agente. Este propone cambios y espera tu intervención antes de continuar.
- **Plan**: el agente crea primero un plan y no puede editar archivos del proyecto.
- **Autopilot**: el agente trabaja de forma autónoma: escribe código, ejecuta pruebas e itera hasta completar la tarea.

Empieza en modo Plan, revisa el plan y, después, utiliza Autopilot para implementarlo.

## Parte de la incidencia

Antes de empezar el trabajo de filtrado, vuelve al Codespace y comprueba que el repositorio y el terminal están listos.

1. Vuelve al Codespace. Si está detenido, reinícialo antes de continuar.
2. Confirma que la PR de valoración por estrellas se ha combinado.
3. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Actualiza `main` y crea una rama para el trabajo de filtrado:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicia Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pulsa <kbd>Tab</kbd> dos veces para abrir la pestaña **Issues**.
7. Pulsa <kbd>A</kbd> para mostrar todas las incidencias.
8. Utiliza las teclas de flecha para resaltar la incidencia titulada **Allow users to filter games by category and publisher**.
9. Pulsa <kbd>C</kbd> para añadir la incidencia al prompt y volver a la pestaña **Session**.

Observa que el prompt empieza ahora por `#7` o un número parecido. El carácter `#` permite incluir en el contexto una incidencia o solicitud de incorporación de cambios de GitHub.

## Planifica la funcionalidad de filtrado

La planificación permite definir cómo implementar una funcionalidad o realizar una tarea antes de asignársela a Copilot. En tareas complejas siempre conviene dedicar tiempo a planificar. Cambiemos al modo Plan y pidamos a Copilot que cree el plan.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Plan. Confirma que el indicador situado debajo del prompt muestra **Plan**.
2. Después de la referencia a la incidencia añadida en el paso anterior, escribe este prompt:

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot empezará a preparar el plan: explorará el proyecto y determinará cuál es el mejor enfoque.

3. Es posible que Copilot te pregunte cómo debe funcionar el filtrado. Responde según tus preferencias; no hay respuestas incorrectas.
4. Cuando el plan esté completo, pulsa <kbd>Control</kbd>+<kbd>E</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandirlo.
5. Desplázate por el plan para revisarlo.
6. Pide a Copilot que modifique cualquier parte que no coincida con tus decisiones.

## Aprueba Autopilot

Ya has escrito y revisado el plan; ahora toca implementarlo. Dejemos que Copilot trabaje en ello con Autopilot.

Autopilot permite que Copilot itere sobre el problema hasta que considere que está resuelto.

1. Selecciona **Accept plan and build on autopilot (recommended)** o la opción equivalente de la versión instalada.
2. Confirma que el indicador de modo situado debajo del prompt muestra **Autopilot**.
3. Observa cómo Copilot sigue el plan acordado, genera código y ejecuta pruebas.

> [!NOTE]
> Al aprobar el plan, la implementación puede empezar de inmediato, así que revísalo primero. Si Copilot indica que faltan dependencias o que hay un conflicto de puertos, resuelve el problema de configuración antes de dar por completadas las comprobaciones.

## Revisa y verifica la implementación

Como con cualquier otro código, hay que revisar el código generado antes de combinarlo. Revisemos el código y ejecutemos el sitio para confirmar que todo funciona.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para entrar en el modo Interactive. Confirma que el indicador de modo ya no muestra **Plan** ni **Autopilot**.
2. Escribe `/diff` e inspecciona la implementación del filtrado y las pruebas.
3. Compara el resultado con la incidencia y con las decisiones tomadas durante la planificación.
4. Después de revisar el código, pulsa <kbd>Esc</kbd> para salir de la vista de diferencias.
5. Revisa los resultados de las comprobaciones del proyecto y pide a Copilot que resuelva los fallos.

## Explora la nueva funcionalidad

De acuerdo, el código parece correcto. ¿Pero funciona? Iniciemos la aplicación como antes y abramos el sitio a través del puerto reenviado de Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Prueba el filtrado por categoría, el filtrado por editorial y las combinaciones acordadas en el plan.
4. Confirma que el restablecimiento y el comportamiento sin resultados coinciden con la incidencia y tus decisiones.
5. Vuelve al Codespace y pide a Copilot que detenga el servidor de desarrollo que ha iniciado.

## Resumen y pasos siguientes

Has utilizado distintos modos de conversación para crear y revisar una funcionalidad. En esta lección:

- has iniciado una conversación de Copilot nueva a partir de una incidencia de GitHub.
- has definido los requisitos en el modo Plan.
- has implementado la funcionalidad con el modo Autopilot.
- has revisado el código.
- has validado manualmente la funcionalidad en un navegador reenviado.

A continuación, profundizaremos en la generación de código y en cómo ajustarla a las prácticas documentadas mediante [instrucciones personalizadas][next-lesson].

## Recursos

- [Autopilot en GitHub Copilot CLI][autopilot]
- [Referencia de comandos de Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/

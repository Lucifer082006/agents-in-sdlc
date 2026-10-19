---
title: "Ejercicio 1 - Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Modo agente →][next-lesson]

El contexto es fundamental cuando se trabaja con IA generativa. Si una tarea debe realizarse de una forma concreta o Copilot necesita información de contexto, conviene asegurarse de que pueda acceder a ella. Hay varias formas de compartir contexto específico con Copilot. Una de las más importantes son los [archivos de instrucciones][instruction-files], que permiten orientar la generación de código.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto específico del proyecto, las directrices de programación y los estándares de documentación mediante instrucciones personalizadas del repositorio y archivos de instrucciones asociados a rutas.
- generarás la primera parte del acceso a datos del filtrado —una función auxiliar de editoriales— con las instrucciones *actuales*.
- añadirás un nuevo estándar para todo el repositorio en `.github/copilot-instructions.md`.
- volverás a enviar el mismo prompt y observarás cómo el código generado sigue el nuevo estándar.
- confirmarás en `main` la actualización de instrucciones y la parte del filtrado para que Copilot pueda utilizar las nuevas directrices en el siguiente ejercicio.

> [!CAUTION]
> El código generado puede no seguir algunos de los estándares definidos. Copilot es no determinista. El objetivo de este ejercicio es observar la *tendencia* del cambio de comportamiento tras actualizar las instrucciones, no obtener exactamente los mismos caracteres que en los ejemplos.

## Archivos de instrucciones

### Escenario

Como cualquier buena empresa de desarrollo, Tailspin Toys tiene directrices y requisitos para las prácticas de desarrollo. Entre ellos:

- La capa de datos siempre debe tener pruebas unitarias.
- La interfaz de usuario debe utilizar un tema oscuro y tener un aspecto moderno.
- El código debe incluir documentación en forma de comentarios TSDoc.
- Al principio de cada archivo debe añadirse un bloque de comentarios que describa su función.

Con los archivos de instrucciones, proporcionarás a Copilot la información necesaria para realizar las tareas de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot para que entienda mejor tu estilo de programación y tus requisitos. Son una herramienta eficaz para orientar a Copilot hacia sugerencias y fragmentos de código más pertinentes. Puedes especificar las convenciones de programación y las bibliotecas que prefieres, e incluso el tipo de comentarios que quieres incluir. Puedes crear instrucciones para todo el repositorio o para tipos de archivos concretos y proporcionar contexto específico para cada tarea.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md` es un único archivo de instrucciones que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información general del proyecto pertinente para la mayoría de las solicitudes de chat o CLI, como la pila tecnológica, una descripción de lo que se está desarrollando, las prácticas recomendadas y las directrices globales.
- Puedes crear archivos `.github/instructions/*.instructions.md` para tareas o tipos de archivo concretos. Sirven, por ejemplo, para proporcionar directrices para lenguajes específicos como TypeScript o Astro, o para tareas como crear un componente de interfaz de usuario o pruebas unitarias.

> [!NOTE]
> En el IDE, los archivos de instrucciones solo se utilizan para generar código en Copilot Chat, no para las finalizaciones de código ni las sugerencias de siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente de GitHub Copilot en la nube utilizan tanto las instrucciones del repositorio como los archivos `*.instructions.md` con metadatos `applyTo`.
>
> Además, Copilot [admite archivos de instrucciones de otros estándares][custom-instructions-support], como AGENTS.md y CLAUDE.md.

### Prácticas recomendadas para gestionar archivos de instrucciones

Explicar en detalle cómo crear archivos de instrucciones queda fuera del alcance de este taller. No obstante, los ejemplos del proyecto muestran un enfoque representativo. En general:

- Mantén las instrucciones de `copilot-instructions.md` centradas en directrices generales del proyecto, como una descripción de lo que se desarrolla, la estructura del proyecto y los estándares de programación globales.
- Utiliza los archivos `*.instructions.md` para indicar directrices específicas sobre tipos de archivos (pruebas unitarias, componentes de Astro o capa de datos) o tareas concretas.
- Utiliza lenguaje natural y directrices claras. Incluye ejemplos de código correcto e incorrecto.

No existe una única manera de crear archivos de instrucciones, del mismo modo que no existe una única forma de utilizar la IA. La experimentación te ayudará a descubrir lo que funciona mejor en tu proyecto.

> [!TIP]
> Todos los proyectos que utilicen GitHub Copilot deberían disponer de una colección sólida de archivos de instrucciones. Al explorar los de este proyecto, quizá observes que abarcan numerosos tipos de tareas, incluidas las [actualizaciones de la interfaz de usuario][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada interfaz ofrece esta opción de manera distinta; por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI. La lección de cada interfaz lo indicará cuando sea pertinente.
>
> ¿Buscas plantillas o un punto de partida? Explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones personalizados de este proyecto

Dedica un momento a leer los archivos de instrucciones incluidos en este repositorio: hay un archivo principal `copilot-instructions.md` y varios archivos `*.instructions.md` para distintas tareas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y fíjate en la breve descripción del proyecto y en secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, observa las directrices anidadas para **GitHub Actions Workflows**. Se aplican a cualquier interacción con Copilot.
3. Abre la carpeta `.github/instructions` y examina su contenido. Observa las instrucciones para archivos Astro, la capa de datos Drizzle, las pruebas y otros elementos.
4. Abre `.github/instructions/unit-tests.instructions.md`. Fíjate en el campo `applyTo` de la cabecera: define un patrón glob, relativo a la raíz del repositorio, que determina los archivos a los que se aplican las instrucciones. En este caso, coincide con cualquier archivo de prueba TypeScript, por ejemplo, los que siguen el patrón `**/*.test.ts`.
5. Observa las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Por último, abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Observa los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Así se pueden dividir conjuntos de instrucciones extensos en archivos más pequeños y reutilizables, y señalar ejemplos que Copilot puede seguir al generar código. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de programación del proyecto, pero todavía no exige documentación en el código. En los pasos siguientes añadirás reglas para los comentarios TSDoc y las cabeceras de comentarios de los archivos.

## Crea una rama para los cambios

Sigamos algunas prácticas recomendadas y creemos una rama para los cambios.

1. Vuelve al codespace del ejercicio anterior.
2. Abre un terminal nuevo con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama nueva y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions
   ```

## Utiliza Copilot Chat *antes* de actualizar las instrucciones

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve al codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación limpia. El modo y la selección del modelo varían según el ejercicio; cada paso lo indica cuando es pertinente.

Para observar el efecto de las instrucciones personalizadas, empieza enviando un prompt con el archivo de instrucciones actual. Más adelante lo actualizarás y volverás a enviar el mismo prompt para ver la diferencia.

1. Cierra las pestañas del editor que estén abiertas para que Copilot solo disponga del contexto necesario.
2. Abre `src/lib/publishers.ts` para que Copilot sepa dónde debe ubicarse la función auxiliar.
3. Selecciona **Agent** en el selector de agentes de la vista Chat para que Copilot pueda aplicar cambios a los archivos.

   ![Captura de pantalla del selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Envía este prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot explora el proyecto y aplica actualizaciones de código, a menudo tanto al archivo de la función auxiliar como a sus pruebas.
6. Observa que la función auxiliar propuesta es una función tipada que recibe un cliente `db` como primer argumento y devuelve una matriz tipada de editoriales. Esto se debe a las convenciones de la capa de datos definidas en `.github/instructions/drizzle.instructions.md`, que se aplican a `src/lib/*.ts`.
7. Fíjate en que al código propuesto **le faltan** comentarios TSDoc y una cabecera de comentario a nivel de archivo.

> [!CAUTION]
> Como Copilot es probabilístico, es posible que añada comentarios de documentación incluso sin indicárselo. No pasa nada: lo importante es observar una mejora de la *coherencia* después de actualizar las instrucciones.

## Añade un nuevo estándar para el repositorio

Como se ha indicado, `.github/copilot-instructions.md` proporciona a Copilot información general del proyecto. Documenta los estándares de programación del repositorio para mejorar las sugerencias de código.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Fíjate en que documenta los estándares de programación del proyecto, pero aún no incluye reglas de documentación en el código; por eso la función auxiliar generada no tenía comentarios.
3. Añade estas líneas de Markdown justo debajo de los estándares existentes para indicar a Copilot que añada cabeceras de comentario en los archivos y comentarios TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como has visto en la lección anterior, los archivos de instrucciones pueden crearse en el nivel del repositorio (`.github/copilot-instructions.md`) para proporcionar directrices globales, o como archivos `*.instructions.md` para determinados lenguajes, tipos de archivo o tareas. El archivo del repositorio es el lugar adecuado para los estándares generales, como la regla sobre comentarios de documentación.

## Vuelve a enviar el prompt y observa el cambio

1. Vuelve a Copilot Chat y selecciona **New Chat** para limpiar el contexto.
2. Selecciona de nuevo `src/lib/publishers.ts` para que Copilot se centre en el archivo correcto.
3. Envía el **mismo prompt** que antes:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observa que el archivo propuesto empieza ahora con un bloque de comentarios parecido a este:

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. Observa que la función propuesta incluye ahora un comentario TSDoc parecido a este:

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Acabas de orientar a Copilot para que siga un estándar nuevo y lo aplique a código real que servirá de base para el siguiente ejercicio.

## Confirma las instrucciones y publica la rama

Los archivos de instrucciones son recursos del repositorio, por lo que se gestionan como cualquier otro elemento mediante el control de código fuente. Confirmemos los cambios y publiquemos la rama.

1. Abre una ventana de terminal nueva con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. Desde el terminal, comprueba que están presentes las instrucciones actualizadas y los cambios de la función auxiliar:

   ```bash
   git status
   ```

3. Prepara y confirma las instrucciones y todos los archivos modificados por Copilot para crear la base de la función auxiliar:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Si git status muestra otros cambios de apoyo (por ejemplo, pruebas), añade también esos archivos.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Publica la rama en el repositorio:

   ```bash
   git push -u origin custom-instructions
   ```

## Crea y combina una solicitud de incorporación de cambios

Ya has publicado la rama. Ahora crearemos una solicitud de incorporación de cambios y la vincularemos a la incidencia sobre los estándares de documentación de la lista de trabajo. En lugar de hacerlo manualmente, pediremos a Copilot que encuentre la incidencia, cree una PR que la cierre y la combine.

> [!NOTE]
> Este es el primer ejercicio en el que Copilot realiza acciones en GitHub por ti. La plantilla del proyecto ya configura las herramientas de GitHub para tu espacio de trabajo, así que no tienes que hacer nada. La primera vez que Copilot utilice una de ellas, VS Code puede pedirte que inicies sesión en GitHub; sigue los pasos para permitirlo. En un ejercicio posterior aprenderás cómo funciona esta conexión (Model Context Protocol).

1. Abre Copilot Chat en el codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir la vista Copilot Chat y confirma que **Agent** está seleccionado en el selector.
3. Pide a Copilot que busque la incidencia relacionada con la actualización de los archivos de instrucciones y cree una PR desde la rama actual que describa tanto las actualizaciones de instrucciones como la nueva base de la función auxiliar de editoriales:

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot empezará a buscar la incidencia y a crear la PR.
5. Si Copilot te pide **Allow** para realizar acciones de GitHub, revisa el comando y selecciona **Allow** según corresponda.
6. Cuando se cree la PR, pide a Copilot que la combine y que vuelva a `main`:

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Si Copilot te pide **Allow** para realizar acciones de GitHub y ejecutar comandos de shell, revisa el comando y selecciona **Allow** según corresponda.

¡Has creado y combinado una solicitud de incorporación de cambios con la ayuda de GitHub Copilot!

## Resumen y pasos siguientes

Has explorado cómo obtiene contexto Copilot de los archivos de instrucciones del proyecto y has utilizado Copilot Chat en VS Code para:

- enviar un prompt de generación de código y observar el resultado con las instrucciones *existentes*.
- añadir un nuevo estándar para todo el repositorio a `.github/copilot-instructions.md`.
- volver a enviar el mismo prompt y observar cómo el código propuesto sigue el nuevo estándar.
- confirmar en `main` la actualización de las instrucciones y la base de la función auxiliar para que el siguiente ejercicio pueda continuar el trabajo.

A continuación, aplicarás esas instrucciones en el [modo agente][next-lesson] para que Copilot añada una funcionalidad nueva en varias partes del código.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Instrucciones personalizadas personales para GitHub Copilot][personal-instructions]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/

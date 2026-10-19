---
title: "Ejercicio 1 - Instrucciones personalizadas (agente en la nube)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Agente de GitHub Copilot en la nube →][next-lesson]

El contexto es fundamental cuando se trabaja con IA generativa. Si una tarea debe realizarse de una forma concreta o Copilot necesita información de contexto, conviene asegurarse de que pueda acceder a ella. Los [archivos de instrucciones][instruction-files] proporcionan esa orientación para que Copilot entienda no solo *qué* quieres que haga, sino también *cómo*.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto específico del proyecto, las directrices de programación y los estándares de documentación mediante instrucciones personalizadas del repositorio y archivos de instrucciones asociados a rutas.
- añadirás un nuevo estándar para todo el repositorio en `.github/copilot-instructions.md`.

> [!NOTE]
> A diferencia de los recorridos de VS Code y CLI, aquí no se comparará un prompt *antes y después*: el agente de GitHub Copilot en la nube trabaja de forma asíncrona con incidencias de GitHub, por lo que resulta más difícil mostrar el efecto en tiempo real. Más adelante, en este recorrido, verás cómo influye el archivo de instrucciones cuando revises las solicitudes de incorporación de cambios que genere el agente.

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

- `.github/copilot-instructions.md` es un único archivo de instrucciones que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información general del proyecto que sea pertinente para la mayoría de las solicitudes de chat o CLI, como la pila tecnológica, una descripción de lo que se está desarrollando, las prácticas recomendadas y las directrices globales.
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
2. Explora el archivo y fíjate en la breve descripción del proyecto y en secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. La información de estas secciones se aplica a cualquier interacción con Copilot.
3. Abre la carpeta `.github/instructions` y examina su contenido. Observa las instrucciones para archivos Astro, la capa de datos Drizzle, las pruebas y otros elementos.
4. Abre `.github/instructions/unit-tests.instructions.md`. Fíjate en el campo `applyTo` de la cabecera: define un patrón glob, relativo a la raíz del repositorio, que determina los archivos a los que se aplican las instrucciones. En este caso, coincide con cualquier archivo de prueba TypeScript, por ejemplo, los que siguen el patrón `**/*.test.ts`.
5. Observa las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Por último, abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Observa los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Así se pueden dividir conjuntos de instrucciones extensos en archivos más pequeños y reutilizables, y señalar ejemplos que Copilot puede seguir al generar código. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de programación del proyecto, pero todavía no exige documentación en el código. En los pasos siguientes añadirás reglas para los comentarios TSDoc y las cabeceras de comentarios de los archivos.

## Añade un nuevo estándar para el repositorio

El siguiente paso es la única edición que harás en este ejercicio: añadir una regla para que la documentación del código incluya comentarios TSDoc y una cabecera de comentario a nivel de archivo. El agente en la nube utilizará esta regla más adelante cuando trabaje en las incidencias que le asignes.

Antes de editar, crea una rama de trabajo. El agente en la nube leerá las instrucciones de la rama a la que se dirija la incidencia, pero de todos modos confirmarás tus cambios de forma ordenada:

1. En el terminal del codespace, crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

Como se ha indicado, `.github/copilot-instructions.md` proporciona a Copilot información general del proyecto. Documenta los estándares de programación del repositorio para mejorar las sugerencias de código.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Fíjate en que documenta los estándares de programación del proyecto, pero aún no incluye reglas de documentación en el código; por eso el asistente generado no añadió comentarios de documentación.
3. Añade estas líneas de Markdown justo debajo de los estándares existentes para indicar a Copilot que añada cabeceras de comentario en los archivos y comentarios TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como has visto en la lección anterior, los archivos de instrucciones pueden crearse en el nivel del repositorio (`.github/copilot-instructions.md`) para proporcionar directrices globales, o como archivos `*.instructions.md` para determinados lenguajes, tipos de archivo o tareas. El archivo del repositorio es el lugar adecuado para los estándares generales, como la regla sobre comentarios de documentación.

## Confirma, publica y combina la actualización de instrucciones

El agente en la nube lee los archivos de instrucciones de la rama a la que se dirige la incidencia. En el siguiente ejercicio, asignarás una incidencia a Copilot, que creará una rama a partir de `main`. Por tanto, para que el agente utilice tus instrucciones, estas deben llegar a `main`.

1. Prepara y confirma los cambios:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Publica la rama:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. En github.com, abre una solicitud de incorporación de cambios de `custom-instructions-cloud` a `main` y combínala. Así, el agente en la nube podrá utilizar estas instrucciones cuando trabaje en las incidencias que le asignes en los siguientes ejercicios.

> [!TIP]
> Si prefieres trabajar directamente en `main` durante este taller, puedes omitir la rama y confirmar los cambios directamente en `main`. El paso de la rama permite practicar el flujo que seguirías en un proyecto real.

## Resumen y pasos siguientes

Has explorado cómo utiliza Copilot el contexto de los archivos de instrucciones de este proyecto y has añadido un nuevo estándar para todo el repositorio en `.github/copilot-instructions.md`. En las solicitudes de incorporación de cambios que genere el agente en la nube a lo largo de este recorrido, verás cómo se aplica ese estándar.

Continúa y [asigna tu primera incidencia al agente de GitHub Copilot en la nube][next-lesson].

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/

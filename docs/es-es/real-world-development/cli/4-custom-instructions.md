---
title: "Lección 4 - Guía a Copilot con instrucciones personalizadas"
description: "Explora las instrucciones del repositorio, añade un estándar de documentación y aplícalo al código de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

El contexto es fundamental cuando se trabaja con IA generativa. Si una tarea debe realizarse de una forma concreta, conviene que Copilot disponga de las directrices necesarias. Los [archivos de instrucciones][instruction-files] describen no solo *qué* código quieres, sino también *cómo* debe estructurarse. Ahora que has creado el filtrado, explorarás las instrucciones utilizadas por Copilot, añadirás un estándar de documentación y lo aplicarás al código.

En esta lección:

- explorarás cómo llegan al agente las instrucciones del repositorio y los archivos asociados a rutas.
- actualizarás el archivo de instrucciones para que se sigan los estándares de programación.
- observarás el efecto de los archivos de instrucciones en el código.

## Escenario

Como cualquier buena empresa de desarrollo, Tailspin Toys tiene directrices y requisitos para las prácticas de desarrollo. Entre ellos:

- Los comentarios deben explicar la intención y las decisiones que no sean evidentes, en lugar de repetir el código.
- Las funciones exportadas en `db/` y `src/lib/` deben documentar su propósito, parámetros y valores de retorno con TSDoc/JSDoc, incluido el argumento `db` inyectable cuando exista.
- Los componentes Astro reutilizables deben documentar el contrato de `Props`, y los comentarios deben actualizarse cuando cambie el código relacionado.
- Deben mantenerse las directrices de formato y lint existentes.

Con los archivos de instrucciones, proporcionarás a Copilot la información necesaria para realizar las tareas de acuerdo con estas prácticas.

## Archivos de instrucciones

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot para que entienda mejor tu estilo de programación y tus requisitos. Son una herramienta eficaz para orientar a Copilot hacia sugerencias y fragmentos de código más pertinentes. Puedes especificar las convenciones de programación y las bibliotecas que prefieres, e incluso el tipo de comentarios que quieres incluir. Puedes crear instrucciones para todo el repositorio o para tipos de archivos concretos y proporcionar contexto específico para cada tarea.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md` es un único archivo de instrucciones que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información general del proyecto pertinente para la mayoría de las solicitudes.
- Los archivos `.github/instructions/*.instructions.md` proporcionan directrices para determinados lenguajes, tipos de archivo o tareas.

> [!NOTE]
> La compatibilidad con otros formatos de instrucciones varía según el entorno. Consulta la [referencia de compatibilidad con instrucciones personalizadas][custom-instructions-support] antes de depender de un formato concreto.

## Explora los archivos de instrucciones personalizadas del proyecto

El proyecto inicial ya incluye varios archivos de instrucciones. Explorémoslos antes de hacer cambios para ver su efecto.

1. Vuelve al Codespace.
2. En el editor de Codespaces, no en el terminal, abre `.github/copilot-instructions.md`.
3. Explora el archivo y fíjate en la descripción breve del proyecto y en las directrices de programación. Estas instrucciones se aplican a todas las interacciones con Copilot en este repositorio.
4. Abre la carpeta `.github/instructions` y explora sus archivos. Observa las instrucciones para archivos Astro, la capa de datos Drizzle, las pruebas y otros elementos.
5. Abre `.github/instructions/unit-tests.instructions.md`. Observa el campo `applyTo` de la cabecera: define un patrón glob que determina a qué archivos se aplican las instrucciones.
6. Abre `.github/instructions/drizzle.instructions.md` y observa los enlaces a otros archivos de instrucciones y archivos existentes del proyecto. Así se pueden dividir conjuntos de instrucciones extensos en archivos más pequeños y reutilizables, y señalar ejemplos que Copilot puede seguir.

## Actualiza los archivos de instrucciones según las directrices del equipo

Los archivos existentes son un buen comienzo, pero aún falta algo. Modificaremos el archivo principal `copilot-instructions.md` para exigir comentarios TSDoc en el nuevo código TypeScript.

1. En `.github/copilot-instructions.md`, busca la sección **Code formatting guidance**, que debería estar cerca de la línea 35.
2. Añade este punto al final de la sección:

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

El archivo se guardará automáticamente.

## Utiliza las directrices actualizadas

Copilot CLI carga las instrucciones del repositorio cuando se inicia una conversación. Después de guardar las instrucciones actualizadas, cierra y reanuda la conversación de filtrado para que Copilot utilice las nuevas directrices sin perder el contexto de la funcionalidad.

1. Si la conversación de filtrado sigue abierta, escribe `/exit` para cerrarla. Copilot CLI guarda las conversaciones automáticamente.
2. Desde la raíz del repositorio, en la rama `game-filters-cli`, reanuda la conversación guardada:

   ```bash
   copilot --resume
   ```

   Si se te pregunta, selecciona la conversación de filtrado y confirma que está activa la rama `game-filters-cli`.
3. Pide a Copilot que aplique las directrices actualizadas:

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. Escribe `/diff` y revisa los archivos TypeScript modificados. Observa los nuevos comentarios TSDoc y confirma que describen el código correctamente.

## Resumen y pasos siguientes

Has explorado cómo obtiene contexto Copilot CLI de los archivos de instrucciones y has aplicado un estándar nuevo a la funcionalidad. En concreto:

- has explorado cómo llegan al agente las instrucciones del repositorio y los archivos asociados a rutas.
- has actualizado el archivo de instrucciones para que se sigan los estándares de programación.
- has observado el efecto de los archivos de instrucciones en el código.

A continuación, [personalizarás y ejecutarás la habilidad reutilizable quality-checks][next-lesson] para asegurar que el lint y las pruebas se ejecutan de forma coherente.

## Recursos

- [Añadir instrucciones personalizadas a Copilot CLI][instruction-files]
- [Compatibilidad con instrucciones personalizadas][custom-instructions-support]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/

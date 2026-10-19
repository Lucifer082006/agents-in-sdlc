---
title: "Lección 5 - Personaliza y utiliza la habilidad quality-checks"
description: "Explora la habilidad quality-checks existente, personaliza el formato del informe y utilízala para validar el filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Programar no consiste únicamente en escribir código. Ya hemos comprobado manualmente que el código funciona y hemos utilizado archivos de instrucciones para asegurarnos de que sigue nuestros estándares. Pero ¿qué ocurre con las pruebas, el lint y el resto de tareas de integración continua (CI)?

Las **habilidades de agente** son ideales para este tipo de tareas. Ayudan a Copilot a entender cómo ejecutar operaciones como estas correctamente.

En esta lección:

- explorarás la habilidad `quality-checks` existente.
- personalizarás el formato de sus resultados.
- volverás a cargar y ejecutar la habilidad.

## Escenario

Tailspin Toys utiliza la habilidad `quality-checks` para ejecutar pruebas unitarias, lint y comprobaciones de tipos. El equipo quiere mejorar el informe para que los resultados sean más fáciles de leer.

## Instrucciones, scripts y recursos

Las habilidades de agente agrupan instrucciones reutilizables para tareas, scripts ejecutables y recursos de apoyo que el agente carga cuando los necesita. En esencia, son carpetas con el nombre de la habilidad que contienen un archivo Markdown llamado `SKILL.md`. El archivo incluye metadatos frontmatter con un nombre y una descripción, una descripción general de la habilidad e indicaciones sobre cuándo debe utilizarse. También puede incluir subcarpetas con scripts y otros recursos.

> [!NOTE]
> Una habilidad no necesita carpetas ni archivos adicionales. La habilidad `quality-checks` de Tailspin Toys solo contiene `SKILL.md`, porque utiliza los comandos existentes del proyecto.

Las habilidades pueden residir en la carpeta `.github/skills` del proyecto como recurso compartido y reutilizable del repositorio, o en la carpeta personal de habilidades `~/.copilot/skills`.

## Explora la habilidad

Exploremos la habilidad que ha creado el equipo de Tailspin Toys para ejecutar pruebas unitarias, lint y comprobaciones de tipos: `quality-checks`.

1. Vuelve al Codespace. En el editor de Codespaces, abre `.github/skills/quality-checks/SKILL.md`.
2. Lee los campos `name` y `description` de la parte superior. La descripción ayuda a Copilot a saber cuándo debe invocar la habilidad.
3. Lee las instrucciones y observa cómo orientan a Copilot en el proceso de pruebas y lint.
4. Fíjate en que la habilidad aún no contiene una sección **Results output formatting**.

## Ejecuta la habilidad antes de modificarla

Las habilidades se pueden invocar directamente desde Copilot CLI o mediante lenguaje natural. Pidamos a Copilot que ejecute sus tres comprobaciones.

1. Vuelve a la conversación de filtrado en modo Interactive.
2. Envía este prompt:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Observa el informe final.

## Personaliza el informe

Nos gustaría tener un informe mejor que indique qué se ha ejecutado, si se ha superado o no y qué han comunicado realmente las herramientas. Actualicemos la habilidad para que genere ese informe.

1. Vuelve a `.github/skills/quality-checks/SKILL.md`.
2. Añade esta sección al final del archivo:

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. El archivo se guardará automáticamente.

## Ejecuta la habilidad actualizada

Ya has realizado el cambio. Veamos cómo funciona. Copilot CLI puede volver a cargar las habilidades editadas sin reiniciar la conversación.

1. Escribe:

   ```plaintext
   /skills reload
   ```

2. Utiliza exactamente el mismo prompt que antes:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Observa el informe final y compáralo con el primero.

## Resumen y pasos siguientes

Has personalizado y utilizado una habilidad de agente existente. En esta lección:

- has explorado la habilidad `quality-checks` para las pruebas unitarias, el lint y las comprobaciones de tipos.
- has personalizado el formato de sus resultados.
- has vuelto a cargar y ejecutado la habilidad.

Ese cambio acompañará al filtrado en la PR de la funcionalidad. A continuación, dejarás que Copilot interactúe directamente con el sitio [mediante el servidor MCP de Playwright][next-lesson].

## Más ejemplos de habilidades

Estos ejemplos de la comunidad son referencias, no tareas adicionales:

- [Especificación de Agent Skills][skill-spec]
- [Flujo de contribución: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas y script de exportación incluido: `drawio`][drawio-example]
- [Pruebas en el navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing

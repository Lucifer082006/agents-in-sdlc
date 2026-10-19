---
title: "Lección 7 - Crea y utiliza un agente de control de calidad"
description: "Crea un agente personalizado de control de calidad que combine los requisitos de la incidencia, la habilidad quality-checks y Playwright MCP."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Has utilizado la habilidad `quality-checks` para ejecutar comprobaciones automatizadas y Playwright MCP para observar la experiencia de filtrado en un navegador. Ahora combinarás esas capacidades en un agente personalizado con un proceso de control de calidad bien definido.

En esta lección:

- explorarás cómo trabaja un agente personalizado con instrucciones, habilidades y herramientas MCP.
- crearás y revisarás un perfil reutilizable de control de calidad.
- seleccionarás el agente de control de calidad y revisarás sus conclusiones respecto a la incidencia de filtrado.

## Escenario

Antes de abrir una solicitud de incorporación de cambios (PR), Tailspin Toys quiere revisar de forma coherente los requisitos, la calidad del código, las comprobaciones automatizadas, la cobertura de pruebas y el comportamiento en el navegador. Un agente personalizado puede coordinar ese proceso de control de calidad y proporcionar un informe reutilizable.

## ¿Qué es un agente personalizado?

Un agente personalizado es una versión especializada de Copilot definida en un perfil Markdown. El perfil describe su propósito, instrucciones y herramientas disponibles. En este taller, definirás un rol de control de calidad en `.github/agents/qa.agent.md` y lo seleccionarás en Copilot CLI.

Las personalizaciones utilizadas tienen funciones distintas. Las instrucciones del repositorio describen los estándares del equipo. La habilidad `quality-checks` agrupa comprobaciones repetibles. Playwright MCP proporciona herramientas de navegador. El perfil de control de calidad indica a Copilot cómo utilizar esas capacidades para evaluar los requisitos y comunicar sus conclusiones. No sustituye a las demás personalizaciones ni requiere otra conversación.

## Crea el perfil de control de calidad

Antes de abrir la PR de la funcionalidad, pide a Copilot que cree un perfil reutilizable de control de calidad. El perfil definirá las comprobaciones y los límites que debe respetar.

1. Vuelve al Codespace y confirma que la conversación de filtrado está en modo Interactive.
2. Pide al agente predeterminado que cree el agente personalizado:

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## Inspecciona el perfil

Antes de utilizar el agente nuevo, revisa su perfil para confirmar que Copilot ha recogido el flujo de control de calidad y los límites deseados. Así evitarás que un agente incompleto o demasiado amplio cambie el código cuando solo quieres verificarlo.

1. Escribe `/diff` y abre `.github/agents/qa.agent.md`.
2. Lee la cabecera. El campo `description` es obligatorio; `name` es opcional, pero si lo incluyes el agente tendrá un nombre visible claro.
3. Lee las instrucciones del perfil y confirma que el agente parte de los requisitos, sigue las instrucciones del repositorio, ejecuta la habilidad `quality-checks` y utiliza Playwright MCP.
4. Confirma que el agente aporta pruebas que respaldan sus conclusiones, pide permiso antes de cambiar código de implementación y no confirma cambios ni abre solicitudes de incorporación de cambios.
5. Si falta alguna responsabilidad o límite, pide al agente predeterminado que revise el perfil antes de continuar.

## Ejecuta el control de calidad para la incidencia

Copilot CLI carga los agentes del proyecto cuando se inicia una conversación. Después de crear el perfil, reanuda la misma conversación de filtrado y selecciona el agente de control de calidad para que pueda utilizar la incidencia y las decisiones de planificación que ya están en contexto.

1. Activa el agente con este prompt:

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> Como acabas de crear el agente, es posible que no aparezca en la lista de agentes disponibles. Aun así, el comando anterior lo activará.

2. Envía este prompt para pedirle que revise la funcionalidad:

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. El agente de control de calidad empezará a trabajar.
4. Cuando termine, lee el informe.

## Resumen y pasos siguientes

Has añadido un rol especializado reutilizable al flujo de trabajo y has revisado su trabajo. En esta lección:

- has explorado cómo trabaja un agente personalizado con instrucciones, habilidades y herramientas MCP.
- has creado e inspeccionado un perfil reutilizable de control de calidad.
- has seleccionado el agente de control de calidad y revisado sus conclusiones respecto a la incidencia de filtrado.

Ya tienes la implementación, la actualización de la habilidad, el perfil de control de calidad, las pruebas y el informe de verificación listos para revisar. A continuación, [los reunirás en una PR de funcionalidad y utilizarás Agent Merge][next-lesson].

## Recursos

- [Crear agentes personalizados para Copilot CLI][create-agents]
- [Configuración de agentes personalizados][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration

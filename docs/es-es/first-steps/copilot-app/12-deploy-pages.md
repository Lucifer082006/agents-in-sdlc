---
title: "Lección 12 - Publicar el sitio con GitHub Pages"
description: "Utiliza la aplicación GitHub Copilot para configurar un flujo de trabajo de Pages, revisar su solicitud de incorporación de cambios y comprobar que Space Quiz está disponible."
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

Publica Space Quiz en Internet con GitHub Pages para que todo el mundo pueda acceder. Copilot puede preparar el flujo de trabajo y guiar la publicación, pero sigues siendo responsable de revisar los cambios y confirmar que el sitio está disponible.

En esta lección, harás lo siguiente:

- pedir a Copilot que configure la publicación en GitHub Pages.
- revisar el flujo de trabajo y la solicitud de incorporación de cambios antes de combinarla.
- seguir la publicación y comprobar el sitio publicado.

## Escenario

Space Quiz ya está en un repositorio de GitHub, pero la gente aún necesita una forma sencilla de probarlo. Publicar el sitio de un solo archivo con GitHub Pages permite acceder a él desde una URL pública sin añadir un servidor ni dependencias de ejecución.

## Pide a Copilot que publique el sitio

En la sesión de la aplicación Copilot para `space-quiz`, envía este prompt:

```plaintext
Enable GitHub Pages for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and merge it after required checks pass. Monitor the deployment until it succeeds, then report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![Diagrama de tres etapas: una solicitud de incorporación de cambios de space-quiz combinada; una ejecución correcta de Deploy website en GitHub Actions con los pasos Build website, Upload artifact y Deploy to Pages; y Space Quiz publicado en yourname.github.io/space-quiz/.](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> Este prompt pide al agente que cree y combine una solicitud de incorporación de cambios y modifique la configuración de Pages del repositorio. Revisa las diferencias, los permisos de publicación y cualquier solicitud de aprobación de GitHub antes de continuar.

## Revisa el flujo de trabajo y comprueba la publicación

1. Confirma que el archivo README solo se añade si el repositorio aún no tiene uno.
2. Revisa los cambios de la solicitud de incorporación de cambios, incluidos los pasos de compilación y publicación del flujo de trabajo.
3. Combina los cambios solo cuando se superen las comprobaciones obligatorias de la solicitud.
4. Sigue la ejecución de publicación de GitHub Actions hasta que se complete correctamente.
5. Abre la URL indicada y confirma que Space Quiz se carga.

Si el agente no puede habilitar Pages ni combinar la solicitud de incorporación de cambios, sigue las instrucciones específicas que te proporcione. No des por finalizada la tarea hasta que la publicación se complete correctamente y el sitio esté disponible.

## Resumen y pasos siguientes

Space Quiz ya está disponible como sitio publicado y has comprobado que funciona, en lugar de confiar únicamente en el informe del agente. Continúa con la [Lección 13: Repaso y pasos siguientes][next-lesson].

[next-lesson]: ../13-review/

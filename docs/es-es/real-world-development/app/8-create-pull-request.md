---
title: "Lección 8: Crea y fusiona la PR de la función"
description: "Revisa conjuntamente el filtrado, las instrucciones, la actualización de la habilidad, el perfil de QA y las pruebas; después, crea una PR y usa Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La implementación del filtrado, las actualizaciones de instrucciones, la actualización de la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una rama. Es el momento de revisarlos juntos y abrir una solicitud de incorporación de cambios. Fusionaste tú la PR de valoración con estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separaríamos la función, las actualizaciones de instrucciones, la actualización de la habilidad y el agente de QA en varias PR. Para agilizar el taller, has mantenido todo el flujo de trabajo de filtrado y calidad en una sesión y una rama, y lo incluirás en esta PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de una fusión;
- inspeccionarás la PR completa de la función y las pruebas de verificación;
- autorizarás Agent Merge solo después de la revisión y confirmarás que la PR se ha fusionado.

## Escenario

Durante el flujo de trabajo de filtrado has usado Copilot para planificar, implementar y verificar una función. Ahora Tailspin Toys quiere automatizar el trabajo restante de la PR sin quitar al desarrollador el control de la autorización para fusionarla.

## Presentación de Agent Merge

**Agent Merge** automatiza el trabajo restante para incorporar una solicitud de incorporación de cambios mediante la aplicación GitHub Copilot. Al habilitarlo, la sesión de la aplicación lee la solicitud y resuelve los impedimentos: corrige las comprobaciones de integración continua (CI) que fallan, responde a comentarios de revisión y vuelve a aplicar los cambios sobre la rama base cuando hace falta. Después la fusiona en cuanto GitHub lo permite. Se ejecuta en segundo plano, sigue funcionando tras reiniciar la aplicación y se desactiva cuando se fusiona la solicitud.

Hasta ahora has seleccionado tú **Merge pull request**. Agent Merge puede encargarse de esa responsabilidad, pero sus permisos para editar código y fusionarlo siguen necesitando tu autorización explícita. Revisa las acciones que puede realizar y el trabajo antes de conceder permiso para fusionar.

## Usa Agent Merge para gestionar la PR

Ya has creado y revisado todo el código. Permitamos que Agent Merge gestione el proceso de la PR.

1. Usa el selector de agentes para seleccionar **Default agent**.
2. Selecciona el menú desplegable junto a **Create PR**.
3. Selecciona **Agent Merge**. El botón cambiará a **Agent Merge**.
4. Selecciona el botón **Agent Merge** para iniciar el proceso. Elegir la opción del menú desplegable no lo inicia por sí solo.

Se inicia el proceso de Agent Merge. El agente:

- crea la solicitud de incorporación de cambios con un título y una descripción;
- si has iniciado la sesión desde una incidencia, la menciona en el cuerpo de la descripción;
- vuelve a aplicar los cambios sobre la rama de destino o gestiona posibles conflictos de fusión;
- supervisa la integración continua para comprobar que se superen todas las comprobaciones;
- supervisa los comentarios de otros desarrolladores y de la revisión de código de Copilot, y hace cambios para responder a ellos;
- puede fusionar automáticamente la PR cuando todo se haya completado correctamente.

Permitamos también que Agent Merge fusione la PR cuando se hayan superado todas las comprobaciones.

5. Revisa el diff de la PR y las pruebas de verificación antes de conceder permiso para fusionar.
6. Selecciona el menú desplegable junto a **Agent Merge**.
7. Comprueba que **Merge pull request** esté marcado.
8. Pide explícitamente al agente que fusione la PR:

    ```plaintext
    Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
    ```

9. Supervisa el progreso y resuelve cualquier impedimento que se indique. Antes de continuar, confirma que el estado de la PR sea **Merged**. Que el permiso de fusión esté habilitado o que el agente esté ejecutándose no significa que la PR ya se haya fusionado.

> [!IMPORTANT]
> Agent Merge no omite las protecciones del repositorio ni los permisos que falten. Resuelve esos impedimentos antes de continuar.

## Resumen y siguientes pasos

Has automatizado varias partes del proceso de desarrollo: generación de código, pruebas y validación y, ahora, el proceso de la solicitud de incorporación de cambios. En esta lección:

- has aprendido qué es Agent Merge y cómo automatiza el ciclo de vida de una fusión;
- has inspeccionado la PR completa de la función y las pruebas de verificación;
- has autorizado Agent Merge solo después de la revisión y has confirmado la fusión de la PR.

A continuación, [usarás un canvas existente y crearás uno de clasificación][next-lesson] para explorar una forma más completa de examinar, planificar y visualizar el trabajo con el agente.

## Recursos

- [Gestionar incidencias y solicitudes de incorporación de cambios con la aplicación GitHub Copilot][managing-issues-prs]
- [Acerca de la aplicación GitHub Copilot][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app

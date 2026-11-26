---
title: "Lección 3: Modos de agente: Plan y Autopilot"
description: "Explora los modos de agente: utiliza Plan para acordar un enfoque, Autopilot para crear el filtrado a partir de una incidencia e Interactive para revisar y verificar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

Empezamos añadiendo una función pequeña al proyecto. Sin embargo, los cambios más amplios requieren un proceso más robusto. La aplicación GitHub Copilot está diseñada para integrarse en el flujo de trabajo de una organización y ayudarnos a crear lo adecuado de la manera correcta. Esta es la primera de varias lecciones en las que seguirás un proceso habitual de desarrollo con agentes: partirás de una incidencia para crear una función, comprobarás que el código es válido y que la función se comporta como esperas y, por último, la fusionarás correctamente en el proyecto.

> [!NOTE]
> A medida que continúes con el flujo de trabajo de la función, usarás la misma sesión. Normalmente se usarían sesiones o PR distintas para los diferentes tipos de archivo, pero simplificaremos el proceso para centrarnos en los conceptos principales.

En esta lección:

- iniciarás una sesión de agente nueva a partir de una incidencia de GitHub;
- definirás los requisitos en modo **Plan**;
- implementarás la nueva función en modo **Autopilot**;
- revisarás el código;
- validarás la función manualmente en un canvas del navegador.

Durante el desarrollo de esta función también actualizarás las instrucciones del repositorio, personalizarás la habilidad existente `quality-checks`, añadirás validación MCP, crearás un agente de QA y abrirás la PR de la función.

## Escenario

El catálogo de Tailspin Toys está creciendo y sus visitantes necesitan filtrar los juegos por categoría y editorial. La incidencia del backlog describe la función, pero antes de programar hay que acordar algunos detalles, como la combinación de categorías. Usarás el modo Plan para tomar esas decisiones y después autorizarás una implementación acotada con Autopilot.

## Contexto

Incorporar agentes de programación con IA al flujo de desarrollo no cambia sus fundamentos. De hecho, son todavía más importantes. La mayoría de los desarrolladores siguen un proceso parecido a este:

1. Abrir una incidencia registrada que describa el trabajo.
2. Crear un plan de lo que hay que desarrollar.
3. Crear y revisar el código.
4. Ejecutar las pruebas para validar el código.
5. Validar manualmente las nuevas funciones.
6. Crear una solicitud de incorporación de cambios (PR).
7. Cuando se haya revisado el código y se haya completado correctamente el proceso de integración continua, fusionar el código.

> [!NOTE]
> Los detalles concretos dependen del equipo y de la organización, pero la mayoría de los procesos son variaciones de los pasos anteriores.

Al seguir este enfoque habitual, te aseguras de que el código generado por IA cumpla los requisitos y pase por el mismo proceso de revisión que el código escrito manualmente.

## Modos de sesión

El **modo de sesión** determina cuánta autonomía tiene el agente. Puedes seleccionarlo en el menú desplegable debajo del campo de prompt y cambiarlo en cualquier momento:

- **Interactive**: tú y el agente trabajáis en equipo. El agente propone cambios y espera tus indicaciones antes de continuar.
- **Plan**: el agente crea primero un plan. Revisas y apruebas el plan antes de que el agente lo ejecute.
- **Autopilot**: el agente trabaja de forma totalmente autónoma: escribe código, ejecuta pruebas e itera sin esperar indicaciones.

Empieza en el modo Plan, revisa el plan y después usa Autopilot para implementarlo.

## Inicia una sesión a partir de la incidencia

Antes de empezar, confirma que se haya fusionado la PR de valoración con estrellas y que la rama local `main` esté actualizada.

1. En **Sessions**, en la barra lateral, selecciona la carpeta del repositorio Tailspin Toys y, después, **Issues** en la navegación superior del repositorio. Abre **Allow users to filter games by category and publisher**. Es una incidencia existente de la plantilla; no tienes que crear otra.
2. Selecciona **New session** y elige un **new working tree** basado en `main` actualizado.

    ![Vista de una incidencia en la aplicación GitHub Copilot con una flecha que señala el botón New session](../../../_images/app-new-session-from-issue.png)

3. Confirma que la incidencia esté vinculada a la sesión y selecciona **Plan** en el selector de modo.

> [!NOTE]
> Si no se encuentra Node.js o npm, pide al agente que ejecute `node --version` y `npm --version`. Si funcionan en tu terminal pero no en la sesión, revisa `PATH` o la configuración del gestor de versiones de la aplicación y ciérrala por completo antes de volver a abrirla. Repite la operación y las comprobaciones que fallaron; no reinstales npm solo porque el agente no lo encuentre.

## Planifica la función de filtrado

La planificación te permite revisar el enfoque antes de que Copilot escriba código. Como has iniciado la sesión desde la incidencia, Copilot ya tiene contexto sobre la solicitud de la función. Envía:

```plaintext
Build this feature.
```

Responde a las preguntas de Copilot y compara el plan con los criterios de aceptación de la incidencia. Comprueba que incluya el filtrado por categoría y editorial, controles accesibles, cambios en el acceso a los datos y pruebas. Aclara cualquier comportamiento poco definido, como la combinación de varias categorías o qué ocurre cuando ningún juego coincide.

El plan debe incluir lint, pruebas unitarias, pruebas E2E y comprobación de tipos con las herramientas existentes del proyecto. Céntralo en implementar y probar el filtrado; crearás la PR después de completar el flujo de calidad. Pide cambios en el plan antes de aprobarlo y guarda la URL de la incidencia y las aclaraciones acordadas para validarlas más adelante.

## Aprueba Autopilot explícitamente

Cuando estés conforme con el plan, selecciona **Approve and implement this plan** o la opción equivalente de tu versión. Comprueba el modo activo en lugar de dar por hecho que la aprobación cambia el modo. Confirma que el indicador muestre **Autopilot**.

Copilot empezará a implementar la función. Verás cómo sigue el plan acordado, genera código y ejecuta pruebas de forma iterativa.

> [!NOTE]
> La aprobación puede iniciar la implementación inmediatamente, así que revisa el plan primero. Si Copilot informa de dependencias ausentes o de un conflicto de puertos, resuelve el problema de configuración antes de dar las comprobaciones por completadas. Detén solo los servidores que hayas iniciado.

## Revisa y verifica la implementación

Como cualquier otro código, el código generado debe revisarse antes de fusionarlo. Revisemos el código y ejecutemos el sitio para comprobar que todo funciona.

1. Abre **Changes** e inspecciona la implementación del filtrado y las pruebas.
2. Compara el resultado con la incidencia y las aclaraciones aprobadas, incluidas las combinaciones de categorías y editoriales. Comprueba que los cambios sigan las instrucciones existentes del repositorio.
3. Examina la salida de lint, las pruebas unitarias, las pruebas E2E y la comprobación de tipos. Una comprobación omitida no cuenta como superada.
4. Resuelve los fallos y vuelve a ejecutar las comprobaciones afectadas antes de aceptar la implementación. La configuración E2E de Playwright compila y sirve una vista previa, y puede reutilizar un servidor local. Asegúrate de que el servidor probado pertenezca a este worktree y no a una lección anterior.

## Explora la nueva función

El código parece correcto, pero ¿se ejecuta? Iniciemos la aplicación como antes y abramos el sitio en un canvas del navegador.

1. Envía este prompt para pedir a Copilot que inicie la aplicación y abra la página en el canvas:

    ```plaintext
    Start the app and open it in the browser canvas.
    ```

2. En unos instantes, se iniciará la aplicación y se abrirá una ventana del navegador dentro de la aplicación Copilot.
3. Selecciona una categoría y confirma que los resultados coincidan. Borra el filtro, selecciona una editorial y comprueba que los resultados correspondan a esa editorial.
4. Combina filtros de categoría y editorial y compara los resultados con la incidencia y el plan aprobado. Si se admiten varias categorías, comprueba el comportamiento acordado para combinarlas.
5. Prueba una combinación sin juegos coincidentes y revisa el estado vacío. Borra o restablece los filtros y confirma que vuelva a aparecer la lista completa.
6. Cuando termines, pide a Copilot que detenga el servidor de desarrollo que inició en esta sesión:

    ```plaintext
    Stop the dev server and close the browser canvas.
    ```

## Conserva esta sesión para las lecciones siguientes

Todavía no crees ni fusiones la PR de filtrado. Conserva esta sesión, worktree y rama para las lecciones 4 a 8. A continuación, aplicarás instrucciones personalizadas al código de filtrado y después añadirás mejoras a la habilidad y al agente de QA antes de crear y fusionar la PR conjunta en la lección 8. Puedes dejar la sesión inactiva después de detener el servidor de desarrollo y retomarla en la lección siguiente.

## Resumen y siguientes pasos

Has utilizado distintos modos de agente para crear y revisar una función. En esta lección:

- has iniciado una sesión de agente nueva a partir de una incidencia de GitHub;
- has definido los requisitos en modo **Plan**;
- has implementado la nueva función en modo **Autopilot**;
- has revisado el código;
- has validado la función manualmente en un canvas del navegador.

A continuación, profundizarás en cómo se genera el código y te asegurarás de que siga las prácticas documentadas [mediante instrucciones personalizadas][next-lesson].

## Recursos

- [Trabajar con sesiones de agente en la aplicación GitHub Copilot][agent-sessions]

[next-lesson]: ../4-custom-instructions/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions

---
title: "Ejercicio 2: Añade funcionalidades con el modo agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas de una aplicación suelen requerir cambios en varios archivos y tareas como ejecutar pruebas. El flujo de trabajo de desarrollo suele consistir en buscar los archivos necesarios, hacer los cambios, ejecutar las pruebas, depurar, averiguar qué archivo falta, volver a actualizarlo... y así sucesivamente.

El modo agente de Copilot te ayuda con todo esto.

El modo agente está diseñado para trabajar con más autonomía en el IDE. Se comporta de forma parecida a un desarrollador: primero explora la estructura del proyecto, después realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los errores que encuentra. Veamos cómo usarlo para añadir funciones al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo agente y el agente en la nube ofrecen experiencias distintas. El modo agente realiza las tareas en el IDE, lo que permite ciclos de respuesta rápidos e interacción. El agente en la nube está pensado para trabajar de forma asíncrona como un miembro del equipo y comunicarse contigo mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás a:

- dejar que el modo agente de Copilot explore el proyecto, identifique los archivos pertinentes y coordine los cambios;
- usar el modo agente de GitHub Copilot para implementar funcionalidades en la interfaz de usuario y la capa de datos;
- revisar los cambios y las pruebas generadas antes de fusionarlos en el código base.

## Escenario

A medida que aumenta la lista de juegos, quieres que los usuarios puedan filtrarlos por categoría y editorial. En el ejercicio anterior ya añadiste un asistente para editoriales; ahora completarás el trabajo restante en la capa de datos, la interfaz de usuario y las pruebas con el modo agente de Copilot.

## Ejecuta el sitio web Tailspin Toys

Antes de hacer cambios, exploremos el sitio web Tailspin Toys para entender cómo funciona actualmente.

El sitio web es una plataforma de financiación colectiva para juegos de mesa con temática de desarrollo. Permite mostrar juegos y sus detalles. Es una única aplicación Astro que genera páginas HTML estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local mediante Drizzle ORM en su frontmatter: no hay una API de backend independiente ni un framework de interfaz en el cliente. Los asistentes de acceso a datos reutilizables están en `src/lib/`, y la interactividad se añade mediante un pequeño elemento `<script>` de Astro con ámbito propio que usa las API DOM estándar.

### Inicia el sitio web

Se ha incluido la tarea `npm run dev` para iniciar el único servidor de desarrollo de Astro. Puedes ejecutarla en GitHub Codespaces siguiendo estos pasos:

1. Vuelve al codespace. Deberías estar en `main` después del ejercicio anterior.
2. Abre una ventana de terminal nueva en el codespace con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea y cambia a una rama nueva para este trabajo de filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Ejecuta este comando para iniciar el sitio:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo de Astro esté listo, aparecerá un mensaje con la URL, similar a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo que te pregunta si quieres abrir una ventana del navegador para `http://localhost:4321`, ciérralo seleccionando **x**.

5. Abre el sitio manteniendo pulsado <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando la dirección del servidor Astro `http://localhost:4321` en el terminal.

> [!NOTE]
> En un codespace, al seleccionar el enlace localhost en el terminal, se te redirigirá automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Es un túnel privado al codespace, que ahora aloja el servidor web.

### Explora el sitio web

Cuando el sitio esté en ejecución, puedes explorar sus funciones. Entre las principales se encuentran:

- **Página de inicio**: muestra juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre una página con más información, como el título, la descripción, la editorial y la categoría.

## Explora el backlog con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve al codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. La selección de modo y modelo varía en cada ejercicio; los pasos correspondientes indicarán qué opción se necesita.

La implementación inicial del sitio funciona, pero queremos ampliarla. Empecemos por revisar el backlog. Al crear el proyecto a partir de la plantilla, se generó automáticamente un backlog de incidencias. Pide a GitHub Copilot que te muestre esos elementos.

1. En el menú desplegable de agentes de la vista Chat, selecciona **Agent**. El agente planifica e implementa cambios de forma autónoma en varios archivos, ejecuta comandos del terminal e invoca herramientas.

   ![Captura del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores de este taller no muestran preferencia por un modelo concreto. Durante su desarrollo usamos Claude Sonnet 4.5 y por eso lo incluimos en estas instrucciones. Esperamos que las sugerencias de código sean relativamente coherentes y ofrezcan una buena experiencia. Sin embargo, como los LLM son probabilísticos, puede que las sugerencias difieran de las que se muestran en el taller. Es normal y esperable.

> [!NOTE]
> Debido a la naturaleza probabilística de los LLM, Copilot puede usar un comando MCP diferente y aun así completar la tarea.

3. Pide a Copilot que muestre el backlog enviándole este prompt:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para ejecutar el comando que enumera todas las incidencias.
5. Revisa la lista generada.

Observa que Copilot también ha priorizado los elementos según cuáles podrían resultar más útiles para el usuario.

## Revisa los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que proporcionará contexto a Copilot. Usarás el archivo de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funciones al sitio web.

1. En el codespace, ve a `.github/instructions/ui.instructions.md`.
2. Observa las directrices generales para añadir funciones. Incluyen:
   - una descripción general de la arquitectura;
   - principios de diseño de componentes, capacidad de prueba y accesibilidad;
   - enlaces a archivos de instrucciones para varios tipos de archivo, incluidos Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden incluir referencias a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas a la ubicación del archivo de instrucciones. Así se pueden reutilizar instrucciones, dividir conjuntos complejos en partes más manejables e incluir ejemplos y plantillas.

## Implementa el filtrado

Para completar el filtrado, tendrás que realizar al menos tres actualizaciones independientes en la aplicación:

- añadir o ajustar la lógica restante de filtrado en la capa de datos (`src/lib/`);
- añadir o actualizar las pruebas del comportamiento del filtrado;
- actualizar la página de lista de juegos para incluir la interfaz de filtrado.

Además, las pruebas deben ejecutarse y superarse antes de fusionar los cambios. El modo agente de Copilot puede realizar estas tareas. Añadamos la función.

1. Puedes continuar la conversación actual con Copilot o empezar otra seleccionando **New Chat**.
2. Selecciona **Add Context**, **Instructions** y **ui** para añadir el archivo de instrucciones.

   ![Captura de cómo seleccionar el archivo de instrucciones de interfaz de usuario](../../../_images/ex3-select-instructions-file.png)

3. Comprueba que **Agent** siga seleccionado en el menú desplegable de agentes de la vista Chat.

   ![Captura del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Comprueba que **Claude Sonnet 4.5** siga seleccionado como modelo.
5. Pide a Copilot que implemente la función según la incidencia relacionada del backlog con este prompt:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos pertinentes. Debería encontrar tanto los asistentes de la capa de datos como la interfaz y las pruebas. Después, modificará los archivos y ejecutará las pruebas.

   ![Captura de Copilot mientras explora los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas, como explorar el proyecto, modificar archivos y ejecutar pruebas. Según la complejidad de la tarea y del código base, el proceso puede tardar unos minutos. Mientras trabaja, puede que aparezcan los botones **Keep** y **Undo** en el editor. Cuando termine, tendrás la opción de aceptar o deshacer todos los cambios; no es necesario seleccionarlos mientras el trabajo siga en curso.

7. Cuando Copilot te lo pida, selecciona **Continue** para ejecutar las pruebas.

   ![Captura de un cuadro de diálogo de Copilot Chat que pide permiso para ejecutar pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Es posible que haya pausas y que algunas pruebas fallen durante el proceso. No pasa nada: Copilot alterna entre generar código y ejecutar pruebas hasta completar la tarea y no detectar errores.

   ![Captura de una sesión completa de Copilot Chat en modo agente](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explora el código generado y busca posibles problemas.

> [!CAUTION]
> Recuerda que siempre es importante revisar el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador donde se está ejecutando el sitio y explora la nueva función.
11. Cuando hayas confirmado que todo funciona y revisado el código, selecciona **Keep** en la ventana de Copilot Chat.

## Resumen y siguientes pasos

Enhorabuena. En este ejercicio has aprendido a usar el modo agente de GitHub Copilot para añadir funciones al sitio web Tailspin Toys. Has aprendido a:

- usar el modo agente de GitHub Copilot para implementar funciones en la interfaz de usuario y la capa de datos;
- dejar que Copilot explore el proyecto, identifique los archivos pertinentes y coordine los cambios;
- revisar los cambios y las pruebas generadas antes de fusionarlos en el código base.

Ahora [probarás la función con el servidor Playwright MCP][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio de exploración adicional: implementar la paginación

A medida que aumente la lista de juegos, será necesario habilitar la paginación. Usa lo aprendido en este ejercicio y pide a Copilot que añada paginación al sitio. Al planificar el código:

- sigue las prácticas recomendadas existentes, incluidos los archivos de instrucciones;
- decide cómo implementar la paginación y si quieres permitir que el usuario seleccione el tamaño de página o fijarlo;
- redacta un prompt que proporcione a Copilot las instrucciones necesarias para crear la implementación que necesitas;
- puede que tengas que iterar con GitHub Copilot, solicitar cambios y proporcionar contexto. Ese es el flujo habitual de trabajo con Copilot.

## Recursos

- [Modos Ask, Edit y Agent de Copilot: qué hacen y cuándo se usan][choose-mode]
- [Modo agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode

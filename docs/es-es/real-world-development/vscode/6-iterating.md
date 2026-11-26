---
title: "Ejercicio 6: Iteración sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
next: false
---

| [← Lección anterior: Supervisión y gestión de agentes][previous-lesson] |
|:--|

## Revisa el trabajo

Durante este laboratorio has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario. Has usado el modo agente para añadir filtros en el cliente y el servidor, el servidor MCP de Playwright para probar manualmente ese trabajo en un navegador y, después, un agente personalizado para implementar funciones de accesibilidad —controles de alto contraste y modo claro—. También has guiado la sesión en curso para ampliar el trabajo. Ahora toca publicar esos cambios locales y revisarlos como lo haría el equipo.

### Escenario

Los fundamentos del diseño de software y DevOps no cambian por incorporar IA generativa. Sigue siendo necesario revisar lo que produce Copilot. En esta lección enviarás los cambios de accesibilidad desde el codespace, abrirás una solicitud de incorporación de cambios y examinarás las diferencias antes de involucrar al resto del equipo.

## Publica las funciones de accesibilidad

Los controles de alto contraste y modo claro que has implementado con el agente personalizado de accesibilidad en el [ejercicio 4][exercise-4] y el [ejercicio 5][exercise-5] están en el codespace como cambios confirmados mediante commits. Enviémoslos a una rama y abramos una solicitud de incorporación de cambios para que el equipo pueda revisarlos.

1. Vuelve al codespace.
2. Abre la vista **Source Control** en VS Code.
3. Comprueba que los cambios de accesibilidad estén incluidos en un commit. Si tienes cambios sin confirmar del ejercicio 5, añádelos al área de preparación y crea un commit con un mensaje descriptivo, como `Add high-contrast and light-mode toggles`.
4. Publica la rama seleccionando **Publish Branch** o utiliza el menú **...** y selecciona **Push**.
5. VS Code te ofrecerá abrir la rama nueva en github.com. Acepta la propuesta o ve manualmente al repositorio y selecciona **Compare & pull request** en el aviso de la rama.
6. Escribe un título claro, como `Add high-contrast and light-mode toggles`, y una descripción breve que resuma qué se ha hecho y por qué.
7. Selecciona **Create pull request**.
8. Cuando se abra la PR, selecciona **Files changed** para revisar todo el trabajo. Presta especial atención a:
   - los componentes de la interfaz que permiten alternar entre modos;
   - el uso del almacenamiento local para conservar las preferencias del usuario;
   - los cambios de CSS o estilos para los modos de alto contraste y claro;
   - los atributos de accesibilidad, como las etiquetas ARIA y la navegación con teclado;
   - el código JavaScript o TypeScript que controla el cambio de modo.

9. Vuelve a la pestaña **Conversation**.
10. Si hay flujos de trabajo pendientes de aprobación, selecciona **Approve and run workflows**.

    ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

11. Espera a que terminen los flujos de trabajo. Si todo va bien, deberían completarse correctamente.

> [!TIP]
> ¿Quieres una segunda opinión sobre el trabajo de accesibilidad? Menciona a `@copilot` en un comentario de la PR con una petición como «revisa esta PR para detectar otros problemas de WCAG» o «sugiere mejoras en la navegación con teclado». Copilot iniciará una sesión nueva para responder al comentario.

## Ejercicio opcional: sigue explorando localmente

Trabajar de forma iterativa con un agente del IDE es una habilidad que solo se desarrolla con la práctica. Puedes probar estas ideas en futuras sesiones de VS Code:

- Añadir un formulario de interés para patrocinadores a la página de detalles del juego.
- Implementar la paginación en la página de la lista de juegos.
- Añadir validación de entradas y gestión de errores a las funciones auxiliares de acceso a datos de `src/lib/`.
- Ampliar el alcance del agente de accesibilidad; por ejemplo, revisar el orden del foco del teclado en todo el sitio.

## Resumen

Enhorabuena. Has completado el itinerario de VS Code. Durante el laboratorio:

- **Has usado Playwright MCP para probar manualmente la funcionalidad.** Has añadido el servidor Playwright MCP y has dejado que Copilot controlase un navegador para verificar el filtrado antes de abrir una solicitud de incorporación de cambios.
- **Has guiado el modo agente para realizar cambios coordinados en distintas capas.** En una sesión has añadido una función de filtrado que afecta al cliente, al servidor y a las pruebas.
- **Has usado un agente personalizado.** Has seleccionado el agente de accesibilidad en el selector de agentes y has observado cómo implementaba el modo de alto contraste en el repositorio.
- **Has gestionado y guiado una sesión de agente.** Has revisado los cambios propuestos en el editor, aceptado los que querías y ampliado la sesión con una petición de modo claro.
- **Has completado el flujo con una solicitud de incorporación de cambios.** Has publicado el trabajo local y lo has revisado de principio a fin como lo haría el equipo.

## Revisión y siguientes pasos

Aquí termina el itinerario obligatorio de VS Code. Puedes detenerte aquí: has completado el taller.

Si quieres ampliar tu experiencia con las funciones de agente de Copilot, los demás itinerarios cubren escenarios relacionados en otras interfaces:

- 💻 **[Itinerario de CLI](../../cli/)**: sigue flujos de trabajo parecidos desde el terminal con Copilot CLI, incluido el modo Plan, las habilidades de agente, los agentes personalizados y comandos slash como `/share`, `/context` y `/delegate`.
- ☁️ **[Itinerario del agente en la nube](../../cloud/)**: céntrate en asignar incidencias al agente en la nube, supervisar sesiones desde la página de agentes e iterar de forma asíncrona sobre solicitudes de incorporación de cambios.

También puedes seguir ampliando lo que has empezado aquí. [awesome-copilot][awesome-copilot] es una buena fuente de archivos de instrucciones, agentes personalizados y habilidades que puedes adaptar a tus proyectos.

Como ampliación opcional, [Opcional: Integra Foundry][exercise-7] usa VS Code y Microsoft Foundry Toolkit para preparar un modelo, implementar Backer Concierge y conectarlo al sitio.

## Recursos

- [GitHub Copilot][github-copilot]
- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Uso del modo agente][agent-mode]

---

| [← Lección anterior: Gestión de agentes][previous-lesson] |
|:--|

[previous-lesson]: ../5-managing-agents/
[exercise-4]: ../4-custom-agents/
[exercise-5]: ../5-managing-agents/
[exercise-7]: ../7-foundry-toolkit/
[github-copilot]: https://github.com/features/copilot
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
[awesome-copilot]: https://github.com/github/awesome-copilot

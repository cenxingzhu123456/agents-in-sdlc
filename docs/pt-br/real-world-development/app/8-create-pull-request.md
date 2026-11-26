---
title: "Lição 8 - Criar e fazer merge do PR do recurso"
description: "Revise em conjunto a filtragem, as instruções, a atualização da skill, o perfil de QA e os testes; depois, crie um PR e use o Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

A implementação da filtragem, as atualizações das instruções, a atualização da skill, o perfil de garantia de qualidade (QA) e os testes estão salvos em uma única branch. Agora é hora de revisar tudo em conjunto e abrir um pull request. Você fez o merge do pull request (PR) de avaliações por estrelas por conta própria; desta vez, permitirá que o **Agent Merge** gerencie o processo.

> [!NOTE]
> Normalmente, dividiríamos o recurso, as atualizações das instruções, a atualização da skill e o agente de QA em alguns PRs separados. Para simplificar o workshop, você manteve todo o fluxo de filtragem e qualidade em uma sessão e branch, e todo esse trabalho fará parte deste PR.

Nesta lição, você vai:

- aprender o que é o Agent Merge e como ele automatiza o ciclo de merge.
- inspecionar o PR completo do recurso e as evidências de verificação.
- autorizar o Agent Merge somente depois da revisão e confirmar que o PR foi mesclado.

## Cenário

Ao longo do fluxo de filtragem, você usou o Copilot para planejar, implementar e verificar um recurso. Agora a Tailspin Toys quer automatizar o trabalho restante do PR, mantendo a autorização do merge sob o controle da pessoa desenvolvedora.

## Conheça o Agent Merge

O **Agent Merge** automatiza o trabalho restante para integrar um pull request no aplicativo GitHub Copilot. Quando você o habilita, a sessão do aplicativo lê o pull request e resolve o que estiver bloqueando o merge — corrige verificações de integração contínua (CI) com falha, responde aos comentários da revisão e faz rebase quando necessário — e faz o merge assim que o GitHub permite. Ele trabalha em segundo plano, continua funcionando depois que o aplicativo é reiniciado e é desativado automaticamente quando o pull request é mesclado.

Até agora, você mesmo selecionou **Merge pull request**. O Agent Merge pode assumir essa responsabilidade, mas sua capacidade de editar código e fazer merge ainda exige sua autorização explícita. Revise as ações permitidas e o trabalho antes de conceder permissão para fazer merge.

## Use o Agent Merge para gerenciar o PR

Com todo o código criado e revisado, vamos permitir que o Agent Merge gerencie o processo do PR.

1. Use o seletor de agentes para selecionar **Default agent**.
2. Selecione o menu suspenso ao lado de **Create PR**.
3. Selecione **Agent Merge**. O botão mudará para **Agent Merge**.
4. Selecione o botão **Agent Merge** para iniciar o processo. Selecionar a opção no menu suspenso, por si só, não inicia o processo.

O processo do Agent Merge começa. Ele vai:

- criar o pull request com título e descrição.
- se a sessão tiver começado a partir de uma issue, incluir uma referência à issue relacionada no corpo da descrição.
- fazer rebase ou tratar possíveis conflitos de merge com a branch de destino.
- acompanhar o processo de integração contínua para garantir que todas as verificações sejam aprovadas.
- acompanhar o PR para verificar comentários de outras pessoas desenvolvedoras ou da revisão de código do Copilot e fazer atualizações para resolvê-los.
- opcionalmente, fazer merge automático do PR quando tudo estiver concluído com sucesso.

Vamos permitir que o Agent Merge também faça o merge do PR depois que tudo for aprovado!

5. Revise o diff do PR e as evidências de verificação antes de conceder permissão para fazer merge.
6. Selecione o menu suspenso ao lado de **Agent Merge**.
7. Confirme se há uma marca ao lado de **Merge pull request**.
8. Peça explicitamente ao agente que faça o merge:

    ```plaintext
    Faça o merge do PR quando todas as verificações e aprovações obrigatórias forem concluídas. Não ignore as proteções do repositório.
    ```

9. Acompanhe o progresso e resolva os bloqueios relatados. Antes de continuar, confirme que o status do PR é **Merged**; a permissão de merge habilitada ou um agente em execução não significa que o merge foi concluído.

> [!IMPORTANT]
> O Agent Merge não ignora as proteções do repositório nem as permissões ausentes. Resolva esses bloqueios antes de continuar.

## Resumo e próximos passos

Você automatizou várias etapas do processo de desenvolvimento, incluindo gerar código, testar e validar o código e, agora, o processo do pull request. Você:

- aprendeu o que é o Agent Merge e como ele automatiza o ciclo de merge.
- inspecionou o PR completo do recurso e as evidências de verificação.
- autorizou o Agent Merge somente depois da revisão e confirmou que o PR foi mesclado.

Em seguida, você [usará um canvas existente e criará um canvas de triagem][next-lesson] para explorar uma forma mais completa de inspecionar, planejar e visualizar o trabalho com o agente.

## Recursos

- [Gerenciar issues e pull requests com o aplicativo GitHub Copilot][managing-issues-prs]
- [Sobre o aplicativo GitHub Copilot][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app

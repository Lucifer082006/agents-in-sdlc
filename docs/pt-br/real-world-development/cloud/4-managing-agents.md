---
title: "Exercício 4 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Nos últimos exercícios, você pediu ao agente de nuvem do Copilot que executasse três tarefas distintas voltadas à melhoria da experiência do usuário e à adição de funcionalidades. Embora o agente de nuvem tenha sido criado para trabalhar de forma assíncrona e autônoma, também é importante poder monitorar essas tarefas.

Há várias ferramentas para gerenciar tarefas atribuídas ao agente de nuvem, incluindo a [página de agentes][agents-page] no GitHub.com. Nesse Mission Control, você pode ver todas as tarefas dos agentes que têm pull requests (PRs) abertos. É possível explorar as operações realizadas e até orientar uma sessão em andamento.

Nesta lição, você vai:

- explorar a página de agentes para monitorar as tarefas do agente de nuvem.
- orientar uma sessão em andamento e solicitar funcionalidades adicionais.

## Cenário

Depois de atribuir ao agente a criação de um modo de alto contraste, a equipe percebeu que seria um bom momento para adicionar também um modo claro. Como o trabalho já atualizava o estilo do site e adicionava funcionalidade de alternância, pareceu lógico incluir esse recurso. Você orientará o agente para garantir que ele adicione o modo claro além do alto contraste.

## Revisar as tarefas do agente de nuvem do Copilot

Vamos verificar o status atual de todas as tarefas atribuídas ao agente de nuvem do Copilot.

1. Acesse a [página de agentes do Copilot](https://github.com/copilot/agents).
2. Observe a lista de tarefas no painel principal e no painel esquerdo. Você deverá ver as tarefas atribuídas ao Copilot, incluindo:
   - atualização da documentação da base de código.
   - geração de APIs para modificar jogos.
   - adição de um modo de alto contraste ao site.
3. Selecione uma das tarefas em execução. Revise as operações realizadas pelo Copilot, que podem incluir:
   - obter o código do repositório.
   - criar o ambiente de trabalho do Copilot.
   - configurar servidores MCP.
   - executar diversas etapas para concluir a tarefa atribuída.

> [!NOTE]
> As etapas exibidas variam conforme o estado do trabalho do Copilot e a abordagem adotada.

4. Observe também o painel de pull request (PR) à direita. Ele permite acompanhar o PR e os arquivos alterados.

## Orientar o agente de nuvem

Agora que você viu as tarefas ativas, vamos pedir ao Copilot que inclua a alternância para o modo claro enquanto trabalha no modo de alto contraste.

1. Selecione a sessão referente à adição do modo de alto contraste. O título exato varia conforme o nome escolhido pelo Copilot e o estado atual do trabalho.

    ![Sessão de acessibilidade no Mission Control](../../../_images/ex6-accessibility-session.png)

2. Acompanhe a sessão por alguns minutos, até que ela indique que a configuração terminou e o trabalho começou. Isso ficará claro quando aparecerem mensagens semelhantes às mostradas abaixo.
3. Na caixa de diálogo **Steer active session while Copilot is working**, adicione o prompt a seguir:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Captura da tarefa do agente de nuvem na página de agentes, com a caixa de diálogo para orientar a sessão ativa destacada](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pressione <kbd>Enter</kbd> para enviar o prompt.
5. Observe que o Copilot reconhece o prompt e o inclui no fluxo de trabalho.

## Deixar o Copilot trabalhar

Como antes, o Copilot trabalhará na tarefa atualizada. Ele incorporará a nova solicitação ao fluxo depois de concluir a etapa em andamento no momento em que você enviou a mensagem.

Como isso também levará alguns minutos, aproveite para refletir sobre o que aprendeu e explorou até agora.

## Resumo e próximos passos

Esta lição explorou a página de agentes do Copilot, o ponto central para monitorar e orientar tarefas do agente de nuvem do GitHub Copilot. Nela, você pode acompanhar todas as tarefas ativas e concluídas, revisar o trabalho e até redirecionar tarefas em andamento para ajustar o escopo ou dar instruções adicionais.

Você explorou estes conceitos:

- explorou o Mission Control e a página de agentes para monitorar tarefas do agente de nuvem.
- redirecionou uma sessão em andamento para solicitar funcionalidades adicionais.

Enquanto o Copilot conclui o trabalho nos recursos de acessibilidade, vamos [iterar nos pull requests criados pelo Copilot][next-lesson].

## Recursos

- [Página de agentes do Copilot][agents-page]
- [Sobre agentes personalizados][custom-agents]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar no trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents

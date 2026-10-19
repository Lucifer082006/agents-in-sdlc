---
title: "Exercício 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

[Agentes personalizados][custom-agents-concept] no GitHub Copilot permitem criar assistentes de IA especializados e adaptados a tarefas ou domínios específicos do fluxo de desenvolvimento. Ao definir agentes com arquivos Markdown na pasta `.github/agents` do repositório, você oferece ao Copilot instruções específicas, práticas recomendadas, padrões de programação e conhecimento de domínio para que ele execute certos tipos de trabalho com mais eficácia. As equipes podem transformar sua experiência em agentes reutilizáveis — por exemplo, um agente de acessibilidade que aplica a conformidade com [WCAG][wcag], um agente de segurança que segue práticas seguras de programação ou um agente de testes que mantém padrões consistentes.

Os agentes personalizados são definidos por arquivos Markdown na pasta `.github/agents` do projeto ou globalmente em `~/.copilot/agents`. Cada arquivo contém frontmatter YAML com pelo menos `name` e `description`, seguido de um prompt Markdown que define o comportamento, a experiência e as instruções do agente.

### Agentes personalizados e skills de agente

Há alguma sobreposição conceitual entre agentes personalizados e [skills de agente][agent-skills-concept]. Ambos são definidos principalmente em arquivos Markdown e instruem uma IA sobre como executar operações. A distinção mais simples é: um **agente personalizado** é quem executa o trabalho; as **skills** são as ferramentas.

Os agentes personalizados têm sua própria janela de contexto e foram criados para orquestrar skills (e até outros agentes) durante o trabalho. Neste laboratório, o agente personalizado de acessibilidade revisa e atualiza o site conforme as diretrizes de acessibilidade; durante esse processo, ele pode invocar skills, como uma para o fluxo de trabalho de pull requests ou outra para executar e gerenciar testes.

> [!NOTE]
> Não existe uma única forma correta de criar um agente personalizado. Como acontece com qualquer recurso de IA, teste e itere para encontrar o que funciona nos seus ambientes e cenários.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Cenário

A Tailspin Toys está comprometida em tornar sua plataforma de financiamento coletivo acessível a todas as pessoas usuárias, independentemente da visão ou das preferências visuais. Comentários recentes indicaram que algumas pessoas têm dificuldade para ler o tema escuro atual devido ao contraste insuficiente entre as cores do texto e do plano de fundo. Para resolver essa questão de acessibilidade, a equipe de design solicitou a implementação de um modo de alto contraste que possa ser ativado e desativado.

Como a acessibilidade é fundamental, você quer implementar isso o mais rápido possível. Para gerar a funcionalidade, usará um agente personalizado.

Neste exercício, você aprenderá a:

- revisar um agente personalizado de acessibilidade existente.
- usar o agente de acessibilidade no Copilot Chat para implementar o modo de alto contraste.

## Revisar o agente personalizado de acessibilidade

Um agente personalizado para acessibilidade já foi criado. Vamos revisar o conteúdo para entender como ele orientará o Copilot.

Volte ao Codespace e abra um terminal. Crie uma branch nova a partir de `main` para o trabalho de acessibilidade (o PR de filtragem do Exercício 3 permanecerá separado):

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abra `.github/agents/accessibility.md`.
2. Observe o frontmatter YAML com os campos `name` e `description`.

> [!CAUTION]
> Os campos `name` e `description` no frontmatter são obrigatórios para agentes personalizados.

3. Examine as próximas seções, que destacam:
   - responsabilidades centrais ao gerar código para um site acessível.
   - práticas recomendadas de acessibilidade.
   - exemplos de código HTML, CSS e JavaScript.
   - uma lista de erros e problemas comuns.

## Usar o agente personalizado no Copilot Chat

O VS Code exibe todos os agentes personalizados definidos em `.github/agents` no seletor de agentes, na parte inferior da visualização do Copilot Chat. Você pode selecionar um agente personalizado para aplicar as instruções e ferramentas dele ao escopo da sessão de chat.

> [!TIP]
> **Abrir o Copilot Chat**
>
> Antes de começar os exercícios abaixo, volte ao Codespace, abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam conforme o exercício — cada etapa indica as opções relevantes.

1. Selecione **Agent** no seletor de agentes da visualização Chat, caso ainda não esteja selecionado.

   ![Captura de tela do seletor de agentes na visualização Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecione o seletor de agentes na parte inferior da visualização Chat (ele mostra o agente ativo, que por padrão é **default**).
3. Selecione **Accessibility agent** na lista de agentes disponíveis.
4. Envie este prompt ao agente de acessibilidade:

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. O Copilot Chat começará a trabalhar: explorará a base de código, proporá alterações e editará os arquivos do projeto. Cada edição aparecerá em linha no chat, com o caminho do arquivo e um diff para revisão.
6. Conforme as edições forem aplicadas, o indicador **Files changed** será atualizado para mostrar o conjunto de arquivos alterados pelo agente.

> [!NOTE]
> O processo provavelmente levará alguns minutos. O Copilot está fazendo alterações reais no repositório — editará arquivos existentes, como componentes Astro, CSS e testes relacionados.

Você revisará e orientará esse trabalho em andamento no próximo exercício.

## Resumo e próximos passos

Esta lição explorou [agentes personalizados][custom-agents] no GitHub Copilot, assistentes de IA especializados para tarefas e domínios específicos. Com agentes personalizados, você pode transformar a experiência e os padrões da equipe em agentes reutilizáveis, que orientam o Copilot para executar certos tipos de trabalho com mais eficácia.

Você explorou estes conceitos:

- como os agentes personalizados são definidos.
- como usar um agente personalizado no modo de agente do Copilot Chat.

Em seguida, você [monitorará e orientará o trabalho do agente][next-lesson], revisando as alterações enquanto são feitas e adicionando uma alternância para o modo claro à mesma sessão.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Criar agentes personalizados no IDE][creating-custom-agents-ide]
- [Agentes personalizados no VS Code][custom-agents-vscode]
- [Configuração de agentes personalizados][custom-agents-config]
- [Agentes personalizados no awesome-copilot][awesome-copilot-agents]

---

| [← Lição anterior: Testar o recurso com o servidor MCP do Playwright][previous-lesson] | [Próxima lição: Monitorar e gerenciar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents

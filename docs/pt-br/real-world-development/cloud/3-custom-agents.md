---
title: "Exercício 3 - Agentes personalizados"
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

Você explorará os seguintes tópicos com agentes personalizados:

- como os agentes personalizados são definidos.
- como atribuir uma tarefa a um agente personalizado.

## Cenário

A Tailspin Toys está comprometida em tornar sua plataforma de financiamento coletivo acessível a todas as pessoas usuárias, independentemente da visão ou das preferências visuais. Comentários recentes indicaram que algumas pessoas têm dificuldade para ler o tema escuro atual devido ao contraste insuficiente entre as cores do texto e do plano de fundo. Para resolver essa questão de acessibilidade, a equipe de design solicitou a implementação de um modo de alto contraste que possa ser ativado e desativado.

Como a acessibilidade é fundamental, você quer implementar isso o mais rápido possível. Para gerar a funcionalidade, usará um agente personalizado.

## Revisar o agente personalizado de acessibilidade

Um agente personalizado para acessibilidade já foi criado. Vamos revisar o conteúdo para entender como ele orientará o Copilot.

Volte ao Codespace e abra o arquivo do agente personalizado de acessibilidade:

1. Abra `.github/agents/accessibility.md`.
2. Observe o frontmatter YAML com os campos `name` e `description`.

> [!CAUTION]
> Os campos `name` e `description` no frontmatter são obrigatórios para agentes personalizados.

3. Examine as próximas seções, que destacam:
   - responsabilidades centrais ao gerar código para um site acessível.
   - práticas recomendadas de acessibilidade.
   - exemplos de código HTML, CSS e JavaScript.
   - uma lista de erros e problemas comuns.

## Criar e atribuir uma issue

O Mission Control é o local central para trabalhar com agentes no seu ambiente. Você pode atribuir tarefas ao agente de nuvem do Copilot, monitorá-las e até redirecionar ou fornecer orientações adicionais. Vamos começar atribuindo ao Copilot a tarefa de criar o modo de alto contraste.

1. Acesse seu repositório.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue** para criar uma issue em branco.
5. Defina **Title** como `Add high contrast mode to website`.
6. Defina **Description** como:

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.
9. Selecione **Accessibility agent** na lista de agentes personalizados.

    ![Captura de tela da atribuição do agente de nuvem, com o agente personalizado e Accessibility destacados](../../../_images/ex5-select-custom-agent.png)

10. Selecione **Assign**.
11. O Copilot começará a trabalhar na tarefa em segundo plano.

## Resumo e próximos passos

Esta lição explorou [agentes personalizados][custom-agents] no GitHub Copilot, assistentes de IA especializados para tarefas e domínios específicos. Com agentes personalizados, você pode transformar a experiência e os padrões da equipe em agentes reutilizáveis, que orientam o Copilot para executar certos tipos de trabalho com mais eficácia.

Você explorou estes conceitos:

- como os agentes personalizados são definidos.
- como atribuir uma tarefa a um agente personalizado.

Enquanto o Copilot trabalha para implementar o modo de alto contraste, vamos [monitorar e orientar a sessão do agente][next-lesson] pelo Mission Control.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Preparar-se para usar agentes personalizados na organização][org-custom-agents]
- [Preparar-se para usar agentes personalizados na empresa][enterprise-custom-agents]

---

| [← Lição anterior: Agente de nuvem do GitHub Copilot][previous-lesson] | [Próxima lição: Monitorar e gerenciar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents

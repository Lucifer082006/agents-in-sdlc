---
title: "Exercício 2 - Agente de nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Instruções personalizadas][previous-lesson] |
|:--|

É provável que poucas organizações não enfrentem dificuldades com dívida técnica. Ela pode incluir problemas de segurança não resolvidos, código legado que precisa de atualizações ou solicitações de recursos esquecidas no backlog por falta de tempo para implementá-las. O agente de nuvem do GitHub Copilot foi criado para executar tarefas como atualizar código e adicionar funcionalidades de forma autônoma. Quando termina o trabalho, ele gera um PR de rascunho para revisão humana. Assim, é possível delegar tarefas tediosas, acelerar o desenvolvimento e liberar as pessoas desenvolvedoras para se concentrarem em questões mais amplas.

Você explorará os seguintes tópicos com o agente de nuvem do Copilot:

- personalizar o ambiente de geração de código.
- garantir que as operações sejam realizadas com segurança.
- entender a importância de issues bem delimitadas.
- atribuir issues ao Copilot.

## Cenários

A Tailspin Toys tem algumas dívidas técnicas que deseja resolver. As pessoas terceirizadas contratadas inicialmente para criar a primeira versão do site deixaram a documentação em um estado inadequado — na verdade, ela está completamente ausente. Como primeiro passo, a equipe quer adicionar comentários TSDoc a todas as funções exportadas no aplicativo.

Além disso, a equipe de design quer melhorar a descoberta de jogos. Ela gostaria que a página de detalhes de cada jogo mostrasse jogos relacionados — outros títulos da mesma categoria — para que as pessoas possam continuar navegando. Ainda não é necessária uma implementação refinada; a equipe quer apenas algo para testar a experiência do usuário. Isso está bloqueado, mas há issues de prioridade maior no momento.

Esses são exemplos de tarefas que podem ser adiadas com facilidade e que são boas candidatas para o agente de nuvem do Copilot. Ele pode trabalhar nelas de forma assíncrona enquanto a pessoa desenvolvedora se ocupa de outras tarefas e, depois, volta para revisar o trabalho e confirmar se está tudo certo.

## Introdução ao agente de nuvem do GitHub Copilot

O [agente de nuvem do GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) pode executar tarefas em segundo plano, de forma semelhante a uma pessoa desenvolvedora. Assim como acontece ao trabalhar com uma pessoa, há várias maneiras de delegar trabalho, como [atribuir uma issue do GitHub ao Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Depois da atribuição, o Copilot cria um pull request de rascunho para acompanhar o progresso, configura um ambiente e começa a trabalhar na tarefa. Você pode acompanhar a sessão enquanto ela está em andamento ou depois de concluída. Quando a solução proposta estiver pronta para revisão, o Copilot mencionará você no pull request.

## A importância de instruções bem delimitadas

Não há mágica no GitHub Copilot — não dá para pular a etapa de pensar. Mesmo operações aparentemente simples ganham complexidade quando analisadas em profundidade; por isso, [delimite cuidadosamente as tarefas para o agente de nuvem do Copilot][cloud-agent-best-practices]. Trate-o como uma pessoa parceira de programação com IA: trabalhe em etapas, aprenda, experimente e adapte o processo. Os fundamentos do desenvolvimento de software não mudam com a inclusão de IA generativa.

## Instruções personalizadas neste repositório

Os exercícios anteriores apresentaram as instruções personalizadas e como elas orientam o Copilot. Se você já fez o exercício prático de instruções personalizadas, viu `.github/copilot-instructions.md` em ação; caso contrário, este é um bom momento para examiná-lo rapidamente. Antes de atribuir trabalho ao agente de nuvem do Copilot, leia os arquivos de instruções já incluídos neste repositório para depois identificar seus efeitos.

Abra os seguintes arquivos na interface Web do GitHub ou em um Codespace, caso já tenha iniciado um:

- `.github/copilot-instructions.md` — revise a seção **Code standards**, especialmente as expectativas para convenções de TypeScript e comentários TSDoc.
- `.github/instructions/unit-tests.instructions.md` — observe o frontmatter `applyTo`, que aplica estas instruções aos arquivos `**/*.test.ts`.

Ao atribuir a issue `Code lacks documentation` ao agente de nuvem no próximo exercício, observe os comentários TSDoc, as convenções de TypeScript e os cabeçalhos de comentários no pull request resultante — essas orientações vêm dos arquivos de instruções. Voltaremos a isso ao revisar os PRs em uma lição posterior.

## Configurar o ambiente de desenvolvimento do agente de nuvem do Copilot

Desenvolver código normalmente exige um ambiente específico e a execução de scripts de configuração para garantir que tudo esteja em boas condições. Isso também se aplica às tarefas atribuídas ao Copilot, que trabalha de forma semelhante a uma pessoa desenvolvedora de software.

O agente de nuvem usa o [GitHub Actions][github-actions] como ambiente de trabalho. Você pode personalizar esse ambiente criando um [workflow especial de configuração][setup-workflow], definido no arquivo `.github/workflows/copilot-setup-steps.yml` e executado antes do início do trabalho. Isso permite disponibilizar as ferramentas e dependências de desenvolvimento necessárias. O arquivo foi pré-configurado para facilitar o laboratório e permitir este aprendizado. Ele garante ao Copilot acesso ao Node.js, às dependências do projeto, aos binários do navegador para testes de ponta a ponta e ao banco de dados SQLite migrado e populado para o único aplicativo Astro:

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas de configuração na guia "Actions" do repositório
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Defina as permissões no nível mais restrito necessário para *suas etapas*. O Copilot receberá um token próprio para suas operações.
    permissions:
      # Se quiser clonar o repositório como parte das etapas de configuração, por exemplo para instalar dependências, será necessária a permissão `contents: read`. Se não clonar o repositório nessas etapas, o Copilot fará isso automaticamente depois que elas forem concluídas.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do front-end/aplicativo - Node.js (todo o aplicativo agora usa Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra e popula o banco de dados SQLite local para que as compilações e os testes tenham dados.
      - name: Set up the database
        run: npm run db:setup
```

O arquivo se parece com qualquer outro workflow do GitHub, mas tem alguns detalhes importantes:

- Ele contém um único job chamado `copilot-setup-steps`. Esse job é executado no GitHub Actions antes que o Copilot comece a trabalhar no pull request.
- Observe o gatilho `workflow_dispatch`, que permite executar o workflow manualmente pela guia **Actions** do repositório. Isso é útil para testar se o workflow funciona em vez de esperar que o Copilot o execute.

## Adicionar documentação

Embora todos reconheçam a importância da documentação, a maioria dos projetos tem informações desatualizadas ou não tem documentação alguma. Esse tipo de dívida técnica costuma permanecer sem solução, reduzindo a produtividade e dificultando a manutenção da base de código ou a integração de novas pessoas desenvolvedoras. Felizmente, o Copilot é muito bom em criar documentação, e esta é uma issue ideal para atribuir ao agente de nuvem. Ele trabalhará em segundo plano para gerar a documentação necessária. Em um exercício futuro, você voltará para revisar o resultado.

1. Em uma nova guia do navegador, acesse o repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue** para criar uma issue em branco.
5. Defina **Title** como `Code lacks documentation`.
6. Defina **Description** como:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

  ![Atribuindo uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

  ![Detalhes da atribuição ao Copilot](../../../_images/ex4-assign-copilot-details.png)

10. Selecione a guia **Pull Requests**.
11. Abra o novo pull request (PR), cujo título será semelhante a `[WIP]: Code lacks documentation`. Se o PR ainda não aparecer na lista, aguarde um ou dois minutos e atualize a página.
12. Depois de alguns minutos, o Copilot deverá criar uma lista de tarefas.

> [!NOTE]
> A lista de tarefas do Copilot pode levar vários minutos para aparecer no PR. O Copilot está criando o ambiente (executando o workflow descrito anteriormente), analisando o projeto e determinando a melhor abordagem para o problema.

13. Revise a lista e as tarefas que serão executadas.
14. Role a linha do tempo do pull request até encontrar uma atualização informando que o Copilot começou a trabalhar na issue.
15. Selecione o botão **View session**.

  ![Visualização da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a página para ver o indicador atualizado.

16. Observe que você pode percorrer a sessão ao vivo e acompanhar como o Copilot resolve o problema: ele explora o código, entende o estado do projeto, faz pausas para pensar e decidir a abordagem e cria código.

Isso provavelmente levará alguns minutos. Um dos principais objetivos do agente de nuvem do Copilot é executar tarefas de forma assíncrona, deixando você livre para se concentrar em outras atividades. Vamos aproveitar esse recurso atribuindo outra tarefa ao agente de nuvem e, em seguida, voltando nossa atenção para a criação de funcionalidades do aplicativo.

## Adicionar uma seção de jogos relacionados à página de detalhes

Como já destacamos, uma das grandes vantagens do agente de nuvem do GitHub Copilot é a possibilidade de dividir o trabalho: você pode se concentrar em um conjunto de tarefas enquanto ele se ocupa de outro. Talvez a seção de jogos relacionados solicitada pela equipe de design não leve muito tempo, mas esse tempo ainda pode ser usado em outras tarefas. Vamos atribuí-la ao agente de nuvem do Copilot.

1. Volte ao repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue** para usar o template em branco.
5. Defina **Title** como `Show related games on the game details page`.
6. Defina **Description** como:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

  ![Atribuindo uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

Logo depois, você verá alguns 👀 no primeiro comentário da issue, indicando que o Copilot começou a trabalhar.

![O Copilot usa o emoji de olhos para indicar que está trabalhando na issue](../../../_images/ex4-issue-eyes-emoji.png)

Agora o Copilot está trabalhando diligentemente na segunda solicitação. O agente de nuvem funciona de forma semelhante a uma pessoa desenvolvedora de software, então não é necessário acompanhá-lo ativamente; basta revisar o resultado quando estiver concluído. Vamos voltar nossa atenção à criação e ao uso de agentes personalizados.

## Resumo e próximos passos

Esta lição explorou o [agente de nuvem do GitHub Copilot][copilot-agents]. Com ele, você pode atribuir issues ao Copilot para que sejam realizadas de forma assíncrona. Você pode usar o Copilot para resolver dívidas técnicas, criar recursos ou ajudar a migrar código de uma estrutura para outra.

Você explorou estes conceitos:

- personalizar o ambiente de geração de código.
- garantir que as operações sejam realizadas com segurança.
- entender a importância de issues bem delimitadas.
- atribuir issues ao Copilot.

Enquanto o agente de nuvem trabalha em segundo plano, vamos criar e usar agentes personalizados. O [agente de nuvem do Copilot também pode usar servidores MCP][cloud-agent-mcp] e tem acesso a instruções personalizadas, que exploramos nos módulos anteriores.

## Recursos

- [Sobre o agente de nuvem do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do agente de nuvem do Copilot][cloud-agent-best-practices]

| [Próxima lição: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions

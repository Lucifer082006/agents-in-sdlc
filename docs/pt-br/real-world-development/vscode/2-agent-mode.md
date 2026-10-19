---
title: "Exercício 2 - Adicionar funcionalidade com o modo de agente do Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Até mesmo as atualizações mais simples de um aplicativo normalmente exigem alterações em vários arquivos e operações como executar testes. O fluxo de trabalho de uma pessoa desenvolvedora costuma envolver localizar todos os arquivos necessários, fazer as alterações, executar testes, depurar, descobrir qual arquivo foi esquecido, fazer outra atualização... e assim por diante.

É aí que entra o modo de agente do Copilot.

O modo de agente do Copilot foi criado para atuar com mais autonomia no IDE. Ele trabalha de forma semelhante a uma pessoa desenvolvedora: começa explorando a estrutura existente do projeto, faz as atualizações necessárias, executa tarefas como testes e corrige automaticamente os problemas encontrados. Vamos explorar como usar o modo de agente para adicionar novas funcionalidades ao site.

> [!NOTE]
> Apesar da semelhança entre os nomes, o modo de agente e o agente de nuvem oferecem experiências diferentes. O modo de agente executa tarefas no IDE, permitindo ciclos rápidos de feedback e interação. O agente de nuvem trabalha de forma assíncrona como uma pessoa da equipe, interagindo com você por meio de issues e pull requests.

Neste exercício, você aprenderá como:

- o modo de agente do Copilot pode explorar o projeto, identificar arquivos relevantes e coordenar alterações.
- o modo de agente do GitHub Copilot pode implementar novos recursos na interface e na camada de dados.
- revisar alterações e testes gerados pelo modo de agente antes de fazer merge na base de código.

## Cenário

À medida que a lista de jogos cresce, você quer permitir que as pessoas filtrem por categoria e distribuidora. No exercício anterior, você já adicionou um helper de distribuidoras; agora concluirá o restante do trabalho na camada de dados, na interface e nos testes com o modo de agente do Copilot.

## Executar o site da Tailspin Toys

Antes de fazer alterações, vamos explorar o site da Tailspin Toys para entender sua funcionalidade atual.

O site é uma plataforma de financiamento coletivo para jogos de tabuleiro com tema de desenvolvimento. Ele permite listar jogos e exibir detalhes sobre eles. O site é um único aplicativo Astro que gera páginas HTML estáticas durante a compilação. As páginas consultam diretamente um banco de dados SQLite local no frontmatter, por meio do Drizzle ORM — não há uma API de back-end separada nem uma estrutura de interface do lado do cliente. Os helpers reutilizáveis de acesso a dados ficam em `src/lib/`, e qualquer interatividade é adicionada com um pequeno elemento `<script>` do Astro, com escopo definido e usando as APIs padrão do DOM.

### Iniciar o site

Para facilitar a execução do site, há uma tarefa `npm run dev` que inicia o único servidor de desenvolvimento Astro. Execute-a no GitHub Codespace seguindo estas etapas:

1. Volte ao Codespace. Você deverá estar na branch `main` depois do exercício anterior.
2. Abra uma nova janela de terminal no Codespace pressionando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crie e alterne para uma branch para o trabalho de filtragem:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Execute o comando a seguir para iniciar o site:

   ```bash
   npm run dev
   ```

   Quando o servidor de desenvolvimento Astro estiver pronto, você verá uma mensagem com a URL, semelhante a esta:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Se aparecer uma caixa de diálogo solicitando que abra uma janela do navegador para `http://localhost:4321`, feche-a selecionando **x**.

5. Abra o site mantendo pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecionando o endereço do servidor Astro `http://localhost:4321` no terminal.

> [!NOTE]
> Em um Codespace, selecionar o link local no terminal redireciona automaticamente para `https://<your-codespace-name>-4321.app.github.dev/`. Esse é um túnel privado para o Codespace, que agora hospeda o servidor Web.

### Explorar o site

Depois que o site estiver em execução, você poderá explorar suas funcionalidades. Os principais recursos incluem:

- **Página inicial**: exibe uma lista de jogos de tabuleiro com títulos, imagens e descrições.
- **Página de detalhes do jogo**: ao selecionar um jogo, você acessa uma página com mais informações, incluindo título, descrição, distribuidora e categoria.

## Explorar o backlog com o Copilot

> [!TIP]
> **Abrir o Copilot Chat**
>
> Antes de começar os exercícios abaixo, volte ao Codespace, abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam conforme o exercício — cada etapa indica as opções relevantes.

A implementação inicial do site funciona, mas queremos aprimorá-la com novos recursos. Vamos começar examinando o backlog. Ao criar o projeto a partir do template, um backlog de issues foi criado automaticamente — peça ao GitHub Copilot que as exiba.

1. Selecione **Agent** no seletor de agentes da visualização Chat. O agente planeja e implementa alterações em vários arquivos de forma autônoma, executa comandos do terminal e invoca ferramentas.

   ![Captura de tela do seletor de agentes na visualização Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecione **Claude Sonnet 4.5** na lista de modelos disponíveis.

> [!CAUTION]
> Os autores deste workshop não estão indicando preferência por um modelo específico. Ao criar o workshop, usamos o Claude Sonnet 4.5 e incluímos esse modelo nas instruções para que as sugestões de código sejam relativamente consistentes e a experiência seja boa. No entanto, como os LLMs são probabilísticos, as sugestões podem ser diferentes das descritas aqui. Isso é normal e esperado.

> [!NOTE]
> Devido à natureza probabilística dos LLMs, o Copilot pode usar um comando MCP diferente, mas ainda assim deverá concluir a tarefa.

3. Envie o seguinte prompt para perguntar ao Copilot sobre o backlog de issues:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecione **Continue** para executar o comando que lista todas as issues.
5. Revise a lista gerada.

Observe como o Copilot priorizou os itens com base nos que considera mais úteis para as pessoas usuárias.

## Revisar os arquivos de instruções

Antes de iniciar o agente para gerar o código, examine o arquivo de instruções que fornecerá contexto ao Copilot. Você usará o arquivo de [interface do usuário (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que contém orientações para adicionar funcionalidades ao site.

1. No Codespace, acesse `.github/instructions/ui.instructions.md`.
2. Observe as orientações gerais para adicionar funcionalidades, incluindo:
   - uma visão geral da arquitetura.
   - princípios de design de componentes, capacidade de teste e acessibilidade.
   - links para arquivos de instruções específicos para diferentes tipos de arquivo, incluindo:
     - Astro.
     - Tailwind CSS.
     - Drizzle.

> [!TIP]
> Os arquivos de instruções permitem referenciar outros arquivos de instruções e arquivos do projeto. Os caminhos são relativos à localização do arquivo de instruções. Assim, é possível reutilizar orientações, dividir instruções complexas em partes menores e incluir exemplos e templates.

## Implementar a funcionalidade de filtragem

Para concluir a filtragem, serão necessárias pelo menos três alterações distintas no aplicativo:

- adicionar ou aprimorar a lógica restante da filtragem na camada de dados (`src/lib/`).
- adicionar ou atualizar testes do comportamento da filtragem.
- atualizar a página de listagem de jogos para incluir a interface de filtragem.

Além disso, os testes precisam ser executados e aprovados antes de fazer o merge na base de código. O modo de agente do Copilot pode executar essas tarefas. Vamos adicionar a funcionalidade.

1. Continue na conversa atual com o Copilot ou inicie outra selecionando **New Chat**.
2. Selecione **Add Context**, **Instructions** e **ui** como arquivo de instruções.

   ![Captura de tela que mostra como selecionar o arquivo de instruções de UI](../../../_images/ex3-select-instructions-file.png)

3. Confirme que **Agent** ainda está selecionado no seletor de agentes da visualização Chat.

   ![Captura de tela do seletor de agentes na visualização Chat.](../../../_images/shared-chat-mode-selector.png)

4. Confirme que **Claude Sonnet 4.5** continua selecionado como modelo.
5. Peça ao Copilot que implemente a funcionalidade com base na issue relacionada do backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Acompanhe o Copilot enquanto ele explora o projeto e localiza os arquivos relacionados à funcionalidade desejada. Ele deverá encontrar os helpers da camada de dados, a interface e os testes. Depois, começará a modificar os arquivos e executar os testes.

   ![Captura de tela do Copilot explorando os arquivos do projeto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> O Copilot executará várias tarefas, como explorar o projeto, modificar arquivos e rodar testes. Isso pode levar alguns minutos, dependendo da complexidade da tarefa e da base de código. Durante o processo, os botões **Keep** e **Undo** podem aparecer no editor de código. Quando o Copilot terminar, você terá a opção **Keep** ou **Undo** para todas as alterações; não é necessário selecioná-las enquanto o trabalho está em andamento.

7. Quando solicitado pelo Copilot, selecione **Continue** para executar os testes.

   ![Captura de tela de uma caixa de diálogo no painel Copilot Chat pedindo confirmação para executar testes](../../../_images/ex3-agent-mode-run-tests.png)

8. Talvez ocorram pausas e algumas falhas nos testes durante o processo. Tudo bem: o Copilot alterna entre gerar código e executar testes até concluir a tarefa sem detectar erros.

   ![Captura de tela de uma sessão completa do Copilot Chat no modo de agente](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explore o código gerado e procure possíveis problemas.

> [!CAUTION]
> Lembre-se: é sempre importante revisar o código gerado pelo Copilot ou por qualquer ferramenta de IA.

10. Volte ao navegador com o site em execução e explore a nova funcionalidade.
11. Depois de confirmar que tudo funciona e revisar o código, selecione **Keep** na janela Copilot Chat.

## Resumo e próximos passos

Parabéns! Neste exercício, exploramos como usar o modo de agente do GitHub Copilot para adicionar recursos ao site da Tailspin Toys. Aprendemos como:

- o modo de agente do GitHub Copilot pode implementar novos recursos na interface e na camada de dados.
- o modo de agente do Copilot pode explorar o projeto, identificar arquivos relevantes e coordenar alterações.
- revisar as alterações e os testes gerados pelo modo de agente antes de fazer merge na base de código.

Agora vamos [testar o recurso com o servidor MCP do Playwright][next-lesson] e abrir um pull request.

### Exercício extra de exploração — implementar paginação

À medida que a lista de jogos crescer, será necessário implementar paginação. Use as habilidades aprendidas neste exercício e peça ao Copilot que atualize o site. Considere:

- seguir as práticas recomendadas, incluindo o uso dos arquivos de instruções existentes.
- decidir como implementar a paginação: permitir que a pessoa usuária selecione o tamanho da página ou defini-lo no código.
- fornecer orientações suficientes no prompt para que o Copilot implemente o que você deseja.
- iterar com o GitHub Copilot, solicitando alterações e fornecendo contexto. Esse é o fluxo normal de trabalho com o Copilot.

## Recursos

- [Modos ask, edit e agent do Copilot: o que fazem e quando usar][choose-mode]
- [Modo de agente no VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode

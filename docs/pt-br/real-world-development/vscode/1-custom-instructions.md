---
title: "Exercício 1 - Instruções personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Modo de agente →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisa ser feita de uma maneira específica — ou se há informações prévias que o Copilot deve conhecer — disponibilize esse contexto. Há várias formas de compartilhar contexto específico com o Copilot. Uma das principais são os [arquivos de instruções][instruction-files], usados para orientar a geração de código.

Neste exercício, você vai:

- explorar como o contexto específico do projeto, as diretrizes de programação e os padrões de documentação chegam ao Copilot por meio das instruções personalizadas do repositório e de arquivos de instruções aplicáveis a caminhos específicos.
- gerar a primeira parte de dados da filtragem (um helper de distribuidoras) com as instruções *atuais*.
- adicionar um novo padrão para todo o repositório a `.github/copilot-instructions.md`.
- executar o mesmo prompt novamente e observar o código gerado adotar o novo padrão.
- fazer commit da atualização das instruções e da parte de filtragem em `main` para que o Copilot use as orientações atualizadas no próximo exercício.

> [!CAUTION]
> O código gerado pode não seguir alguns dos padrões definidos. O Copilot é não determinístico. O objetivo deste exercício é observar a *tendência* de mudança de comportamento depois da atualização das instruções, não obter uma saída idêntica caractere por caractere.

## Arquivos de instruções

### Cenário

Como qualquer boa empresa de desenvolvimento, a Tailspin Toys tem diretrizes e requisitos para as práticas de desenvolvimento. Entre eles:

- A camada de dados sempre precisa de testes unitários.
- A interface deve usar o modo escuro e ter uma aparência moderna.
- O código deve incluir documentação na forma de comentários TSDoc.
- Um bloco de comentários no início de cada arquivo deve descrever sua finalidade.

Com os arquivos de instruções, você garantirá que o Copilot tenha as informações certas para realizar tarefas conforme as práticas destacadas.

### Instruções personalizadas

As instruções personalizadas oferecem contexto e preferências ao Copilot para que ele entenda melhor seu estilo de programação e seus requisitos. Esse recurso ajuda a orientar o Copilot para que ofereça sugestões e trechos de código mais relevantes. Você pode especificar convenções de código, bibliotecas e até mesmo os tipos de comentários que prefere. É possível criar instruções para todo o repositório ou para tipos específicos de arquivo e contexto de tarefa.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`: um único arquivo enviado ao Copilot em **todas** as solicitações para o repositório. Ele deve conter informações do projeto — contexto relevante para a maioria das solicitações de chat ou CLI. Isso pode incluir a pilha tecnológica, uma visão geral do que está sendo desenvolvido, práticas recomendadas e outras orientações globais.
- `.github/instructions/*.instructions.md`: arquivos que podem ser criados para tarefas ou tipos de arquivo específicos. Você pode usá-los para fornecer diretrizes para linguagens específicas, como TypeScript ou Astro, ou para tarefas como criar um componente de interface ou testes unitários.

> [!NOTE]
> No IDE, os arquivos de instruções são usados para gerar código no Copilot Chat, mas não para conclusões de código ou sugestões de próxima edição.
>
> O Copilot Chat, o Copilot CLI e o agente de nuvem do Copilot usam os arquivos de instruções do repositório e os arquivos `*.instructions.md` com frontmatter `applyTo` ao gerar código.
>
> O Copilot também [oferece suporte a arquivos de instruções em outros padrões][custom-instructions-support], incluindo AGENTS.md e CLAUDE.md.

### Práticas recomendadas para gerenciar arquivos de instruções

Uma conversa completa sobre como criar arquivos de instruções está fora do escopo do workshop. No entanto, os exemplos do projeto mostram uma abordagem representativa. Em linhas gerais:

- Mantenha as instruções em `copilot-instructions.md` focadas na orientação do projeto, como a descrição do que está sendo desenvolvido, a estrutura do projeto e padrões globais de programação.
- Use arquivos `*.instructions.md` para instruções específicas de tipos de arquivo, como testes unitários, componentes Astro ou camada de dados, ou para tarefas específicas.
- Use linguagem natural. Mantenha as orientações claras e dê exemplos de como o código deve (e não deve) ser.

Não há uma única maneira certa de criar arquivos de instruções, assim como não há uma única maneira de usar IA. A experimentação ajudará você a descobrir o que funciona melhor para seu projeto.

> [!TIP]
> Todo projeto que usa o GitHub Copilot deve ter um conjunto robusto de arquivos de instruções. Ao explorar os arquivos deste projeto, você verá que há instruções para vários tipos de tarefa, como [atualizações de interface][ui-instructions] e [Astro][astro-instructions].
>
> O Copilot também pode ajudar a gerar arquivos de instruções. Cada interface disponibiliza esse recurso de uma maneira diferente (por exemplo, **Configure Chat → Generate Agent Instructions** no VS Code ou `/init` no Copilot CLI); a lição do ambiente em uso indicará quando for relevante.
>
> Quer encontrar templates ou um ponto de partida? Explore o [awesome-copilot][awesome-copilot], um repositório com arquivos de instruções, agentes personalizados e outros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar os arquivos de instruções personalizadas deste projeto

Leia os arquivos de instruções incluídos neste repositório: um arquivo principal `copilot-instructions.md` e um conjunto de arquivos `*.instructions.md` para diferentes tarefas. Abra-os no editor ou na interface Web do GitHub.

1. Abra `.github/copilot-instructions.md`.
2. Explore o arquivo e observe a breve descrição do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Em **Code standards**, observe as orientações aninhadas para **GitHub Actions Workflows**. Elas se aplicam a qualquer interação com o Copilot.
3. Abra a pasta `.github/instructions` e explore os arquivos. Observe as instruções para arquivos Astro, a camada de dados Drizzle, testes e outros.
4. Abra `.github/instructions/unit-tests.instructions.md`. Observe o campo `applyTo` no início do arquivo: ele define um glob (relativo à raiz do repositório) que determina a quais arquivos as instruções se aplicam. Neste caso, qualquer arquivo TypeScript de teste (por exemplo, um que corresponda a `**/*.test.ts`) será incluído.
5. Observe as instruções específicas para criar testes unitários neste projeto.
6. Por fim, abra `.github/instructions/drizzle.instructions.md` e role até o final. Observe os links para outros arquivos de instruções, como `unit-tests.instructions.md`, e para arquivos existentes do projeto. Isso permite dividir conjuntos maiores em arquivos menores e reutilizáveis, além de apontar exemplos para o Copilot. (Os caminhos são relativos ao arquivo de instruções, não à raiz do repositório.)

> [!NOTE]
> A seção **Code formatting requirements** em `copilot-instructions.md` documenta os padrões de programação do projeto, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários TSDoc e cabeçalhos de comentários nos arquivos.

## Criar uma branch para as alterações

Vamos seguir algumas práticas recomendadas e criar uma branch para as alterações.

1. Volte ao Codespace do exercício anterior.
2. Abra um terminal pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crie uma branch e alterne para ela:

   ```bash
   git checkout -b custom-instructions
   ```

## Usar o Copilot Chat *antes* de atualizar as instruções

> [!TIP]
> **Abrir o Copilot Chat**
>
> Antes de começar os exercícios abaixo, volte ao Codespace, abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam conforme o exercício — cada etapa indica as opções relevantes.

Para observar o impacto das instruções personalizadas, comece enviando um prompt com o arquivo de instruções atual. Depois, você o atualizará e enviará o mesmo prompt novamente para comparar os resultados.

1. Feche as guias abertas de exercícios anteriores para que o Copilot receba apenas o contexto desejado.
2. Abra `src/lib/publishers.ts` para que o Copilot saiba onde deve ficar o helper.
3. Selecione **Agent** no seletor de agentes da visualização Chat para que o Copilot possa aplicar alterações aos arquivos.

   ![Captura de tela do seletor de agentes na visualização Chat.](../../../_images/shared-chat-mode-selector.png)

4. Envie o prompt a seguir:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. O Copilot explora o projeto e aplica as atualizações no código, normalmente abrangendo o arquivo do helper e seus testes.
6. Observe que o helper proposto é uma função tipada que recebe um cliente `db` como primeiro argumento e retorna uma matriz tipada de distribuidoras — isso segue as convenções da camada de dados em `.github/instructions/drizzle.instructions.md` (aplicável a `src/lib/*.ts`).
7. Observe que o código proposto **não inclui** comentários TSDoc nem um cabeçalho de comentários no arquivo.

> [!CAUTION]
> Como o Copilot é probabilístico, pode ser que ele adicione comentários de documentação mesmo sem essa orientação. Isso não é um problema: o objetivo é observar a melhora na *consistência* depois da atualização das instruções.

## Adicionar um novo padrão para o repositório

Como mencionado, `.github/copilot-instructions.md` fornece informações do projeto ao Copilot. Vamos documentar os padrões de programação do repositório para melhorar as sugestões de código.

1. Abra novamente `.github/copilot-instructions.md`.
2. Localize a seção **Code formatting requirements**, que deve estar perto da linha 27. Observe que ela documenta os padrões de programação do projeto, mas ainda não inclui regras para documentação no código, motivo pelo qual o helper gerado não tinha comentários de documentação.
3. Adicione as linhas Markdown a seguir logo abaixo dos padrões existentes para orientar o Copilot a incluir cabeçalhos de comentários nos arquivos e comentários TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Salve `copilot-instructions.md`.

> [!TIP]
> Como você viu na lição anterior, os arquivos de instruções podem estar no nível do repositório (`.github/copilot-instructions.md`), para orientação global, ou ser arquivos `*.instructions.md` para linguagens, tipos de arquivo ou tarefas específicas. O arquivo do repositório é o local correto para padrões globais, como a regra de comentários de documentação que você acabou de adicionar.

## Executar o prompt novamente e observar a mudança

1. Volte ao Copilot Chat e selecione **New Chat** para limpar o contexto.
2. Volte a `src/lib/publishers.ts` para que o Copilot se concentre no arquivo certo.
3. Envie o **mesmo prompt** de antes:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observe que o arquivo proposto agora começa com um bloco de comentários semelhante a:

   ```typescript
   /**
    * Helpers de acesso a dados de distribuidoras da plataforma de financiamento
    * coletivo Tailspin Toys. Fornece funções para recuperar informações de
    * distribuidoras do banco de dados.
    */
   ```

5. Observe que a função proposta agora inclui um comentário TSDoc semelhante a:

   ```typescript
   /**
    * Retorna uma lista de todas as distribuidoras com seus IDs e nomes.
    *
    * @param db - O cliente do banco de dados Drizzle.
    * @returns Uma promise resolvida com uma matriz de objetos de distribuidora.
    */
   ```

Você acabou de orientar o Copilot a seguir um novo padrão e aplicá-lo ao código real que servirá de base para o próximo exercício.

## Fazer commit das instruções e enviar a branch

Os arquivos de instruções são como qualquer outro recurso do repositório e devem ser gerenciados pelo controle de versão da mesma forma. Vamos fazer commit e enviar a branch.

1. Abra uma nova janela de terminal pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. No terminal, confirme que as alterações nas instruções e no helper estão presentes:

   ```bash
   git status
   ```

3. Prepare e faça commit da atualização das instruções e de todos os arquivos alterados pelo Copilot para criar a base do helper:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Se git status mostrar outras atualizações de apoio (por exemplo, testes), adicione esses arquivos também.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Envie a branch ao repositório:

   ```bash
   git push -u origin custom-instructions
   ```

## Criar e integrar um pull request

Com a branch enviada, vamos abrir um pull request e vinculá-lo à issue sobre os padrões de documentação do backlog. Poderíamos fazer isso manualmente, mas o Copilot pode cuidar dessa tarefa usando as ferramentas do GitHub já conectadas ao projeto. Vamos pedir que localize a issue, crie um PR para encerrá-la e depois faça o merge.

> [!NOTE]
> Este é o primeiro exercício em que o Copilot age no GitHub em seu nome. O template do projeto já conecta as ferramentas do GitHub ao espaço de trabalho, então não é necessário configurar nada. Na primeira vez que o Copilot usar uma delas, o VS Code poderá solicitar que você entre no GitHub. Siga as instruções para permitir o acesso. Você aprenderá como essa conexão funciona (Model Context Protocol) em um exercício posterior.

1. Abra o Copilot Chat no Codespace.
2. Pressione <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir a visualização Copilot Chat e confirme que **Agent** está selecionado no seletor de agentes.
3. Peça ao Copilot que localize a issue relacionada à atualização dos arquivos de instruções e crie um PR a partir da branch atual, descrevendo tanto as atualizações das instruções quanto a base do novo helper de distribuidoras:

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. O Copilot começará a localizar a issue e criar o PR.
5. Quando o Copilot solicitar **Allow** para executar ações do GitHub em seu nome, revise o comando e selecione **Allow**, se apropriado.
6. Depois que o PR for criado, peça ao Copilot que faça o merge para `main` e volte para essa branch localmente:

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Quando o Copilot solicitar **Allow** para executar ações do GitHub e comandos de shell em seu nome, revise o comando e selecione **Allow**, se apropriado.

Você acabou de criar e integrar um pull request com a ajuda do GitHub Copilot!

## Resumo e próximos passos

Você explorou como o Copilot obtém contexto dos arquivos de instruções deste projeto e usou o Copilot Chat no VS Code para:

- enviar um prompt de geração de código e observar o resultado com as instruções *existentes*.
- adicionar um novo padrão global a `.github/copilot-instructions.md`.
- executar novamente o mesmo prompt e observar o código proposto adotar o novo padrão.
- fazer commit das instruções e da base do helper em `main` para que o próximo exercício possa se apoiar nelas.

Em seguida, você colocará essas instruções em prática no [modo de agente][next-lesson], enquanto o Copilot adiciona um novo recurso em vários pontos da base de código.

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [5 dicas para escrever instruções melhores para o Copilot][copilot-instructions-five-tips]
- [Instruções personalizadas pessoais do GitHub Copilot][personal-instructions]
- [Awesome Copilot — coleção de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot

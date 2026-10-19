---
title: "Lição 4 - Orientar o Copilot com instruções personalizadas"
description: "Explore as instruções do repositório, adicione um padrão de documentação e aplique-o ao código de filtragem."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisa ser feita de uma maneira específica, disponibilize essa orientação ao Copilot. [Arquivos de instruções][instruction-files] descrevem não apenas *qual* código você quer, mas também *como* ele deve ser estruturado. Agora que você implementou a filtragem, vai explorar as instruções usadas pelo Copilot, adicionar um padrão de documentação e aplicá-lo ao código.

Nesta lição, você vai:

- explorar como as instruções do repositório e os arquivos de instruções aplicáveis a caminhos específicos chegam ao agente.
- atualizar o arquivo de instruções para garantir que os padrões de codificação sejam seguidos.
- observar o impacto dos arquivos de instruções no código.

## Cenário

Como qualquer boa empresa de desenvolvimento, a Tailspin Toys tem diretrizes e requisitos para as práticas de desenvolvimento. Entre eles:

- Os comentários devem explicar a intenção e decisões não óbvias, em vez de repetir o código.
- Funções exportadas em `db/` e `src/lib/` devem documentar sua finalidade, parâmetros e valores de retorno com TSDoc/JSDoc, incluindo um argumento `db` injetável quando houver.
- Componentes Astro reutilizáveis devem documentar seus contratos `Props`, e os comentários devem permanecer atualizados quando o código relacionado mudar.
- As orientações existentes de formatação e lint devem ser preservadas.

Com os arquivos de instruções, você garantirá que o Copilot tenha as informações certas para realizar as tarefas de acordo com as práticas destacadas.

## Arquivos de instruções

As instruções personalizadas oferecem contexto e preferências ao Copilot para que ele entenda melhor seu estilo de programação e seus requisitos. Esse recurso ajuda a orientar o Copilot para que ofereça sugestões e trechos de código mais relevantes. Você pode especificar convenções de código, bibliotecas e até mesmo os tipos de comentários que prefere. É possível criar instruções para todo o repositório ou para tipos específicos de arquivo e contexto de tarefa.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`: um único arquivo de instruções enviado ao Copilot em **todas** as solicitações para o repositório. Ele deve conter informações do projeto relevantes para a maioria das solicitações.
- `.github/instructions/*.instructions.md`: arquivos que fornecem diretrizes para linguagens, tipos de arquivo ou tarefas específicas.

> [!NOTE]
> Os formatos de instrução e o suporte variam conforme o ambiente. Consulte a [referência de suporte a instruções personalizadas][custom-instructions-support] antes de depender de um formato específico.

## Explorar os arquivos de instruções personalizadas deste projeto

Para começar, o projeto inicial já inclui vários arquivos de instruções. Vamos explorá-los antes de fazer uma alteração e observar seu impacto.

1. Volte ao Codespace.
2. No editor do Codespaces (não no terminal), abra `.github/copilot-instructions.md`.
3. Explore o arquivo e observe a breve descrição do projeto e as orientações de programação. Essas instruções se aplicam a todas as interações com o Copilot neste repositório.
4. Abra a pasta `.github/instructions` e explore os arquivos. Observe as instruções para arquivos Astro, a camada de dados Drizzle, testes e outros.
5. Abra `.github/instructions/unit-tests.instructions.md`. Observe o campo `applyTo` no início do arquivo: ele define um glob que determina a quais arquivos as instruções se aplicam.
6. Abra `.github/instructions/drizzle.instructions.md` e observe os links para outros arquivos de instruções e arquivos existentes do projeto. Isso permite dividir conjuntos maiores em arquivos menores e reutilizáveis, além de apontar exemplos para o Copilot.

## Atualizar os arquivos de instruções de acordo com as diretrizes da equipe

Os arquivos existentes são um bom começo, mas ainda falta algo. Vamos modificar o arquivo principal `copilot-instructions.md` para garantir que comentários TSDoc sejam adicionados ao novo TypeScript.

1. Em `.github/copilot-instructions.md`, localize a seção **Code formatting guidance**, que deve estar perto da linha 35.
2. Adicione o item a seguir como último tópico:

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

O arquivo será salvo automaticamente!

## Usar as orientações atualizadas

O Copilot CLI carrega as instruções do repositório quando uma conversa é iniciada. Depois de salvar as instruções atualizadas, saia da conversa de filtragem e retome-a para que o Copilot comece a usar as orientações sem perder o contexto do recurso.

1. Se a conversa de filtragem ainda estiver aberta, digite `/exit` para fechá-la. O Copilot CLI salva as conversas automaticamente.
2. Na raiz do repositório, na branch `game-filters-cli`, retome a conversa salva:

   ```bash
   copilot --resume
   ```

   Se solicitado, selecione a conversa de filtragem e confirme que a branch `game-filters-cli` está em uso.
3. Peça ao Copilot que aplique as orientações atualizadas:

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. Digite `/diff` e leia os arquivos TypeScript alterados. Observe os comentários TSDoc recém-gerados e confirme que descrevem o código com precisão.

## Resumo e próximos passos

Você explorou como o Copilot CLI obtém contexto dos arquivos de instruções e aplicou um novo padrão ao recurso. Em particular, você:

- explorou como as instruções do repositório e os arquivos aplicáveis a caminhos específicos chegam ao agente.
- atualizou o arquivo de instruções para garantir que os padrões de codificação sejam seguidos.
- observou o impacto dos arquivos de instruções no código.

Em seguida, você [personalizará e executará a skill reutilizável quality-checks][next-lesson] para garantir que lint e testes sejam executados de forma consistente.

## Recursos

- [Adicionar instruções personalizadas ao Copilot CLI][instruction-files]
- [Suporte a instruções personalizadas][custom-instructions-support]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [Awesome Copilot — coleção de arquivos de instruções e outros recursos][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/

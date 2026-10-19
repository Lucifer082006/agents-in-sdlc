---
title: "Exercício 0: Pré-requisitos"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Antes de começar os exercícios do VS Code, prepare tudo. Você criará sua própria cópia do repositório Tailspin Toys, iniciará um [Codespace][codespaces] para trabalhar e confirmará que o GitHub Copilot Chat está funcionando no editor.

## Configurar o repositório do laboratório

Para criar uma cópia do repositório que conterá o código, crie uma instância a partir do [template][template-repository]. A nova instância conterá todos os arquivos necessários para o laboratório e será usada durante os exercícios.

1. Em uma nova janela do navegador, acesse o [template da Tailspin Toys][tailspin-template].
2. Crie sua própria cópia do repositório selecionando o botão **Use this template** na página do repositório do laboratório. Em seguida, selecione **Create a new repository**.

    ![Botão Use this template](../../../_images/ex0-use-template.png)

3. Se estiver participando de um evento conduzido pelo GitHub ou pela Microsoft, siga as instruções das pessoas mentoras. Caso contrário, crie o novo repositório em uma organização na qual você tenha acesso ao GitHub Copilot.

    ![Configurações do template do repositório](../../../_images/ex0-repository-settings.png)

4. Anote o caminho do repositório criado (**organization-or-user-name/repository-name**), pois você precisará dele mais tarde no laboratório.

> [!NOTE]
> **Seu backlog está pronto**
>
> Quando você cria o repositório a partir do template, um backlog de issues do GitHub é criado automaticamente. Você trabalhará nessas issues durante o workshop — não precisa criar nenhuma.

## Criar um Codespace

Em seguida, você usará um Codespace para concluir os exercícios do laboratório.

O [GitHub Codespaces][codespaces] é um ambiente de desenvolvimento na nuvem que permite escrever, executar e depurar código diretamente no navegador. Ele oferece um IDE completo, com suporte a várias linguagens de programação, extensões e ferramentas.

1. Acesse o repositório que acabou de criar.
2. Selecione o botão verde **Code**.

    ![Selecione o botão Code](../../../_images/ex0-code-button.png)

3. Selecione a guia **Codespaces** e depois o botão **+** para criar um Codespace.

    ![Criar um Codespace](../../../_images/ex0-create-codespace.png)

A criação do Codespace levará alguns minutos, mas ainda será bem mais rápida do que instalar manualmente todos os serviços. Enquanto isso, você pode explorar outros recursos do GitHub Copilot, que veremos a seguir.

> [!CAUTION]
> Você voltará ao Codespace em um exercício futuro. Por enquanto, deixe-o aberto em uma guia do navegador.

> [!NOTE]
> Este workshop foi criado para ser executado em um Codespace ou em um [dev container][dev-containers] local. Ambos garantem que o ambiente tenha os pré-requisitos necessários. Se preferir executá-lo localmente, abra o repositório clonado no VS Code e selecione **Reopen in Container** quando solicitado. O VS Code criará o mesmo dev container usado pelo Codespace.

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## Usar o GitHub Copilot Chat e o modo de agente

Para acessar o modo de agente do GitHub Copilot Chat, você precisa ter a extensão GitHub Copilot Chat instalada no IDE. Ela já deve estar instalada se você estiver usando um GitHub Codespace.

> [!TIP]
> Se a extensão GitHub Copilot Chat não estiver instalada, você pode [instalá-la pelo Visual Studio Code Marketplace][copilot-chat-extension]. Outra opção é abrir a visualização Extensions no Visual Studio Code, pesquisar **GitHub Copilot Chat** e selecionar **Install**.

Depois de instalar a extensão, talvez seja necessário autenticar sua conta do GitHub para habilitá-la.

1. Volte ao Codespace.
2. Se o Copilot Chat não estiver à direita do editor, selecione o ícone **Copilot Chat** na parte superior do Codespace.
3. Digite uma mensagem, como "Hello world", na janela do Copilot Chat e pressione Enter. Isso deverá ativar o Copilot Chat.
4. Se você não estiver autenticado, será solicitado que entre na sua conta do GitHub. Siga as instruções para autenticar.

    ![Exemplo de solicitação de autenticação do Copilot Chat](../../../_images/ex1-copilot-authentication.png)

5. Após a autenticação, a janela do Copilot Chat deverá aparecer.

## Resumo

Parabéns, você criou uma cópia do repositório do laboratório! Você também iniciou a criação do Codespace, que será usado quando começar a escrever código.

## Próxima etapa

Vamos começar a usar o Copilot. Continue para o [Exercício 1 - Instruções personalizadas][next-lesson], no qual você ensinará ao Copilot as convenções do projeto.

## Recursos

- [Visão geral do GitHub Codespaces][codespaces]
- [Criar um repositório a partir de um template][template-repository]
- [Introdução ao Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/

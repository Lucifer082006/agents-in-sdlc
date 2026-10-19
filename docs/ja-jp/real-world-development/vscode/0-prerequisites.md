---
title: "演習 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

VS Code の演習を始める前に、必要な準備をします。Tailspin Toys リポジトリの自分用コピーを作成し、作業用の [codespace][codespaces] を起動して、エディターで GitHub Copilot Chat が利用できることを確認します。

## ラボ用リポジトリをセットアップする

作成するコード用にリポジトリのコピーを用意します。[テンプレート][template-repository]から自分用のインスタンスを作成します。作成したインスタンスにはラボに必要なファイルが含まれており、各演習で使用します。

1. 新しいブラウザーウィンドウで [Tailspin Toys テンプレート][tailspin-template]に移動します。
2. ラボ用リポジトリのページで **Use this template** を選択し、**Create a new repository** を選択して自分用のコピーを作成します。

    ![テンプレートからリポジトリを作成するボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft が主催するイベントの一環として参加する場合は、メンターの指示に従います。それ以外の場合は、GitHub Copilot を利用できる組織に新しいリポジトリを作成します。

    ![リポジトリテンプレートの設定画面](../../../_images/ex0-repository-settings.png)

4. 後の演習で参照するため、作成したリポジトリのパス (**organization-or-user-name/repository-name**) を控えておきます。

> [!NOTE]
> **バックログの準備は完了しています**
>
> テンプレートからリポジトリを作成すると、GitHub Issue のバックログも自動で作成されます。ワークショップではこれらの Issue を使います。自分で Issue を作成する必要はありません。

## Codespace を作成する

次に、codespace を使ってラボの演習に取り組みます。

[GitHub Codespaces][codespaces] は、ブラウザーで直接コードを記述、実行、デバッグできるクラウド開発環境です。複数のプログラミング言語、拡張機能、ツールに対応した多機能な IDE が用意されています。

1. 新しく作成したリポジトリに移動します。
2. 緑色の **Code** ボタンを選択します。

    ![Code ボタン](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを選択し、**+** ボタンを選択して新しい Codespace を作成します。

    ![新しい codespace を作成する画面](../../../_images/ex0-create-codespace.png)

Codespace の作成には数分かかりますが、サービスを手動でインストールするよりずっと早く完了します。作成中に GitHub Copilot のほかの機能を試してみてください。次の演習で詳しく説明します。

> [!CAUTION]
> 今後の演習でこの codespace に戻ります。ひとまず、ブラウザーのタブを開いたままにしてください。

> [!NOTE]
> このワークショップは codespace またはローカルの [dev container][dev-containers] で実行するように作られています。どちらの環境にも、スムーズに作業するための前提条件がインストールされています。ローカルで実行する場合は、クローンしたリポジトリを VS Code で開き、確認が表示されたら **Reopen in Container** を選択します。codespace と同じ dev container が VS Code で構築されます。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## GitHub Copilot Chat とエージェントモードを使う

GitHub Copilot Chat のエージェントモードを使うには、IDE に GitHub Copilot Chat 拡張機能がインストールされている必要があります。GitHub Codespace を使っている場合は、通常すでにインストールされています。

> [!TIP]
> GitHub Copilot Chat 拡張機能がインストールされていない場合は、[Visual Studio Code Marketplace からインストール][copilot-chat-extension]できます。または、Visual Studio Code の Extensions ビューを開き、**GitHub Copilot Chat** を検索して **Install** を選択します。

拡張機能のインストール後、利用するために GitHub アカウントで認証が必要な場合があります。

1. codespace に戻ります。
2. エディターの右側に Copilot Chat が表示されていない場合は、codespace の上部にある **Copilot Chat** アイコンを選択します。
3. Copilot Chat ウィンドウに「Hello world」などのメッセージを入力して、Enter キーを押します。Copilot Chat が有効になります。
4. まだ認証されていない場合は、GitHub アカウントへのサインインが求められます。画面の指示に従って認証します。

    ![Copilot Chat の認証プロンプト](../../../_images/ex1-copilot-authentication.png)

5. 認証が完了すると、Copilot Chat ウィンドウが表示されます。

## まとめ

ラボ用リポジトリのコピーを作成し、コードを書くときに使う codespace の作成を開始しました。

## 次のステップ

Copilot を使い始めましょう。[演習 1 - カスタム指示][next-lesson]に進み、Copilot にプロジェクトの規則を伝えます。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces のクイックスタート][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/

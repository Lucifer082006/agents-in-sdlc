---
title: "演習 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Copilot cloud agent の演習を始める前に、必要な準備をします。Tailspin Toys リポジトリの自分用コピーを作成し、[codespace][codespaces] を起動します。codespace では、指示ファイルを編集し、cloud agent が作成した変更をレビューできます。

## ラボ用リポジトリをセットアップする

作成するコード用にリポジトリのコピーを用意します。[テンプレート][template-repository]から自分用のインスタンスを作成します。作成したインスタンスにはラボに必要なファイルが含まれており、各演習で使用します。

1. 新しいブラウザーウィンドウで [Tailspin Toys テンプレート][tailspin-template]に移動します。
2. ラボ用リポジトリのページで **Use this template** を選択し、**Create a new repository** を選択して、自分用のコピーを作成します。

    ![テンプレートからリポジトリを作成するボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft が主催するイベントの一環としてワークショップに参加する場合は、メンターの指示に従ってください。それ以外の場合は、GitHub Copilot を利用できる組織に新しいリポジトリを作成します。

    ![リポジトリテンプレートの設定画面](../../../_images/ex0-repository-settings.png)

4. 後の演習で参照するため、作成したリポジトリのパス (**organization-or-user-name/repository-name**) を控えておきます。

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

## まとめ

ラボ用リポジトリのコピーを作成しました。Copilot cloud agent と並行して作業するために使う codespace も作成し始めました。

## 次のステップ

cloud agent が従うカスタム指示を追加します。[演習 1 - カスタム指示][next-lesson]に進みます。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces のクイックスタート][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/

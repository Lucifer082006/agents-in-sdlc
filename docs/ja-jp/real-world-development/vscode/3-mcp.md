---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: エージェントモード][previous-lesson] · [次のレッスン: カスタムエージェント →][next-lesson]

エージェントモードでフィルター機能を構築しました。プルリクエストを作成する前に、ブラウザーで実際に動作することを確認します。自分でアプリを操作する代わりに、**Playwright MCP サーバー**を接続して Copilot に実際のブラウザーを操作させ、機能をテストします。その後、ブランチを公開して PR を作成します。

この演習では、次の内容を学びます。

- Model Context Protocol (MCP) と、MCP サーバーが Copilot に新しいツールを追加する方法を学ぶ。
- Playwright MCP サーバーをワークスペースに追加する。
- Copilot にブラウザーでフィルター機能を手動テストさせる。
- ブランチを公開し、フィルター機能のプルリクエストを作成する。

## Model Context Protocol (MCP) とは

エージェントモードは、エディターの外部に接続できると、さらに強力になります。Copilot は Model Context Protocol (MCP) を使って外部に接続します。これは、エージェントが外部のツールやサービスと通信するための標準的な方法です。

![エージェントモードがコンテキスト、LLM、ツールと連携する仕組み。MCP サーバーや VS Code 拡張機能が提供するツールを含む](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) は、AI エージェントが外部ツールやサービスと通信する方法を提供します。MCP により、エージェントは外部ツールやサービスとリアルタイムに通信し、リソースを使って最新情報にアクセスしたり、ツールを使ってユーザーに代わり操作したりできます。

ツールやリソースには、AI エージェントと外部のツール・サービスをつなぐ MCP サーバーを介してアクセスします。MCP サーバーは、既存の API や npm パッケージなどのローカルツールを含む外部ツールとの通信を管理します。サーバーごとに、エージェントが利用できるツールとリソースが異なります。

よく使われる MCP サーバーの例を次に示します。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API にアクセスできます。AI エージェントはリポジトリの作成や更新、Issue やプルリクエストの管理などを実行できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright を使ったブラウザー自動化機能を提供します。Web ページへの移動、フォームへの入力、ボタンの選択などをエージェントが実行できます。

ほかにもさまざまなツールやリソースにアクセスできる MCP サーバーがあります。GitHub は、エコシステム内の MCP サーバーを見つけ、貢献しやすくするための [MCP レジストリ](https://github.com/mcp)をホストしています。

> [!CAUTION]
> セキュリティの観点から、MCP サーバーもプロジェクト内のほかの依存関係と同様に扱ってください。使用前にソースコードを注意深く確認し、発行元を検証して、セキュリティへの影響を検討してください。信頼できる MCP サーバーのみを使用し、機密リソースや操作へのアクセス許可には注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースから利用できる MCP サーバーを設定します。

1. codespace で `.vscode/mcp.json` を開きます。
2. 次のように `github` サーバーがすでに設定されています。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

この `github` エントリはプロジェクトテンプレートに含まれています。前の演習で Copilot が Issue のバックログを読めたのはこのためです。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使うため、ローカルへのインストールは不要です。VS Code は HTTP 経由で接続し、Copilot が最初にツールを使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

次に 2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot がブラウザーを操作できるため、機能のテストに適しています。

1. `.vscode/mcp.json` に `github` と並べて `playwright` エントリを追加し、次の内容にします。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモートの `github` サーバーとは異なり、`playwright` は**ローカル**サーバーです。VS Code は `npx @playwright/mcp@latest` を実行して起動します。`--headless` フラグを使うと、画面に表示せずにブラウザーを実行できます。デスクトップ環境がない codespace では、この設定が必要です。

> [!NOTE]
> Tailspin Toys プロジェクトはエンドツーエンドテストでも Playwright を使うため、必要なブラウザーは通常すでにインストールされています。Copilot がブラウザーの不足を報告したら、`npx playwright install chromium` を実行して再試行するよう依頼します。

## サーバーを起動して信頼する

VS Code は MCP サーバーを**必要になったときに起動**します。Copilot が初めてサーバーのツールを必要とすると、VS Code がサーバーを起動し、信頼するか確認します。

1. `.vscode/mcp.json` の `playwright` エントリの上に **Start** アクションが表示されます。選択してサーバーを起動し、構成が正しいことを確認します。
2. VS Code がサーバーを信頼するか確認したら、構成を確認して信頼を許可し、サーバーを起動します。

手動起動は省略できます。その場合は、次の手順で Copilot が初めてツールを必要とした際に VS Code がサーバーを起動し、信頼を確認します。

## フィルター機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が実際のブラウザーを操作できます。自分でアプリを操作して作業を確認しなくても、エージェントがページを開き、移動し、フィルターを適用し、結果を読み取って要約できます。会話を離れずに機能が期待どおりに動作することを確かめる簡単な方法です。

Playwright MCP サーバーは、スクリーンショットではなくページの[アクセシビリティツリー][playwright-mcp-server]を使います。エージェントは支援技術と同じように、ボタン、リンク、リスト項目など、ラベル付けされた構造化要素を調べます。簡単な機能確認を、アクセシビリティの簡易チェックにも活用できます。

サーバーを接続し、アプリを実行した状態で、次のプロンプトを Copilot に送信し、作成したフィルター機能を試します。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバーを通じてブラウザーを起動し、各手順を実行して結果を報告します。報告を Issue の受け入れ基準と照らし合わせてください。問題があれば、追加で質問するか、プルリクエストを作成する前にコードを修正するよう依頼します。

> [!NOTE]
> テスト時はアプリが `http://localhost:4321` で実行されている必要があります。開発サーバーを停止した場合は、プロンプトを送信する前に再起動します。Copilot が Playwright MCP サーバーを初めて使うときに、ブラウザーのダウンロードが必要な場合があります。不足が報告されたら、`npx playwright install chromium` を実行して再試行するよう依頼します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能が動くことを確認したので、チームがレビューできるようプルリクエスト (PR) を開きます。最初に `filtering-vscode` ブランチを公開します。

1. codespace の **Source Control** パネルに移動し、Copilot による変更を確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンでコミットメッセージを生成します。

   ![変更内容を表示する Source Control パネル](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択してブランチをリポジトリにプッシュします。

## プルリクエストを作成する

github.com や GitHub コマンドラインインターフェイス (CLI) など、プルリクエストを作成する方法はいくつかあります。ここでは GitHub Copilot に作成を依頼します。関連する Issue を探し、Issue に関連付けた PR を作成できます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを開始します。
2. GitHub ツールを Copilot が使えるよう、エージェントドロップダウンで **Agent** が選択されていることを確認します。
3. 次のプロンプトで PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 必要に応じて **Continue** を選択し、情報収集と操作を許可します。GitHub ツールを初めて使う際は GitHub の認証を求められる場合があります。指示に従って許可します。
5. Copilot が Issue を検索して適切なものを見つけ、PR を作成する様子を確認します。
6. Copilot が生成したリンクを選択して PR をレビューしますが、**まだマージしないでください**。

## まとめと次のステップ

この演習では、次の作業を行いました。

- Model Context Protocol (MCP) と、MCP サーバーが Copilot に新しいツールを追加する方法を学びました。
- Playwright MCP サーバーをワークスペースに追加しました。
- ブラウザーで機能を手動テストするために Playwright MCP を使いました。
- ブランチを公開し、フィルター機能のプルリクエストを作成しました。

MCP サーバーを使って機能を**テスト**しました。Copilot を拡張する方法は MCP だけではありません。次はアクセシビリティレビューなどの作業を効率化する[カスタムエージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration

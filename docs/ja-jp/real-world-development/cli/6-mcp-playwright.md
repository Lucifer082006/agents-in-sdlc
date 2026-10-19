---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI の Playwright MCP を確認または設定し、ブラウザーでフィルター機能を試します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

すでに確認したように、コードを書くことだけが開発ではありません。データや外部サービスを扱い、Copilot が追加の自動化を利用できるようにする必要もあります。MCP サーバーを使うと、Copilot は CLI に組み込まれた機能を超えて、ツールやサービスを利用できます。

このレッスンでは、次の内容を学習します。

- Model Context Protocol (MCP) とは何か、Copilot CLI がどう使うかを理解する。
- まだ利用できない場合に Playwright MCP サーバーを追加する。
- エージェントにブラウザーを操作させ、フィルター機能を確認する。

## シナリオ

ユニットテストとエンドツーエンドテストは重要ですが、UI の変更を検証するには実際に操作する必要があります。Copilot がユーザーと同じように開発中の Web サイトを使えるようにして、変更が期待どおりに動作する確信を高めます。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] は、AI エージェントが外部ツールやサービスと通信する方法を提供します。MCP を使うと、エージェントは外部ツールやサービスとリアルタイムに通信し、最新情報にアクセスして、ユーザーに代わって操作できます。

ツールやリソースには、AI エージェントと外部ツール・サービスをつなぐ MCP サーバーを通じてアクセスします。MCP サーバーごとに、エージェントが利用できるツールやリソースが異なります。

よく使われる MCP サーバーの例を次に示します。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、Issue、プルリクエストを管理する API にアクセスできます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright を使ったブラウザー自動化機能を提供します。

ほかにも多くの MCP サーバーがあります。GitHub は、エコシステム内の MCP サーバーを見つけ、貢献しやすくするための [MCP レジストリ][mcp-registry]をホストしています。

> [!CAUTION]
> MCP サーバーもプロジェクト内のほかの依存関係と同様に扱ってください。使用前にソースコードを確認し、発行元を検証して、セキュリティへの影響を検討してください。

## Playwright MCP サーバーを追加する

Playwright MCP サーバーがまだ利用できない場合は追加し、Copilot がユーザーと同じように Web サイトを操作できるようにします。

> [!NOTE]
> Copilot CLI に Playwright MCP がすでに設定されている場合は、このセットアップ手順を省略して次のセクションに進んでください。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力し、MCP サーバーの追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前に `playwright` と入力し、<kbd>Tab</kbd> を押します。
4. <kbd>Enter</kbd> を押してサーバーの種類を **STDIO** にし、<kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して、新しい MCP サーバーを保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを閉じます。

## Playwright で機能を確認するよう Copilot に依頼する

これまでは機能が期待どおりに動くことを手動で確認しました。次は Copilot が利用できる Playwright MCP サーバーを使って同じことをしましょう。

1. 次のプロンプトを使い、Playwright MCP サーバーで機能を検証するよう Copilot に伝えます。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常、Copilot は MCP サーバーを自動的に選ぶため、明示的に指定する必要はありません。ただし、どのサーバーを使うべきか分かっている場合は、指定すると結果が安定し、トークンの節約にもなります。

2. Copilot がブラウザーで機能を確認するために実行する手順を見ます。
3. レポートを読み、すべてが期待どおりに動作していることを確認します。

Copilot はサーバーを起動し、Playwright で Web サイトを操作し、サーバーを停止して、結果を報告します。

## まとめと次のステップ

Copilot CLI から Playwright MCP サーバーを使い、実際のブラウザーで機能を確認できました。次の作業を行いました。

- Model Context Protocol (MCP) と Copilot CLI での利用方法を学びました。
- まだ利用できない場合に Playwright MCP サーバーを追加しました。
- エージェントにブラウザーを操作させ、フィルター機能を確認しました。

次は、スキルとブラウザーツールを専門的な役割にまとめる [QA カスタムエージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers

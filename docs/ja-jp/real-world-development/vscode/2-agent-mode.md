---
title: "演習 2 - Copilot Agent Mode で新機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの小さな更新でも、複数のファイルの変更やテストの実行が必要になることがあります。開発者は、必要なファイルを探し、変更を加え、テストを実行し、デバッグし、変更し忘れたファイルを見つけ、さらに更新する、といった作業を繰り返します。

こうした作業に役立つのが Copilot Agent Mode です。

Copilot Agent Mode は IDE 内でより自律的に動作します。開発者と同じように、まず既存のプロジェクト構成を調査し、必要な変更を行い、テストなどのタスクを実行し、見つかった問題を自動的に修正します。Agent Mode でサイトに新しい機能を追加する方法を見ていきましょう。

> [!NOTE]
> 名前は似ていますが、エージェントモードと cloud agent は異なる体験を提供します。エージェントモードは IDE 内で作業し、素早くやり取りできます。cloud agent はチームのメンバーのように非同期で動作し、Issue とプルリクエストを通じてやり取りします。

この演習では、次の内容を学びます。

- Copilot Agent Mode がプロジェクトを調査し、関連ファイルを特定して、複数ファイルにわたる変更を行う方法。
- GitHub Copilot Agent Mode が UI とデータ層全体に新機能を実装する方法。
- Copilot Agent Mode が生成した変更とテストを、コードベースにマージする前にレビューする方法。

## シナリオ

ゲーム一覧が増えてきたため、ユーザーがカテゴリと発行元でゲームを絞り込めるようにします。前の演習では発行元ヘルパーを追加しました。ここでは Copilot Agent Mode を使って、残りのデータ層、UI、テストを完成させます。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の Web サイトを確認して、現在の機能を把握します。

この Web サイトは、開発者向けテーマのボードゲームをクラウドファンディングするプラットフォームです。ゲーム一覧と詳細を表示します。単一の Astro アプリで、ビルド時にページを静的 HTML としてレンダリングします。ページでは、frontmatter 内から Drizzle ORM を使ってローカルの SQLite データベースを直接クエリします。独立したバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータアクセスヘルパーは `src/lib/` に配置し、標準 DOM API を使う小さなスコープ付き Astro `<script>` でインタラクティブ機能を追加します。

### Web サイトを起動する

Web サイトを実行しやすいように、単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。GitHub Codespace で次の手順を実行します。

1. codespace に戻ります。前の演習の後、`main` にいるはずです。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押し、Codespace 内で新しいターミナルを開きます。
3. フィルター機能の新しいブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドで Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーが準備できると、次のような URL のバナーが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くブラウザーウィンドウを求めるダイアログが表示された場合は、**x** を選択して閉じます。

5. <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら、ターミナル内の Astro サーバーアドレス `http://localhost:4321` を選択して Web サイトを開きます。

> [!NOTE]
> codespace のターミナルで localhost URL のリンクを選択すると、`https://<your-codespace-name>-4321.app.github.dev/` に自動的に転送されます。これは、Web サーバーをホストしている codespace へのプライベートトンネルです。

### Web サイトを確認する

Web サイトを起動したら、機能を確認します。主なページは次のとおりです。

- **ホームページ**: ボードゲームの一覧と、タイトル、画像、説明を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、発行元、カテゴリなどの詳細を表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に、codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習ごとに異なります。必要な箇所で説明します。

最初の Web サイトは動作していますが、新しい機能を追加して改善したいところです。まずバックログを確認します。テンプレートからプロジェクトをセットアップすると、Issue のバックログが自動的に作成されています。GitHub Copilot に項目を表示してもらいます。

1. Chat ビューのエージェントドロップダウンで **Agent** を選びます。**Agent** は複数ファイルの変更を自律的に計画・実装し、ターミナルコマンドを実行してツールを呼び出します。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデルの一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者は、特定のモデルを推奨しているわけではありません。ワークショップ作成時に Claude Sonnet 4.5 を使ったため、体験の一貫性を保つ目的で手順に含めています。ただし、LLM は確率的に動作するため、ここに記載したものと異なる提案が表示される場合があります。これは正常な動作です。

> [!NOTE]
> LLM の確率的な性質により、Copilot は異なる MCP コマンドを使うことがありますが、タスクは完了できるはずです。

3. 次のプロンプトを送信して、バックログの Issue を Copilot に確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択し、すべての Issue を一覧表示するコマンドを実行します。
5. 生成された Issue の一覧を確認します。

Copilot がユーザーにとって有用と思われる内容に基づいて、項目に優先順位を付けたことを確認してください。

## 指示ファイルを確認する

コード生成をエージェントに依頼する前に、作業のコンテキストを伝える指示ファイルを確認します。Web サイトへの機能追加方法が記載された [ユーザーインターフェイス (UI) 指示ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)を使います。

1. codespace で `.github/instructions/ui.instructions.md` に移動します。
2. 機能追加に関する全体的なガイダンスを確認します。次の内容が含まれます。
   - アーキテクチャの概要。
   - コンポーネント設計、テスト容易性、アクセシビリティの原則。
   - Astro、Tailwind CSS、Drizzle など、ファイル種別ごとの指示へのリンク。

> [!TIP]
> 指示ファイルから、ほかの指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルからの相対パスです。指示を再利用し、複雑な内容を小さく管理しやすい単位に分割して、例やテンプレートを示せます。

## フィルター機能を実装する

フィルター機能を完成させるには、少なくともアプリケーションを次の 3 か所で更新します。

- データ層 (`src/lib/`) の残りのフィルター処理を追加または改善する。
- フィルターの動作をテストするテストを追加または更新する。
- ゲーム一覧ページにフィルター UI を追加する。

コードベースにマージする前にテストを実行し、合格させる必要もあります。Copilot Agent Mode にこれらの作業を依頼しましょう。

1. 現在の Copilot 会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択して、指示ファイルを追加します。

   ![UI 指示ファイルを選択する例](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェントドロップダウンで **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

4. モデルに **Claude Sonnet 4.5** が選択されていることを確認します。
5. バックログの関連 Issue に基づいて実装するよう、次のプロンプトを送信します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調査し、機能に関連するファイルを見つける様子を確認します。データ層のヘルパー、UI、テストを特定してファイルを変更し、テストを実行します。

   ![Copilot がプロジェクトのファイルを調査している画面](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの変更、テストの実行などを行います。タスクやコードベースの複雑さにより、数分かかる場合があります。作業中、コードエディターに **Keep** と **Undo** が表示されることがあります。作業が完了するとすべての変更に **Keep** または **Undo** が表示されるため、処理中に選択する必要はありません。

7. Copilot から求められたら **Continue** を選択してテストを実行します。

   ![テスト実行の確認を求める Copilot Chat のダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりする場合があります。問題ありません。Copilot はタスクが完了してエラーが検出されなくなるまで、コード生成とテストを繰り返します。

   ![Copilot Agent Mode の会話と提案された変更](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを表示しているブラウザーに戻り、新しい機能を試します。
11. 動作とコードを確認したら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

GitHub Copilot Agent Mode を使って Tailspin Toys の Web サイトに新しい機能を追加する方法を確認しました。

- GitHub Copilot Agent Mode で UI とデータ層全体に新機能を実装できます。
- Copilot Agent Mode はプロジェクトを調査し、関連ファイルを特定して複数ファイルを変更します。
- コードベースにマージする前に、生成された変更とテストをレビューします。

次は [Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを作成します。

### ボーナス演習 — ページネーションを実装する

ゲーム一覧が増えると、ページネーションが必要になります。この演習で学んだスキルを使い、Copilot にページネーションを実装してもらいましょう。次の点を検討してください。

- 既存の指示ファイルを使うなど、既存のベストプラクティスに従う。
- ユーザーがページサイズを選択できるようにするか、固定値にするかなど、ページネーションの実装方法を検討する。
- 希望する実装に必要なガイダンスをプロンプトに含める。
- Copilot に変更やコンテキストを伝え、反復が必要になる場合があります。これは Copilot を使う通常の作業方法です。

## リソース

- [Copilot の Ask、Edit、Agent モード: それぞれの役割と使いどころ][choose-mode]
- [VS Code のエージェントモード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode

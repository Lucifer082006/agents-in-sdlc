---
title: "レッスン 2 - 星評価の追加: 小さな成果"
description: "Copilot CLI でゲームカードに小さな変更を加え、転送したブラウザーで確認して、最初のプルリクエストとしてマージします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI をインストールして会話を試したので、次はプロジェクトに初めて変更を加えます。変更は小さなものです。ゲームのデータにはすでに星評価がありますが、ホームページのゲームカードには表示されていません。エージェントに評価を表示させ、変更をレビューし、最初のプルリクエストとしてマージします。

このレッスンでは、次の内容を学習します。

- 機能ブランチで Copilot との会話を始める。
- エージェントに小さな変更を依頼する。
- `/diff` で変更をレビューする。
- アプリを実行し、転送されたブラウザーで変更を確認する。
- 最初のプルリクエストを作成してマージする。

## シナリオ

Tailspin Toys の各ゲームには星評価があり、ゲーム詳細ページにはすでに表示されています。しかし、ホームページのゲームカードにはタイトル、カテゴリ、発行元、説明しか表示されません。ウォームアップとして、各カードに既存の評価を表示するようエージェントに依頼します。これは小さく独立した変更なので、最初のセッションに最適です。

## 会話の構成

**会話**は、Copilot CLI でタスクに取り組む場所です。Copilot app とは異なり、通常の CLI 会話では専用の worktree を作成せず、現在ターミナルでチェックアウトされているリポジトリと Git ブランチを使います。保存した会話から後で同じ話し合いを再開できますが、ファイルとブランチはディスク上の通常の Git 状態として保持されます。

会話には、プロンプトとエージェントの応答、ファイルを調査・編集するエージェントのツール操作、`/diff` で確認できる変更の 3 つが表示されます。

## 会話を始め、変更を依頼する

機能の実装に向けて、新しい会話を始めましょう。

1. Codespace に戻ります。
2. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押します。
3. まだ起動していない場合は、次のコマンドで Copilot を起動します。

   ```bash
   copilot --yolo
   ```

4. スラッシュコマンド `/new` を実行して <kbd>Enter</kbd> を押し、新しいセッションを開始します。
5. 次のプロンプトで変更を依頼します。

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot はプロジェクトを調査し、ゲーム詳細の表示に使われているファイルを見つけて、必要なコードを作成します。Copilot CLI で新しい機能を追加できました。

## 差分をレビューする

小さな変更であっても、AI が生成した変更はマージ前にレビューする必要があります。Copilot CLI で変更を確認しましょう。

1. `/diff` を入力し、変更されたすべてのファイルを確認します。
2. ゲームカードに数値の評価がある場合はそれが表示され、`starRating` が `null` の場合は `No rating yet` と表示されることを確認します。
3. テストが両方の状態をカバーしていることを確認します。
4. Copilot が実行したチェックの結果を確認し、失敗があれば修正を依頼します。
5. レビューが終わったら <kbd>Esc</kbd> を押して差分画面を閉じます。

> [!NOTE]
> 生成 AI ツールは確率的に動作するため、Copilot が作成するコードは毎回異なる場合があります。特定の実装と一致するかではなく、動作をレビューしてください。

## 変更を確認する

コードを読むだけで動作すると判断せず、Copilot に Web サイトを起動してもらい、Codespaces から転送されたブラウザーで UI を確認します。

1. 次のプロンプトで Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら、**Open in Browser** を選択します。
3. ゲームカードに 5 段階の評価が表示されることを確認します。
4. Copilot に戻り、起動したサーバーを停止するよう依頼します。

   ```plaintext
   Stop the development server you started.
   ```

## 最初のプルリクエストを作成してマージする

機能を作成できました。新しいコードをプロジェクトにマージするプルリクエスト (PR) を作成しましょう。

1. デフォルトエージェントに、変更をコミットするよう依頼します。

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. `/pr create` を実行します。Copilot CLI は PR の作成時に既存のコミットをプッシュし、PR の URL を表示します。
3. <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら、Copilot CLI が表示した URL を選択して PR を開きます。
4. 変更ファイルとチェックをレビューします。
5. 問題がなければ、**Merge pull request** を選択し、マージを確定します。
6. Codespace に戻り、`/exit` で Copilot CLI を終了します。
7. ローカルの `main` を更新します。

   ```bash
   git checkout main
   git pull
   ```

## まとめと次のステップ

GitHub Copilot CLI を使って、初めての変更をリリースできました。具体的には、次の作業を行いました。

- 機能ブランチで Copilot との会話を始めました。
- エージェントにゲームカードの小さな変更を依頼しました。
- `/diff` で変更をレビューしました。
- アプリを実行し、転送されたブラウザーで星評価を確認しました。
- 最初のプルリクエストを作成してマージしました。

次は、[フィルター機能の Issue から始め、Plan モードと Autopilot モードを使って][next-lesson]、より大きな機能を構築します。

## リソース

- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference

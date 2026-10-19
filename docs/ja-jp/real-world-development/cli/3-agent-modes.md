---
title: "レッスン 3 - エージェントモード: Plan と Autopilot"
description: "Plan でアプローチを決め、Autopilot で Issue に基づいてフィルター機能を実装し、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初に、小さな機能をプロジェクトに追加しました。しかし、大きな変更にはより確かなプロセスが必要です。GitHub Copilot CLI は組織の既存の開発フローに合わせて動作し、適切な方法で必要なものを構築できるよう支援します。このレッスンから、一般的なエージェント主導の開発プロセスに沿って作業します。Issue を基に新機能を作り、コードと機能の動作を検証してから、プロジェクトにマージします。

> [!NOTE]
> 機能の作業を続ける間、同じ会話とブランチを使います。通常はファイルの種類ごとに異なるブランチや PR を使いますが、ここでは主要な概念に集中するため、手順を簡略化しています。

このレッスンでは、次の内容を学習します。

- GitHub Issue から新しい Copilot 会話を始める。
- Plan モードで要件を定義する。
- Autopilot モードで機能を実装する。
- コードをレビューする。
- 転送されたブラウザーで機能を手動検証する。

機能の作業を続けながら、リポジトリの指示を更新し、既存の `quality-checks` スキルをカスタマイズし、MCP による検証を追加して、QA エージェントを作成し、機能 PR を開きます。

## シナリオ

Tailspin Toys のカタログが増えているため、訪問者がカテゴリと発行元でゲームを絞り込めるようにします。バックログの Issue には機能の概要がありますが、カテゴリを組み合わせる方法などの詳細は、コーディング前に合意する必要があります。Plan モードで決定事項を整理してから、範囲を定めた実装を Autopilot に任せます。

## 背景

AI コーディングエージェントを開発フローに導入しても、基本は変わりません。むしろ、より重要になります。多くの開発者は次のような流れで作業します。

1. 実装する内容を記述した Issue から始める。
2. 構築する内容を計画する。
3. コードを作成してレビューする。
4. テストを実行してコードを検証する。
5. 新しい機能を手動で検証する。
6. プルリクエスト (PR) を作成する。
7. コードレビューと継続的インテグレーションが成功したら、コードをマージする。

> [!NOTE]
> 手順の詳細はチームや組織によって異なりますが、多くの場合は上記の流れを基にしています。

この標準的な方法に沿うことで、AI が生成したコードが要件を満たし、手書きのコードと同じ検証プロセスを経るようにできます。

## 会話モード

**会話モード**は、エージェントにどの程度の自律性を与えるかを制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押してモードを切り替えます。

- **Interactive**: エージェントが変更を提案し、次に進む前に入力を待ちます。
- **Plan**: エージェントが先に計画を作成します。プロジェクトファイルは編集できません。
- **Autopilot**: エージェントがコードを書き、テストを実行し、タスクが完了するまで自律的に反復します。

まず Plan モードで計画を確認し、次に Autopilot で実装します。

## Issue から始める

フィルター機能の作業を始める前に、Codespace に戻り、リポジトリとターミナルを準備します。

1. Codespace に戻ります。停止中の場合は再起動してから続けます。
2. 星評価の PR がマージ済みであることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押します。
4. `main` を更新し、フィルター作業用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての Issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの Issue を選択します。
9. <kbd>C</kbd> を押して Issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトの先頭に `#7` などの番号が表示されます。`#` を使うと、GitHub 上の Issue やプルリクエスト (PR) を会話のコンテキストに含められます。

## フィルター機能を計画する

計画を立てると、機能の実装やタスクを Copilot に任せる前に、アプローチを定義できます。複雑な作業では、時間をかけて計画するのがお勧めです。Plan モードに切り替え、Copilot に計画を作成してもらいましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下にあるモード表示が **Plan** であることを確認します。
2. 前の手順で追加した Issue 参照の後に、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot がプロジェクトを調査し、最適な方法を検討して計画を作成します。

3. フィルター機能の動作について質問されたら、希望に沿って回答します。唯一の正解はありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して展開します。
5. 計画を上下にスクロールして確認します。
6. 決定内容と異なる箇所があれば、修正を依頼します。

## Autopilot を承認する

計画を作成してレビューできたので、実装に進みます。Autopilot モードを使って Copilot に作業を任せましょう。

Autopilot を使うと、Copilot はタスクが完了したと判断するまで問題に取り組み続けます。

1. **Accept plan and build on autopilot (recommended)**、またはインストール済みバージョンで同様の名前のオプションを選択します。
2. プロンプトの下にあるモード表示が **Autopilot** であることを確認します。
3. Copilot が計画に沿って反復し、コードを生成してテストを実行する様子を確認します。

> [!NOTE]
> 承認すると、すぐに実装が始まる場合があります。必ず先に計画をレビューしてください。Copilot が依存関係の不足やポートの競合を報告した場合は、チェックが完了したとみなす前にセットアップの問題を解決します。

## 実装をレビューして検証する

生成されたコードは、ほかのコードと同様にマージ前にレビューする必要があります。コードを確認し、サイトを実行して問題がないか確かめましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示に **Plan** や **Autopilot** がないことを確認します。
2. `/diff` を入力して、フィルター機能の実装とテストを確認します。
3. 結果を Issue および計画時に決めた内容と照らし合わせます。
4. レビューが終わったら <kbd>Esc</kbd> を押して差分表示を終了します。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に解決を依頼します。

## 新機能を試す

コードに問題がなくても、実際に動くでしょうか。前と同じようにアプリを起動し、Codespaces の転送ポートを通じてサイトを開きます。

1. 次のプロンプトで Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら、**Open in Browser** を選択します。
3. カテゴリによる絞り込み、発行元による絞り込み、および計画時に決めた組み合わせを試します。
4. リセットと結果なしの場合の動作が、Issue と決定内容に一致することを確認します。
5. Codespace に戻り、Copilot が起動した開発サーバーを停止するよう依頼します。

## まとめと次のステップ

会話モードを使い分けて機能を構築し、レビューしました。このレッスンでは、次の内容を学びました。

- GitHub Issue から新しい Copilot 会話を始める。
- Plan モードで要件を定義する。
- Autopilot モードで機能を実装する。
- コードをレビューする。
- 転送されたブラウザーで機能を手動検証する。

次は[カスタム指示を使い][next-lesson]、コードが文書化された開発慣行に従うようにする方法を学びます。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/

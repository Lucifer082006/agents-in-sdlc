---
title: "レッスン 7 - QA エージェントの作成と使用"
description: "Issue の要件、quality-checks スキル、Playwright MCP を組み合わせる QA カスタムエージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` スキルで自動チェックを実行し、Playwright MCP でブラウザーのフィルター機能を確認しました。次は、明確な QA プロセスを定義するカスタムエージェントに、これらの機能をまとめます。

このレッスンでは、次の内容を学習します。

- カスタムエージェントが指示、スキル、MCP ツールと連携する方法を確認する。
- 再利用可能な品質保証プロファイルを作成して確認する。
- QA エージェントを選択し、フィルター機能を Issue の要件と照らし合わせて評価する。

## シナリオ

Tailspin Toys では、プルリクエスト (PR) を作成する前に、要件、コード品質、自動チェック、テストカバレッジ、ブラウザーでの動作を一貫した方法でレビューしたいと考えています。カスタムエージェントを使うと、この QA プロセスを調整し、再利用可能なレポートを作成できます。

## カスタムエージェントとは

カスタムエージェントは、Markdown プロファイルで定義する Copilot の専門バージョンです。プロファイルには、エージェントの目的、指示、利用可能なツールを記述します。このワークショップでは、`.github/agents/qa.agent.md` で QA の役割を定義し、Copilot CLI で選択します。

これまでに使ったカスタマイズには、それぞれ異なる役割があります。リポジトリの指示はチームの標準を示し、`quality-checks` スキルは繰り返し実行できるチェックをまとめます。Playwright MCP はブラウザーツールを提供します。QA プロファイルは、それらの機能を使って要件を評価し、結果を報告する方法を Copilot に指示します。カスタマイズを置き換えたり、別の会話を必要としたりするものではありません。

## QA プロファイルを作成する

機能 PR を開く前に、再利用可能な QA プロファイルの作成を Copilot に依頼します。プロファイルには、実施するチェックと守るべき制約を定義します。

1. Codespace に戻り、フィルター機能の会話が Interactive モードであることを確認します。
2. デフォルトエージェントに、新しいカスタムエージェントの作成を依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しいエージェントを使う前に、意図した QA の流れと制約が反映されているか確認します。検証だけを依頼したのに、不完全または範囲の広すぎるエージェントが機能を変更するのを防げます。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. フロントマターを読みます。`description` は必須です。`name` は省略可能ですが、含めると表示名が分かりやすくなります。
3. プロファイルの指示を読み、QA が要件から始め、リポジトリの指示に従い、`quality-checks` スキルを実行し、Playwright MCP を使うことを確認します。
4. QA が証拠を示して報告し、実装コードの変更前に確認を求め、変更をコミットせず PR も開かないことを確認します。
5. 必要な役割や制約が欠けていたら、先にデフォルトエージェントへ修正を依頼します。

## Issue に対して QA を実行する

Copilot CLI は会話の開始時にプロジェクトのエージェントを読み込みます。プロファイルを作成した後、同じフィルター機能の会話を再開して QA を選択します。Issue と計画の決定事項をそのまま活用できます。

1. 次のプロンプトでエージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> エージェントを作成した直後は、利用可能なエージェントの一覧に表示されない場合があります。その場合も、上記のコマンドで有効にできます。

2. 次のプロンプトで機能のレビューを依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントが作業を始めます。
4. 作業が完了したら、レポートを確認します。

## まとめと次のステップ

再利用可能な専門的役割をワークフローに追加し、その結果を確認しました。このレッスンでは、次の内容を学びました。

- カスタムエージェントが指示、スキル、MCP ツールと連携する方法を確認しました。
- 再利用可能な品質保証プロファイルを作成して確認しました。
- QA エージェントを選び、フィルター機能を Issue の要件と照らし合わせて評価しました。

実装、スキルの更新、QA プロファイル、テスト、検証レポートが揃いました。次はそれらをまとめて機能 PR を作成し、Agent Merge を使います。

## リソース

- [Copilot CLI 用カスタムエージェントを作成する][create-agents]
- [カスタムエージェントの構成][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration

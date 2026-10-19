---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用可能なカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使って、Tailspin Toys の一連のワークフローに取り組みました。次の作業を行いました。

- Codespace を準備し、Copilot CLI をインストールしてプロジェクトを確認し、フィルター機能の Issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- フィルター機能の Issue から始め、Plan モードでアプローチを定義し、Autopilot モードで実装して、Interactive モードでレビューしました。
- カスタム指示でエージェントをガイドし、既存の `quality-checks` スキルをカスタマイズして、ユニットテスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルター機能を確認しました。
- 品質保証 (QA) カスタムエージェントを作成して選択し、要件、カバレッジ、スキルの結果、ブラウザーでの証拠を評価しました。
- フィルター機能の変更全体をレビューし、その PR で Agent Merge を承認しました。
- コンテキスト、モデル、共有、cloud agent への任意の委任に使うスラッシュコマンドを確認しました。

## リリースした内容

このワークショップには、更新済みの `main` からそれぞれ別のブランチで作成する、2 つの PR の節目があります。

1. **星評価:** 既存の `starRating` と、評価がない場合の明示的な状態をゲームカードに表示します。
2. **フィルター機能と品質ワークフロー:** フィルター機能を実装し、指示を更新して機能に適用し、`quality-checks` のレポートをカスタマイズし、QA プロファイルを作成して関連するテストを追加します。

フィルター機能の計画から PR の作成まで、同じ会話とブランチを使いました。ワークショップを簡略化するため、作業を 1 つの PR にまとめています。

## さまざまな検証方法

自動テスト、自分でのブラウザー確認、MCP を使った Copilot のブラウザー操作など、複数の方法でコードを確認しました。`quality-checks` スキルは新しい形式でユニットテスト、lint、型チェックを実行して報告しました。QA は、要件とテストカバレッジのレビューにその結果を加えました。テストを追加する場合は、実際に不足している部分を補います。QA の実行時にテスト追加が不要と判断されることもあります。マージを承認する前にコードと証拠を確認し、変更後は影響を受ける証拠を取り直してください。

## ベストプラクティス

Copilot に与えるコンテキストとツールは、その作業内容を左右します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成して、MCP サーバーを設定しました。これらのカスタマイズは会話をまたいで再利用し、チームのニーズに応じて調整できます。指示は標準を定め、スキルは繰り返し実行するタスクを記述し、カスタムエージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合った **モードとモデル** を選びます。構築前にアプローチを考えるには **Plan**、範囲を絞った変更に関わり続けるには **Interactive**、範囲の明確なタスクを任せるには **Autopilot** を使います。定型的な編集には高速なモデルを、複雑な作業には高性能なモデルを選びます。

インフラストラクチャと同じくらい、コンテキストも重要です。何を、なぜ、どのように作るのかを明確に説明すると、出力が大きく変わります。

## さらに試す

基本的なワークフローを学びました。次の CLI 機能も試してみてください。

- `/review` でコードレビューエージェントに変更の分析を依頼します。
- `/rubber-duck` で問題について話し合い、別の視点を得ます。
- `/fleet` で独立したサブタスクを並行して進めます。
- `/worktree` で別のタスクを分離します。
- `/delegate` で Copilot cloud agent にタスクを送信します。

## 次のステップ

ツールを使い続けることが、活用方法を身に付ける一番の方法です。実運用のコード、趣味のコード、何年も作りたいと思っていた小さなアプリなどに使ってみてください。学んだことをチームで共有し、ほかのメンバーからも学びましょう。そして、ドキュメントも確認してください。

ターミナルから Tailspin Toys をさらに拡張する場合は、オプションの [Foundry Backer Concierge シリーズ][foundry]に進みます。ほかの環境を比べたい場合は、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]をご覧ください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests

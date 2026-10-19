---
title: "レッスン 8 - 機能 PR の作成とマージ"
description: "フィルター機能、指示、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルター機能の実装、指示の更新、スキルの更新、品質保証 (QA) プロファイル、テストは、1 つのブランチに保存されています。まとめてレビューし、プルリクエストを作成しましょう。星評価の PR は自分でマージしましたが、今回は **Agent Merge** にマージ処理を任せます。

> [!NOTE]
> 通常は機能、指示の更新、スキルの更新、QA エージェントを複数の PR に分けます。このワークショップでは、手順を簡略化するため、フィルター機能と品質に関する作業を 1 つの会話とブランチにまとめ、この PR に含めます。

このレッスンでは、次の内容を学習します。

- Agent Merge と、マージライフサイクルを自動化する方法を学ぶ。
- 機能の変更全体と検証の証拠を確認する。
- フィルター機能の PR を作成する。
- レビュー後にのみ Agent Merge を有効にし、PR がマージされたことを確認する。

## シナリオ

フィルター機能の作業を通じて、Copilot を使って機能を計画、実装、検証してきました。Tailspin Toys では、開発者がマージの承認権を保持したまま、残りの PR 作業を自動化したいと考えています。

## Agent Merge とは

**Agent Merge** は、プルリクエストをマージするまでに必要な残りの作業を自動化します。有効にすると、Copilot は PR を妨げている要因を解消します。たとえば、失敗した継続的インテグレーション (CI) チェックを修正し、レビューコメントに対応し、必要に応じてリベースします。その後、リポジトリで許可されていれば GitHub の自動マージを有効にします。

ここまでは、自分で **Merge pull request** を選択してきました。Agent Merge にその役割を任せることもできます。有効にする前に作業をレビューし、準備ができていることを確認してください。

## Agent Merge で PR を管理する

コードが揃ったら、まとめてレビューして PR を作成し、残りの処理を Agent Merge に任せます。

1. Codespace に戻ります。
2. `/agent` を入力してエージェントダイアログを開きます。
3. 一覧から **Default** を選び、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドで新しい PR を作成します。
5. `/pr agentmerge` コマンドで Agent Merge を有効にします。
6. Copilot が PR の継続的インテグレーションの状態を確認します。すべて成功すると、マージを実行します。
7. Copilot から「PR #14 was squash-merged successfully.」などのメッセージが表示されたことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージキュー、リポジトリ設定、権限不足を回避しません。処理がブロックされた場合は、報告された理由を確認し、リポジトリで許可される場合はレビュー済みの変更を手動でマージしてください。

## まとめと次のステップ

コードの生成、テストと検証、プルリクエストの処理など、開発プロセスのいくつかを自動化しました。このレッスンでは、次の作業を行いました。

- Agent Merge と、マージライフサイクルを自動化する方法を学びました。
- 機能の変更全体と検証の証拠を確認しました。
- フィルター機能の PR を作成しました。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認しました。

次は[便利な Copilot CLI のスラッシュコマンド][next-lesson]を確認し、コンテキスト、モデル、共有、cloud agent への任意の委任について学びます。

## リソース

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference

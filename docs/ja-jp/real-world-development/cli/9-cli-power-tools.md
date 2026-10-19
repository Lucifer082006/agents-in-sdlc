---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュコマンド"
description: "コンテキスト管理、モデルの選択、セッションの共有、cloud agent への任意の委任に使うスラッシュコマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

多くの CLI ツールと同じように、GitHub Copilot CLI には操作用のスラッシュコマンドが多数あります。これらのコマンドでは、高度な機能、内部情報、追加の構成オプションを利用できます。これまでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などを使いました。ここでは、さらに便利なコマンドをいくつか見ていきます。

このレッスンでは、次の内容を学習します。

- `/context` と `/compact` を使い、Copilot が会話のコンテキストを管理する方法を確認する。
- `/model` で利用可能なモデルを確認する。
- `/share` でセッションをエクスポートまたは共有する方法を学ぶ。
- 並列作業、worktree、cloud agent への委任に使う任意のコマンドを確認する。

## シナリオ

CLI の基本的なワークフローを終えました。ここでは、コンテキストの管理、モデルの切り替え、セッションの共有、[Copilot cloud agent][about-cloud-agent] への任意の委任など、追加機能を確認します。

## Copilot CLI のコンテキストを確認する

大規模または複雑なタスクでは、モデルの最大コンテキストウィンドウに達する場合があります。Copilot CLI は必要に応じて会話を自動的に圧縮します。また、スラッシュコマンドを使って状態を確認し、手動で圧縮することもできます。

1. Codespace に戻り、まだ起動していなければリポジトリのルートから Copilot CLI を起動します。
2. 次を入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム指示、ツール、メッセージ、空き領域にコンテキストがどう配分されているかを確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力して結果を比較します。会話が短い場合は、大きな変化がないこともあります。

> [!NOTE]
> コンテキストウィンドウが埋まると、Copilot CLI は自動的に圧縮します。圧縮するタイミングを選ぶ場合は `/compact` を使います。関係のない別のタスクに切り替えて会話を新しくする場合は、`/clear` または `/new` を使います。

## モデルを選ぶ

モデルごとに得意分野があり、開発者によって好みも異なります。Copilot CLI では、使用できるモデルの一覧表示と選択ができます。

1. 次を入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用状況の情報を確認します。
3. 現在のモデルを維持するか、別のモデルを選択するか、<kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで協力し、学びを共有すると、AI ツールの活用を改善できます。`/share` コマンドでは、セッションを Markdown または HTML ファイルにエクスポートしたり、共有リンクを作成したり、GitHub gist を公開したりできます。

1. `/help` を入力し、インストール済みバージョンの `/share` オプションを確認します。
2. セッションを共有する場合は、用途に適した方法を選びます。たとえば `/share file` はローカルの Markdown ファイルにエクスポートします。
3. 誰かに送信したり公開したりする前に、エクスポート内容を確認します。プロンプト、応答、プロジェクトの詳細が含まれる場合があります。

リンクや gist の公開は任意です。チームが共有を意図していないリポジトリや会話の内容を公開しないでください。

## オプション: 作業を拡張、または委任する

ワークショップの基本部分は完了です。Copilot CLI には、より大きなタスク向けのコマンドもあります。

- `/fleet` は独立したサブタスクをサブエージェントに割り当て、並列実行できます。
- `/worktree` は別のタスク用に分離された Git worktree を作成できます。
- `/delegate` は非同期で作業し、プルリクエストを開く場合もある Copilot cloud agent にタスクを送信できます。

追加の worktree やリモート作業が発生する場合があるため、これらのコマンドは任意です。試すときは、範囲を明確にした新しいタスクを用意し、通常のワークフローで結果をレビューしてください。非同期エージェントの作業について詳しく知りたい場合は、[cloud agent ワークショップ][cloud-workshop]に進みます。

## まとめと次のステップ

Copilot CLI のスラッシュコマンドを使うと、構成やセッション共有を行い、内部の動作を確認できます。このレッスンでは、次の内容を学びました。

- `/context` と `/compact` を使い、Copilot が会話のコンテキストを管理する方法を確認しました。
- `/model` で利用可能なモデルを確認しました。
- `/share` でセッションをエクスポートまたは共有する方法を学びました。
- 並列作業、worktree、cloud agent への委任に使う任意のコマンドを確認しました。

Copilot CLI には、ほかにも多くのスラッシュコマンドがあります。学んだ内容を[振り返り][next-lesson]、次のステップを確認して、さらに学習を続けましょう。

## リソース

- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/

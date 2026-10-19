---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

ここまでの演習で、ユーザー体験の向上や機能追加を目的とする 3 つのタスクを Copilot cloud agent に依頼しました。cloud agent は非同期かつ自律的に動作しますが、タスクを監視することも重要です。

GitHub.com の[エージェントページ][agents-page]など、cloud agent に割り当てたタスクを管理するツールが複数あります。このミッションコントロールでは、未解決のプルリクエスト (PR) があるすべてのエージェントタスクを確認できます。実行された操作を調べたり、進行中のセッションに指示を加えたりできます。

このレッスンでは、次の内容を学習します。

- cloud agent のタスクを監視するため、エージェントページを確認する。
- 実行中のセッションに追加機能を依頼して方向付ける。

## シナリオ

エージェントにハイコントラストモードの作成を依頼した後、チームはライトモードも追加するよい機会だと気付きました。サイトのスタイル更新と切り替え機能の作業が進んでいるため、ライトモードも含めるのが自然です。ハイコントラストに加えてライトモードも追加するよう、エージェントの作業を方向付けます。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの現在の状態を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)に移動します。
2. メイン領域と左側の領域に表示されるタスク一覧を確認します。次のような割り当て済みタスクが表示されます。
    - コードベースのドキュメントを更新する。
    - ゲームを変更する API を生成する。
    - Web サイトにハイコントラストモードを追加する。
3. 実行中のタスクを選択し、Copilot が実施した内容を確認します。たとえば、次の操作が行われます。
    - リポジトリのコードをチェックアウトする。
    - Copilot の作業環境を作成する。
    - MCP サーバーを設定する。
    - タスク完了に必要な各種手順を実行する。

> [!NOTE]
> 表示される手順は、Copilot の進捗状況や作業方法によって異なります。

4. 右側に表示される PR 領域も確認します。PR と変更ファイルを確認して、さらに詳しく状況を監視できます。

## cloud agent に指示を追加する

実行中のタスクを確認したので、Copilot がハイコントラストモードを作業中にライトモードの切り替えも追加するよう依頼します。

1. ハイコントラストモードの追加を示すセッションを選択します。Copilot が付けたタイトルや作業状況によって、表示名は異なります。

    ![ミッションコントロールのアクセシビリティセッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了して作業を開始したことを示すメッセージが表示されるまで、数分間セッションを確認します。
3. **Steer active session while Copilot is working** ダイアログに、次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Copilot の作業中にセッションを方向付けるダイアログを表示した agents ページ](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot が依頼を受け取り、作業に含めたことを確認します。

## Copilot に作業を任せる

これまでと同様、Copilot は更新されたタスクに取り組みます。メッセージを送った時点で進めていた手順が終わると、新しい依頼を作業に反映します。

前と同様、完了まで数分かかります。その間に、これまでに学んだことを振り返ってみましょう。

## まとめと次のステップ

GitHub Copilot cloud agent のタスクを監視してガイドする中心的な場所である、Copilot エージェントページを確認しました。ミッションコントロールでは、進行中と完了済みのタスクを追跡し、作業内容をレビューし、実行中のタスクの範囲や追加のガイダンスを調整できます。

次の内容を学びました。

- ミッションコントロールとエージェントページで cloud agent のタスクを監視する。
- 進行中のセッションに追加機能を依頼する。

Copilot がアクセシビリティ機能を実装している間に、Copilot が作成したプルリクエストを[反復して改善する][next-lesson]方法を確認します。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents

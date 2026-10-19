---
title: "演習 5: GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー体験の改善や機能追加を目的とする複数のタスクに GitHub Copilot と取り組みました。コードへのドキュメント追加、デザインチームが改善できる関連ゲーム機能の実装、ハイコントラストモードとライトモードの切り替えを含むアクセシビリティ機能を依頼しました。変更を確認し、必要に応じて Copilot にフィードバックを送り、作業を改善しましょう。

### シナリオ

生成 AI を加えても、ソフトウェア設計や DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに従います。そこで、チームでのレビューを有効にする前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能に関する GitHub Copilot の提案を確認します。

## GitHub Copilot cloud agent のセキュリティ

Copilot cloud agent は非同期かつ監視なしでタスクを実行するため、安全性を確保するセキュリティ制約があります。

- Copilot はリポジトリへの読み取りアクセス権を持ち、コード用ブランチへの書き込みアクセスのみを持ちます。
- cloud agent は GitHub Actions 内で動作し、独立した一時的な環境を作成します。
- GitHub Actions ワークフローは、人間が承認するまで実行されません。
- MCP サーバーを含め、[外部リソースへのアクセスは既定で制限][agent-firewall]されています。

## 生成されたドキュメントをレビューする

まず、コードへのドキュメント追加を目的として GitHub Copilot cloud agent が作成した最初のプルリクエスト (PR) を、GitHub.com の通常の PR インターフェイスで確認します。

> [!NOTE]
> PR を確認する際、GitHub Copilot がファイアウォールでブロックされたという警告が表示される場合があります。Copilot は MCP サーバーなどの外部リソースに既定では制限付きでアクセスするため、これは**想定された動作**です。必要に応じて、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選び、PR の一覧を開きます。
3. **Add missing documentation** などのタイトルが付いた PR を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、PR に **[WIP]** の表示があります。Copilot が作業を終えるまで待ちます。数分かかる場合があるため、その間に休憩したり、学んだことを振り返ったりしてください。

4. PR の準備ができたら **Files changed** タブを選び、変更を確認します。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. TSDoc ドキュメントコメントなど、更新されたコードを確認します。変更内容は実行ごとに異なります。Exercise 2 の最初に確認したカスタム指示ファイルに基づく TSDoc コメント、TypeScript の規則、コメントヘッダーを探します。
6. 更新内容を確認して問題がなければ、**Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示す表示があることを確認します。
8. 承認待ちの場合は **Approve and run workflows** を選択します。

   ![ワークフローを承認して実行するボタン](../../../_images/shared-approve-workflows.png)

9. PR のチェック領域でワークフローがキューに入ったことを確認します。単一 Astro アプリのプロジェクトチェックがすべて成功するはずです。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

Copilot との PR 作業は一方通行ではありません。チームメンバーに依頼するように、PR のコメントやコードのインラインコメントで Copilot にメンションできます。Copilot はコメントを確認し、変更に取り組む別のセッションを開始します。生成結果は毎回異なるため、依頼文を厳密に指定することはできません。たとえば、次のような変更を依頼できます。

- 各コードファイルの先頭に、その役割を簡潔に説明するコメントヘッダーを追加する。
- TypeScript ファイルと Astro ファイルに TSDoc ドキュメントコメントを追加する。
- Astro アプリの構成を説明する README を作成する。

1. 上記の例など、コードベースに必要なドキュメントの変更を選び、通常のユーザーと同じように **@copilot** をメンションしてコメントします。
2. **View Session** を選択し、Copilot の作業を確認します。Copilot は更新用の新しいセッションを開始します。
3. **Back to pull request** を選択して PR に戻ります。

    ![プルリクエストに戻るボタン](../../../_images/ex7-back-to-pr.png)

4. Copilot が変更を完了したら、PR に新しいコミットが追加されていることを確認します。
5. **Files changed** タブを選択して変更をレビューします。

納得できるまで反復して改善してください。問題がなければ、ドラフトからレビュー準備完了に変更し、`main` ブランチにマージできます。

![PR をレビュー準備完了にするボタン](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する Issue に対して、Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルが付いた PR を選びます。
4. **Files changed** タブを選択し、生成されたコードを確認します。
5. 変更を確認して問題がなければ、**Conversation** タブに戻り、下にスクロールします。
6. ワークフローの承認待ちを示す表示があることを確認します。
7. 承認待ちの場合は **Approve and run workflows** を選択します。

   ![ワークフローを承認して実行するボタン](../../../_images/shared-approve-workflows.png)

8. PR のチェック領域でワークフローがキューに入ったことを確認します。単一 Astro アプリのプロジェクトチェックがすべて成功するはずです。完了まで数分かかる場合があります。
9. **省略可能:** Codespace で Copilot が作成したブランチに切り替えて、関連ゲーム機能を手動でテストできます。Codespace に移動してターミナルを開き、次のコマンドを実行します (`<branch-name>` は Copilot が作成したブランチ名、たとえば **copilot/fix-8** に置き換えます)。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を実装しました。Copilot cloud agent に更新を依頼して、さらに改善することもできます。たとえば表示する関連ゲームの数を調整したり、コメントヘッダーや TSDoc ドキュメントコメントを追加したりします (この Issue はカスタム指示を更新する**前**に割り当てたことに注意してください)。前と同様、**Conversation** タブに新しいコメントを追加すると、Copilot が確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ用カスタムエージェントで実装した機能を確認します。この PR には、Exercise 3 で依頼したハイコントラストモードと、Exercise 4 のミッションコントロールから追加依頼したライトモードの両方が含まれます。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルが付いた PR を選びます。

> [!NOTE]
> Copilot がまだ作業中の場合、PR に **[WIP]** の表示があります。作業が完了するまで待ちます。数分かかる場合があります。

4. **Files changed** タブを選択し、生成されたコードを確認します。
5. 実装をレビューし、特に次の点を確認します。
   - モードを切り替える UI コンポーネント
   - ユーザー設定を保存する local storage の使用
   - ハイコントラストモードとライトモードの CSS やスタイル変更
   - アクセシビリティ属性 (ARIA ラベル、キーボード操作など)
   - モード切り替えを管理する JavaScript/TypeScript コード

6. 更新内容を確認して問題がなければ、**Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示す表示があることを確認します。
8. 承認待ちの場合は **Approve and run workflows** を選択します。

   ![ワークフローを承認して実行するボタン](../../../_images/shared-approve-workflows.png)

9. PR のチェック領域でワークフローがキューに入ったことを確認します。単一 Astro アプリのプロジェクトチェックがすべて成功するはずです。完了まで数分かかる場合があります。
10. **省略可能:** Codespace で Copilot が作成したブランチに切り替えて、アクセシビリティ機能を手動でテストできます。Codespace に移動してターミナルを開き、次のコマンドを実行します (`<branch-name>` を Copilot が作成したブランチ名に置き換えます)。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    アプリケーションを起動し、ブラウザーでハイコントラストとライトモードの切り替えをテストします。ページの再読み込み後も設定が維持されることを確認してください。

アクセシビリティ用カスタムエージェントが、アクセシビリティのベストプラクティスに沿って Copilot の実装を支援したことを確認します。懸念点や改善案があれば、前の PR と同じようにコメントで **@copilot** に更新を依頼できます。

## 省略可能な演習 — 委任を続ける

cloud agent は、実際のバックログ項目を任せている間に別の作業を進めると、より効果的に活用できます。リポジトリに Issue をいくつか作成して Copilot に割り当て、習慣にしてみましょう。たとえば次のようなタスクがあります。

- ゲーム詳細ページに支援者向けの関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセスヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent ワークショップを完了しました。各レッスンを通じて、次の作業を行いました。

- cloud agent の出力への影響を後で確認できるよう、リポジトリに含まれるカスタム指示ファイルを確認しました。
- Issue を Copilot cloud agent に割り当て、環境の準備、計画、非同期の作業を確認しました。
- アクセシビリティ用カスタムエージェントを作成して使用し、ハイコントラストとライトモードの切り替えを追加しました。
- Copilot agents ページをミッションコントロールとして使い、進行中のアクセシビリティセッションを監視して方向付けました。
- Copilot のプルリクエストをレビューし、`@copilot` に変更を依頼して、ワークフローを承認しました。

## 振り返りと次のステップ

Cloud agent ワークショップを完了しました。さらに学びたい場合は、別のワークショップで関連する内容を練習できます。

- 🖥️ **[VS Code ワークショップ](../../vscode/)** — IDE から Copilot Chat のエージェントモードと MCP 統合を試します。
- 💻 **[CLI ワークショップ](../../cli/)** — Plan と Autopilot、エージェントスキル、QA カスタムエージェント、Playwright MCP、Agent Merge、cloud agent への `/delegate` を使い、ターミナルからレビュー済みの変更を 2 つリリースします。

自分のリポジトリでは、次のアイデアを試してみてください。

- リファクタリングやテストカバレッジに関する Issue を cloud agent に割り当てる。
- 別の分野向けにカスタムエージェントを作成する。
- 異なる技術スタック用に `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agents について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローに関するベストプラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/

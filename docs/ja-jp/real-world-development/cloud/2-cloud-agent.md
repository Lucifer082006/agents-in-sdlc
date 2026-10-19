---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩んでいない組織はほとんどありません。未解決のセキュリティ問題、更新が必要なレガシーコード、実装する時間がなくバックログに残った機能リクエストなどがあります。GitHub Copilot cloud agent は、コードの更新や機能追加などを自律的に実行します。作業が完了すると、人間の開発者がレビューできるドラフト PR を作成します。面倒な作業を委任して開発を速め、開発者がより大きな課題に集中できるようにします。

この演習では、Copilot cloud agent を使って次の内容を確認します。

- コード生成用の環境をカスタマイズする。
- 操作を安全に行う。
- Issue の範囲を明確にすることの重要性を理解する。
- Issue を Copilot に割り当てる。

## シナリオ

Tailspin Toys には対応したい技術的負債があります。最初のサイトを構築した請負業者は、ドキュメントを十分に整備していません。実際には、ドキュメントがまったくない状態です。まず、アプリケーション内のすべてのエクスポート関数に TSDoc ドキュメントコメントを追加します。

また、デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリのほかのタイトルを「関連ゲーム」として表示し、閲覧を続けられるようにします。まだ完成度の高い実装は必要なく、UX の受け入れテストに使えるものを求めています。これは現在ブロッカーですが、優先度の高い別の Issue もあります。

このような作業は優先順位が下がりやすいため、Copilot cloud agent に任せるのに適しています。開発者がほかの作業を進めている間に非同期で処理し、後から結果をレビューできます。

## GitHub Copilot cloud agent の概要

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同様の方法で、バックグラウンドでタスクを実行できます。たとえば、[GitHub Issue を Copilot に割り当て][assign-issue]ます。割り当てると、Copilot は進行状況を追跡するドラフトのプルリクエストを作成し、環境を準備して、タスクに取り組み始めます。実行中または完了後に Copilot のセッションを確認できます。提案内容のレビュー準備ができると、プルリクエストで通知されます。

## 範囲を明確にした指示の重要性

GitHub Copilot に魔法はありません。考える手間を省けるわけではありません。一見簡単な作業にも複雑さがあるため、[Copilot cloud agent に依頼するタスクの範囲を慎重に決めてください][cloud-agent-best-practices]。AI ペアプログラマーとして扱い、段階的に作業し、学び、試して、必要に応じて調整します。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

これまでの演習で、カスタム指示と Copilot へのガイダンス方法を学びました。カスタム指示の演習を終えている場合は `.github/copilot-instructions.md` の動作を確認済みです。まだの場合も、今が確認するよい機会です。Copilot cloud agent に作業を割り当てる前に、このリポジトリに含まれる指示ファイルを読み取り専用で確認し、後で影響を見つけられるようにします。

リポジトリの GitHub Web UI、または起動済みの codespace で、次のファイルを開きます。

- `.github/copilot-instructions.md` — **Code standards** セクション、特に TypeScript の規則と TSDoc ドキュメントコメントの要件を確認します。
- `.github/instructions/unit-tests.instructions.md` — `applyTo` フロントマターで `**/*.test.ts` ファイルに指示を適用していることを確認します。

次のセクションで `Code lacks documentation` Issue を cloud agent に割り当てたら、作成された PR で TSDoc コメント、TypeScript の規則、コメントヘッダーを確認してください。これらの内容は指示ファイルに基づいています。後の演習で PR をレビューする際にも取り上げます。

## Copilot cloud agent の開発環境をセットアップする

誰がコードを作成する場合でも、通常は環境の準備やセットアップスクリプトの実行が必要です。Copilot にタスクを割り当てる場合も同様です。cloud agent はソフトウェアエンジニアと同じような手順で作業します。

cloud agent は作業環境に [GitHub Actions][github-actions] を使用します。作業開始前に実行する[専用セットアップワークフロー][setup-workflow]を `.github/workflows/copilot-setup-steps.yml` に作成して、必要な開発ツールと依存関係を利用できるようにします。演習の進行を助け、学習機会を提供するため、ここでは事前に設定済みです。Node.js、プロジェクトの依存関係、エンドツーエンドテスト用のブラウザーバイナリ、および単一の Astro アプリ用にマイグレーションとシードを実行した SQLite データベースを Copilot が利用できます。

```yaml
name: "Copilot Setup Steps"

# リポジトリの「Actions」タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 手順で必要な最低限の権限を設定します。Copilot は操作用に独自のトークンを付与されます。
    permissions:
      # たとえば依存関係をインストールするためにセットアップ手順でリポジトリをクローンする場合は、contents: read 権限が必要です。クローンしない場合は、手順の完了後に Copilot が自動で行います。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体が Astro + Drizzle/libSQL です)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ローカル SQLite データベースをマイグレーションし、シードデータを設定します。
      - name: Set up the database
        run: npm run db:setup
```

ほかの GitHub Actions ワークフローと同様の形式ですが、重要な点がいくつかあります。

- `copilot-setup-steps` という単一のジョブがあります。このジョブは、Copilot がプルリクエストの作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使うと、リポジトリの **Actions** タブから手動でワークフローを実行できます。Copilot が実行するのを待たずに、ワークフローの動作を確認できます。

## ドキュメントを追加する

ドキュメントの重要性は広く認識されていますが、多くのプロジェクトでは情報が古いか、まったくありません。こうした技術的負債は放置され、生産性を下げ、コードベースの保守や新しい開発者の参加を難しくします。Copilot はドキュメントの作成を得意とするため、cloud agent に割り当てるのに適した Issue です。Copilot はバックグラウンドで作業し、必要なドキュメントを作成します。後の演習で結果をレビューします。

1. 新しいブラウザータブで github.com のリポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して新しい Issue ダイアログを開きます。
4. **Blank issue** を選択して Issue を作成します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択します。
8. 右側の **Assign to Copilot** を選択して割り当てダイアログを開きます。

  ![Issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

  ![Copilot の割り当て詳細](../../../_images/ex4-assign-copilot-details.png)

10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` などです。PR が一覧に表示されない場合は、少し待ってからブラウザーを更新します。
12. 数分後、Copilot が todo リストを作成したことを確認します。

> [!NOTE]
> Copilot の todo リストが PR に表示されるまで、数分かかることがあります。Copilot は環境を作成し (前述のワークフローを実行し)、プロジェクトを分析して、タスクへの取り組み方を決めています。

13. リストと、実行予定のタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が Issue に取り組み始めたことを示す更新を確認します。
15. **View session** ボタンを選択します。

  ![Copilot セッションの表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された表示が出るまで、ウィンドウの再読み込みが必要な場合があります。

16. ライブセッションをスクロールして Copilot の作業を確認します。コードの調査や現状の把握、計画を考えるための一時停止、コードの作成などが行われます。

作業には数分かかる場合があります。Copilot cloud agent の主な目的は、非同期でタスクを実行し、その間にほかの作業を進められるようにすることです。この機能を活用して、もう 1 つのタスクも Copilot cloud agent に割り当ててから、アプリケーションの機能追加に取り組みます。

## ゲーム詳細ページに関連ゲームを追加する

GitHub Copilot cloud agent の大きな利点は、作業を分担できることです。デザインチーム向けの関連ゲーム表示に時間がかからないとしても、その間にほかの作業を進められます。Copilot cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択します。
4. 空のテンプレートを使うため、**Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択します。
8. 右側の **Assign to Copilot** を選択します。

  ![Issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

しばらくすると、Issue の最初のコメントに 👀 が表示され、Copilot が作業中であることを示します。

![Copilot が Issue の作業中であることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼に取り組んでいます。Copilot cloud agent はソフトウェアエンジニアと同様に作業するため、常に監視する必要はありません。完了後にレビューします。次はカスタムエージェントの作成と使用に進みます。

## まとめと次のステップ

このレッスンでは [GitHub Copilot cloud agent][copilot-agents] を確認しました。cloud agent に Issue を割り当てると、非同期で処理できます。技術的負債への対応、新機能の作成、フレームワーク間のコード移行などに活用できます。

次の内容を学びました。

- コード生成用の環境をカスタマイズする。
- 操作を安全に行う。
- Issue の範囲を明確にすることの重要性を理解する。
- Issue を Copilot に割り当てる。

cloud agent がバックグラウンドで作業している間に、カスタムエージェントの作成と使用に進みます。[Copilot cloud agent は MCP サーバー][cloud-agent-mcp]も使用でき、前のモジュールで確認したカスタム指示も利用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローに関するベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions

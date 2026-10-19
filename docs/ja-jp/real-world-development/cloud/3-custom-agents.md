---
title: "演習 3 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフローの特定のタスクや分野に合わせた専門的な AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルで定義し、Copilot が特定の種類の作業を効果的に行えるよう、焦点を絞った指示、ベストプラクティス、コーディングパターン、分野固有の知識を与えます。チームの知識を再利用可能なエージェントにまとめられます。たとえば [WCAG][wcag] への準拠を支援するアクセシビリティエージェント、セキュアコーディングのプラクティスに従うセキュリティエージェント、一貫したテストパターンを保つテストエージェントを作成できます。

カスタムエージェントは、プロジェクト内の `.github/agents`、またはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには少なくとも `name` と `description` を含む YAML フロントマターがあり、その後にエージェントの動作、専門知識、指示を定める Markdown プロンプトを記述します。

### カスタムエージェントとエージェントスキルの違い

カスタムエージェントと[エージェントスキル][agent-skills-concept]には、役割の重複する部分があります。どちらも主に Markdown ファイルで定義し、AI に操作方法を伝えます。最も簡単な区別は、**カスタムエージェントが作業者で、スキルがツール**ということです。

カスタムエージェントには独自のコンテキストウィンドウがあり、作業の一環としてスキル (さらにはほかのエージェント) を調整します。この演習では、アクセシビリティ用のカスタムエージェントがガイドラインに沿ってサイトをレビューし、更新します。プルリクエストのワークフロースキルや、テストを実行・管理するスキルなどを利用できます。

> [!NOTE]
> カスタムエージェントの作成に唯一の正解はありません。AI を使うほかの作業と同様、環境やシナリオに合わせてテストし、調整してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

この演習では、カスタムエージェントを使って次の内容を学びます。

- カスタムエージェントの定義方法。
- カスタムエージェントへのタスクの割り当て方。

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディングプラットフォームを利用できるように取り組んでいます。最近、テキストと背景色のコントラストが不足しているため、現在のダークテーマを読みにくいという意見が寄せられました。デザインチームは、切り替え可能なハイコントラストモードの実装を依頼しました。

アクセシビリティは重要なので、できるだけ早く実装したいと考えています。カスタムエージェントを使って機能を生成します。

## アクセシビリティ用カスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントはすでに作成されています。Copilot をガイドする方法を理解するため、内容を確認します。

codespace に戻り、アクセシビリティエージェントのファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドを含む YAML フロントマターを確認します。

> [!CAUTION]
> カスタムエージェントには、`name` と `description` を含むフロントマターが必須です。

3. 続くセクションを読み、次の内容を確認します。
   - アクセシブルな Web サイトのコードを生成する際の主要な責務。
   - アクセシビリティのベストプラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある問題や間違い。

## Issue を作成して割り当てる

ミッションコントロールは、環境内のエージェントを操作する中心的な場所です。Copilot cloud agent にタスクを割り当て、進捗を監視し、追加の指示を与えられます。まず、ハイコントラストモードの作成タスクを Copilot に割り当てます。

1. リポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択します。
4. **Blank issue** を選択します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次を入力します。

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. **Create** を選択します。
8. 右側の **Assign to Copilot** を選択して割り当てダイアログを開きます。
9. 一覧から **Accessibility agent** を選択します。

    ![カスタムエージェントとアクセシビリティエージェントを選択した cloud agent の割り当て画面](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドでタスクに取り組み始めます。

## まとめと次のステップ

このレッスンでは、特定のタスクや分野に合わせた GitHub Copilot の専門的な AI アシスタントである[カスタムエージェント][custom-agents]を確認しました。チームの知識や標準を再利用可能なエージェントにまとめ、Copilot が効率よく作業できるようにします。

次の内容を学びました。

- カスタムエージェントの定義方法。
- カスタムエージェントへのタスクの割り当て方。

Copilot がハイコントラストモードを実装している間に、ミッションコントロールから[エージェントセッションを監視して方向付ける][next-lesson]方法を確認します。

## リソース

- [カスタムエージェントについて][custom-agents]
- [組織でカスタムエージェントを使う準備][org-custom-agents]
- [エンタープライズでカスタムエージェントを使う準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents

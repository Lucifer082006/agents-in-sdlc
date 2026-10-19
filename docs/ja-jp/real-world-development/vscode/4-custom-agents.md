---
title: "演習 4 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフローの特定のタスクや分野に合わせた専門的な AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルで定義し、Copilot が特定の種類の作業を効果的に行えるよう、焦点を絞った指示、ベストプラクティス、コーディングパターン、分野固有の知識を与えます。チームの知識を再利用可能なエージェントにまとめられます。たとえば [WCAG][wcag] への準拠を支援するアクセシビリティエージェント、セキュアコーディングのプラクティスに従うセキュリティエージェント、一貫したテストパターンを保つテストエージェントを作成できます。

カスタムエージェントは、プロジェクト内の `.github/agents`、またはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには少なくとも `name` と `description` を含む YAML フロントマターがあり、その後にエージェントの動作、専門知識、指示を定める Markdown プロンプトを記述します。

### カスタムエージェントとエージェントスキルの違い

カスタムエージェントと[エージェントスキル][agent-skills-concept]には、役割の重複する部分があります。どちらも主に Markdown ファイルで定義し、AI に操作方法を伝えます。最も簡単な区別は、**カスタムエージェントが作業者で、スキルがツール**ということです。

カスタムエージェントには独自のコンテキストウィンドウがあり、作業の一環としてスキル (さらにはほかのエージェント) を調整します。このラボでは、アクセシビリティ用のカスタムエージェントがガイドラインに沿ってサイトをレビューし、更新します。その作業の一環として、プルリクエストのワークフロースキルや、テストを実行・管理するスキルなどを呼び出せます。

> [!NOTE]
> カスタムエージェントの作成に唯一の正解はありません。AI を使うほかの作業と同様、環境やシナリオに合わせてテストし、調整してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディングプラットフォームを利用できるように取り組んでいます。最近、テキストと背景色のコントラストが不足しているため、現在のダークテーマを読みにくいという意見が寄せられました。デザインチームは、切り替え可能なハイコントラストモードの実装を依頼しました。

アクセシビリティは重要なので、できるだけ早く実装したいと考えています。カスタムエージェントを使って機能を生成します。

この演習では、次の内容を学びます。

- 既存のアクセシビリティ用カスタムエージェントを確認する。
- Copilot Chat でアクセシビリティエージェントを使い、ハイコントラストモードを実装する。

## アクセシビリティ用カスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントはすでに作成されています。Copilot をガイドする方法を理解するため、内容を確認します。

codespace に戻り、ターミナルを開いて、アクセシビリティ作業用の新しいブランチを `main` から作成します (Exercise 3 のフィルター機能 PR とは分けて作業します)。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドを含む YAML フロントマターを確認します。

> [!CAUTION]
> カスタムエージェントには、`name` と `description` を含むフロントマターが必須です。

3. 続くセクションを読み、次の内容を確認します。
   - アクセシブルな Web サイトのコードを生成する際の主要な責務。
   - アクセシビリティのベストプラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある問題や間違い。

## Copilot Chat でカスタムエージェントを使う

VS Code の Copilot Chat ビュー下部にあるエージェントドロップダウンには、`.github/agents` で定義されたすべてのカスタムエージェントが表示されます。カスタムエージェントを選択すると、その指示とツールを適用したチャットセッションを開始できます。

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に、codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習ごとに異なります。必要な箇所で説明します。

1. Chat ビューのエージェントドロップダウンで、まだ選択されていなければ **Agent** を選びます。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

2. Chat ビューの下部にあるエージェントドロップダウンを選択します (既定では、アクティブなエージェントとして **default** が表示されます)。
3. 利用可能なエージェントの一覧から **Accessibility agent** を選択します。
4. アクセシビリティエージェントに次のプロンプトを送信します。

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat がコードベースを調査し、変更を提案して、プロジェクトファイルに適用します。編集内容はファイルパスと差分とともにチャット内に表示されます。
6. エージェントが変更したファイルを示す **Files changed** の表示が更新されます。

> [!NOTE]
> この作業には数分かかる場合があります。Copilot は Astro コンポーネント、CSS、関連テストなどの既存ファイルを実際に変更します。

次の演習では、進行中の作業をレビューして方向付けます。

## まとめと次のステップ

特定のタスクや分野に合わせた GitHub Copilot の専門的な AI アシスタントである[カスタムエージェント][custom-agents]を確認しました。チームの知識や標準を再利用可能なエージェントにまとめ、Copilot が効果的に作業できるようにします。

次の内容を学びました。

- カスタムエージェントの定義方法。
- Copilot Chat のエージェントモードでカスタムエージェントを使う方法。

次は[エージェントの作業を監視して方向付け][next-lesson]、変更を確認しながら同じセッションにライトモードの切り替えを追加します。

## リソース

- [カスタムエージェントについて][custom-agents]
- [IDE でカスタムエージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタムエージェント][custom-agents-vscode]
- [カスタムエージェントの構成][custom-agents-config]
- [awesome-copilot のカスタムエージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents

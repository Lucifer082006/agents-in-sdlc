---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使う際はコンテキストが重要です。タスクを特定の方法で実行したい場合や、Copilot に知っておいてほしい背景情報がある場合は、その情報を参照できるようにします。[指示ファイル][instruction-files]を使うと、Copilot に実行する内容だけでなく、その方法も伝えられます。

この演習では、次の内容を学びます。

- リポジトリのカスタム指示とパスを限定した指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot にどのように届くかを確認する。
- リポジトリ全体に適用する標準を `.github/copilot-instructions.md` に追加する。

> [!NOTE]
> VS Code や CLI のワークショップとは異なり、ここでは変更前後のプロンプトを実行しません。Copilot cloud agent は GitHub Issue に対して非同期に動作するため、リアルタイムで並べて影響を示すのが難しいからです。後の演習で cloud agent が作成したプルリクエストをレビューする際に、指示ファイルの影響を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。

- データ層には必ずユニットテストを用意します。
- UI はダークモードにし、現代的な印象にします。
- TSDoc ドキュメントコメントを使ってコードに説明を加えます。
- 各ファイルの先頭に、そのファイルの役割を説明するコメントブロックを追加します。

指示ファイルを使い、Copilot がこれらのプラクティスに沿って作業できるようにします。

### カスタム指示

カスタム指示を使うと、コーディングスタイルや要件のコンテキストと好みを Copilot に伝えられます。推奨するコーディング規則、ライブラリ、コメントの書き方などを指定して、提案やコードスニペットを調整できます。リポジトリ全体、または特定の種類のファイルに指示を作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対するすべてのリクエストで Copilot に送られる単一の指示ファイルです。Copilot Chat や CLI の多くのリクエストに関係する技術スタック、構築中のものの概要、ベストプラクティスなど、プロジェクト全体の情報を記述します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル種別に適用できます。TypeScript や Astro などの言語、UI コンポーネントやユニットテストなどのタスク向けのガイドラインを提供できます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリレベルの指示と、`applyTo` フロントマターで適用先を指定した `*.instructions.md` ファイルの両方を使用します。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの標準に基づく指示ファイルにも対応しています。[サポートされる形式][custom-instructions-support]をご覧ください。

### 指示ファイルの管理に関するベストプラクティス

指示ファイルの作成を詳しく扱うことは、このワークショップの範囲外です。サンプルプロジェクトには、参考となる方法が含まれています。概要は次のとおりです。

- `copilot-instructions.md` には、構築するものやプロジェクト構成、全体に適用するコーディング標準など、プロジェクトレベルのガイダンスを記述します。
- `*.instructions.md` には、ユニットテスト、Astro コンポーネント、データ層などのファイル種別や、特定のタスクに適用する指示を記述します。
- 自然な言葉で明確に書き、コードの良い例と悪い例を示します。

指示ファイルの作り方や AI の使い方に唯一の正解はありません。実際に試しながら、プロジェクトに合う方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトでは、充実した指示ファイルを用意すると役立ちます。このプロジェクトには [UI の更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けのファイルがあります。
>
> Copilot に指示ファイルを作成してもらうこともできます。操作方法は環境ごとに異なります (たとえば VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init`)。このワークショップでは、必要な箇所で説明します。
>
> テンプレートや出発点を探す場合は、指示ファイル、カスタムエージェントなどを集めた [awesome-copilot][awesome-copilot] をご覧ください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに含まれる指示ファイルを読みます。共通の `copilot-instructions.md` と、各タスク向けの `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要と **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内の **GitHub Actions Workflows** に関するガイダンスも確認してください。これらは Copilot とのすべてのやり取りに適用されます。
3. `.github/instructions` フォルダーを開き、内容を確認します。Astro ファイル、Drizzle データ層、テストなどの指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。リポジトリのルートを基準とする glob で、適用対象を指定します。たとえば `**/*.test.ts` に一致する TypeScript テストファイルが対象です。
5. このプロジェクトでユニットテストを作成する際の指示を確認します。
6. `.github/instructions/drizzle.instructions.md` の末尾にあるリンクを確認します。`unit-tests.instructions.md` などの指示ファイルや、プロジェクトの既存ファイルを参照しています。指示を小さな再利用可能なファイルに分け、コード生成時の例を示せます (ここにあるパスはリポジトリのルートではなく、指示ファイルからの相対パスです)。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションには、プロジェクトのコーディング標準が記載されていますが、コード内ドキュメントの規則はまだありません。次の手順で、TSDoc コメントとファイルヘッダーの規則を追加します。

## リポジトリ全体に適用する標準を追加する

この演習で行う唯一の編集として、TSDoc ドキュメントコメントとファイルレベルのコメントヘッダーをコードに記述するプロジェクト全体の規則を追加します。以後このワークショップで Issue に取り組む際に、cloud agent がこの指示を読み込みます。

編集前に、新しいブランチを用意します (cloud agent は Issue の対象ブランチから指示を読み込みますが、ここでは編集を整理してコミットします)。

1. codespace のターミナルで次を実行し、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

先ほど確認したとおり、`.github/copilot-instructions.md` は Copilot にプロジェクト全体の情報を提供します。コーディング標準を文書化して、コードの提案を改善しましょう。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。既存の標準は説明されていますが、コード内ドキュメントの規則がありません。そのため、生成されたヘルパーにもドキュメントコメントがありませんでした。
3. 次の Markdown を既存の標準の下に追加し、ファイルヘッダーと TSDoc ドキュメントコメントを追加するよう Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで説明したように、指示はリポジトリレベル (`.github/copilot-instructions.md`) または特定の言語、ファイル種別、タスク向け (`*.instructions.md`) に作成できます。追加したドキュメントコメントの規則のようなプロジェクト全体の標準には、リポジトリレベルのファイルが適しています。

## 指示の変更をコミット、プッシュ、マージする

cloud agent は、Issue の対象ブランチにある指示ファイルを読み込みます。次の演習では `main` からブランチを作成して Copilot に Issue を割り当てるため、指示の変更を `main` に反映する必要があります。

1. ステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` に向けたプルリクエストを開き、マージします。次の演習で Issue に取り組む際、cloud agent はこの指示を読み込みます。

> [!TIP]
> このワークショップで `main` に直接作業したい場合は、ブランチの作成を省略し、`main` に直接コミットできます。実際のプロジェクトでの作業方法を体験できるよう、ここではブランチを使います。

## まとめと次のステップ

このプロジェクトの指示ファイルから Copilot がコンテキストを取得する方法を確認し、リポジトリ全体に適用する標準を `.github/copilot-instructions.md` に追加しました。以降の演習で cloud agent が作成するプルリクエストをレビューするときに、この標準が反映されていることを確認します。

次は [GitHub Copilot cloud agent に最初の Issue を割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/

---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェントモード →][next-lesson]

生成 AI を使う際はコンテキストが重要です。タスクを特定の方法で実行したい場合や、Copilot に知っておいてほしい背景情報がある場合は、その情報を参照できるようにします。Copilot と特定のコンテキストを共有する方法はいくつかあります。重要な方法の 1 つが[指示ファイル][instruction-files]です。コード生成のガイダンスを提供できます。

この演習では、次の内容を学びます。

- リポジトリのカスタム指示とパスを限定した指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot にどのように届くかを確認する。
- 現在の指示のまま、フィルター機能のデータスライス第 1 段階として発行元ヘルパーを生成する。
- リポジトリ全体に適用する標準を `.github/copilot-instructions.md` に追加する。
- 同じプロンプトを再実行し、生成コードに新しい標準が反映されることを確認する。
- 指示の更新とフィルター機能の基礎を `main` にコミットし、次の演習で Copilot が更新済みガイダンスを使えるようにする。

> [!CAUTION]
> 生成コードが設定した標準から外れることもあります。Copilot は確率的に動作します。この演習の目的は、指示を更新した後の傾向の変化を確認することであり、出力を文字単位で一致させることではありません。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。

- データ層には必ずユニットテストを用意します。
- UI はダークモードにし、現代的な印象にします。
- TSDoc ドキュメントコメントを使ってコードに説明を加えます。
- 各ファイルの先頭に、そのファイルの役割を説明するコメントブロックを追加します。

指示ファイルを使い、Copilot がこれらのプラクティスに沿って作業できるようにします。

### カスタム指示

カスタム指示を使うと、コーディングスタイルや要件のコンテキストと好みを Copilot に伝えられます。コーディング規則、ライブラリ、コメントの書き方などを指定して、提案やコードスニペットを調整できます。リポジトリ全体、または特定の種類のファイルに指示を作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対するすべてのリクエストで Copilot に送られる単一の指示ファイルです。技術スタック、構築中のものの概要、ベストプラクティスなど、プロジェクト全体に関する情報を記述します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル種別に適用する指示ファイルです。TypeScript や Astro などの言語、UI コンポーネントやユニットテストなどのタスク向けにガイドラインを設定できます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリレベルの指示と、`applyTo` フロントマターで適用先を指定した `*.instructions.md` ファイルの両方を使用します。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの標準に基づく指示ファイルにも対応しています。[サポートされる形式][custom-instructions-support]をご覧ください。

### 指示ファイルの管理に関するベストプラクティス

指示ファイルの作成を詳しく扱うことは、このワークショップの範囲外です。サンプルプロジェクトには、参考となる方法が含まれています。

- `copilot-instructions.md` には、構築するものやプロジェクト構成、全体に適用するコーディング標準など、プロジェクトレベルのガイダンスを記述します。
- `*.instructions.md` には、ユニットテスト、Astro コンポーネント、データ層などのファイル種別や、特定のタスク向けの指示を記述します。
- 自然な言葉で明確に書き、コードの良い例と悪い例を示します。

指示ファイルの作り方や AI の使い方に唯一の正解はありません。試しながら、プロジェクトに合う方法を見つけてください。

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

スタータープロジェクトに含まれる共通の `copilot-instructions.md` と、各タスク向けの `*.instructions.md` ファイルを読みます。エディターまたは GitHub Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要と **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内の **GitHub Actions Workflows** に関するガイダンスも確認してください。これらは Copilot とのすべてのやり取りに適用されます。
3. `.github/instructions` フォルダーを開きます。Astro ファイル、Drizzle データ層、テストなどの指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。リポジトリのルートを基準にした glob で、適用対象を指定します。たとえば `**/*.test.ts` に一致する TypeScript テストファイルが対象です。
5. このプロジェクトでユニットテストを作成する際の指示を確認します。
6. `.github/instructions/drizzle.instructions.md` の末尾にあるリンクを確認します。`unit-tests.instructions.md` などの指示ファイルや、プロジェクトの既存ファイルを参照しています。これにより、指示を小さな再利用可能なファイルに分け、Copilot がコード生成時に参照する例を示せます (パスはリポジトリのルートではなく、この指示ファイルからの相対パスです)。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションには、プロジェクトのコーディング標準が記載されていますが、コード内ドキュメントの規則はまだありません。次の手順で TSDoc コメントとファイルヘッダーの規則を追加します。

## 変更用のブランチを作成する

ベストプラクティスに従い、変更用のブランチを作成します。

1. 前の演習で使った codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に、codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習ごとに異なります。必要な箇所で説明します。

カスタム指示の影響を確認するため、まず現在の指示ファイルを使ってプロンプトを送信します。その後、指示を更新して同じプロンプトを再送し、違いを確認します。

1. 前の演習で開いたエディタータブを閉じ、Copilot に必要なコンテキストだけを与えます。
2. `src/lib/publishers.ts` を開き、ヘルパーの配置場所を Copilot に知らせます。
3. Chat ビューのエージェントドロップダウンで **Agent** を選び、Copilot がファイルを変更できるようにします。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot はプロジェクトを調査してコードを変更します。ヘルパーファイルとテストなど、複数のファイルを変更する場合があります。
6. 提案されたヘルパーが型付き関数であり、最初の引数に `db` クライアントを取り、型付きの発行元配列を返すことを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ層規則に基づいています。
7. 生成コードに TSDoc ドキュメントコメントとファイルレベルのコメントヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくてもドキュメントコメントを追加する場合があります。その場合も問題ありません。指示を更新した後に、一貫性が改善されることを確認するのが目的です。

## リポジトリ全体に適用する標準を追加する

`.github/copilot-instructions.md` は Copilot にプロジェクト全体の情報を提供します。コーディング標準を文書化して、コードの提案を改善しましょう。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. 27 行目付近の **Code formatting requirements** セクションを探します。既存のコーディング標準は説明されていますが、コード内ドキュメントの規則がありません。そのため、生成されたヘルパーにもドキュメントコメントがありませんでした。
3. 次の Markdown を既存の標準の下に追加し、ファイルヘッダーと TSDoc ドキュメントコメントを Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 指示はリポジトリレベル (`.github/copilot-instructions.md`) または特定の言語、ファイル種別、タスク向け (`*.instructions.md`) に作成できます。追加したドキュメントコメントの規則のようなプロジェクト全体の標準には、リポジトリレベルのファイルが適しています。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話を新しくします。
2. `src/lib/publishers.ts` を選択し、Copilot が適切なファイルに注目するようにします。
3. 先ほどと**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメントブロックがあることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 生成された関数に、次のような TSDoc コメントが含まれていることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Copilot に新しいプロジェクト標準を適用させ、次の演習で基盤として使うコードに反映できました。

## 指示ファイルをコミットしてブランチをプッシュする

指示ファイルもリポジトリのほかのファイルと同様、ソース管理で管理します。変更をコミットしてブランチをプッシュしましょう。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押し、新しいターミナルを開きます。
2. 次のコマンドを実行し、指示とヘルパーの変更が存在することを確認します。

   ```bash
   git status
   ```

3. 指示の更新と、Copilot がヘルパーの基盤用に変更したすべてのファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status に追加の関連ファイル (テストなど) が表示された場合は、それらも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、バックログのドキュメント標準の Issue に関連付けてプルリクエストを作成します。手動でもできますが、GitHub ツールを使える Copilot に依頼します。Issue を探し、Issue をクローズする PR を作成してからマージするようプロンプトで伝えます。

> [!NOTE]
> Copilot に GitHub 操作を依頼するのはこれが初めてです。プロジェクトテンプレートでは GitHub ツールがワークスペースに接続済みなので、セットアップは不要です。Copilot が初めてツールを使う際に、VS Code から GitHub へのサインインを求められる場合があります。プロンプトに従って許可してください。後の演習で Model Context Protocol による接続方法を学びます。

1. codespace 内で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェントピッカーで **Agent** が選択されていることを確認します。
3. 指示ファイルの更新に関する Issue を探し、現在のブランチから PR を作成するよう Copilot に依頼します。指示の更新と、今後のフィルター機能のために発行元ヘルパーの基盤も追加したことを説明に含めます。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が Issue を見つけて PR を作成します。
5. Copilot が GitHub 操作の **Allow** を求めたら、実行内容を確認し、適切な場合のみ許可します。
6. PR が作成されたら、次のプロンプトでマージしてローカルのブランチを `main` に戻すよう依頼します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot が GitHub 操作やシェルコマンドの **Allow** を求めたら、実行内容を確認し、適切な場合のみ許可します。

GitHub Copilot の支援でプルリクエストを作成し、マージできました。

## まとめと次のステップ

このプロジェクトの指示ファイルから Copilot がコンテキストを取得する方法を確認し、VS Code の Copilot Chat で次の作業を行いました。

- 既存の指示でコード生成プロンプトを送信し、Copilot の出力を確認しました。
- リポジトリ全体に適用する標準を `.github/copilot-instructions.md` に追加しました。
- 同じプロンプトを再実行し、生成コードに新しい標準が反映されることを確認しました。
- 次の演習で基盤として使えるよう、指示とヘルパーの変更を `main` にコミットしました。

次は[エージェントモード][next-lesson]を使い、コードベース全体にわたる新機能の追加に指示を適用します。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/

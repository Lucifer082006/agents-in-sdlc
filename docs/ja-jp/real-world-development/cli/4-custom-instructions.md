---
title: "レッスン 4 - カスタム指示で Copilot をガイドする"
description: "リポジトリの指示を確認し、ドキュメント標準を追加して、フィルター機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使う際はコンテキストが重要です。特定の方法でタスクを実行したい場合は、その指針を Copilot が参照できるようにします。[指示ファイル][instruction-files]を使うと、作成するコードだけでなく、その構成方法も伝えられます。フィルター機能を構築したので、Copilot が参照した指示を確認し、ドキュメント標準を追加して、コードに適用します。

このレッスンでは、次の内容を学習します。

- リポジトリの指示とパスを限定した指示ファイルが、エージェントにどのように適用されるかを確認する。
- コーディング標準に従うよう指示ファイルを更新する。
- 指示ファイルがコードに与える影響を確認する。

## シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。

- コメントはコードを言い換えるのではなく、意図や自明でない判断を説明します。
- `db/` と `src/lib/` のエクスポートされた関数には、目的、パラメーター、戻り値を説明する TSDoc/JSDoc を記述します。該当する場合は、注入可能な `db` 引数も含めます。
- 再利用可能な Astro コンポーネントでは `Props` の契約を文書化し、関連するコードの変更時にはコメントも最新に保ちます。
- 既存の書式と lint のガイダンスを維持します。

指示ファイルを使い、Copilot がこれらのプラクティスに沿ってタスクを実行できるようにします。

## 指示ファイル

カスタム指示を使うと、コーディングスタイルや要件のコンテキストと好みを Copilot に伝えられます。推奨するコーディング規則、ライブラリ、コメントの書き方などを指定できます。リポジトリ全体、または特定の種類のファイルに指示を設定できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対するすべてのリクエストで Copilot に送られる単一の指示ファイルです。多くの Copilot Chat や CLI のリクエストに関係する技術スタック、構築中のものの概要、ベストプラクティスなど、プロジェクト全体の情報を記述します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル種別、タスク向けのガイドラインを提供します。

> [!NOTE]
> その他の指示形式とサポート状況は、環境によって異なります。特定の形式を前提にする前に、[カスタム指示のサポートに関するリファレンス][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

スタータープロジェクトには、あらかじめ指示ファイルが含まれています。変更する前に内容を確認し、影響を確かめましょう。

1. Codespace に戻ります。
2. Codespaces エディターで (ターミナルではなく) `.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディングガイダンスを確認します。これらの指示は、このリポジトリで Copilot を使うすべての操作に適用されます。
4. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro ファイル、Drizzle データ層、テストなどの指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。この glob に一致するファイルに指示が適用されます。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの指示ファイルやプロジェクト内のファイルへのリンクを確認します。これにより、指示を小さな再利用可能なファイルに分け、Copilot がコード生成時に参考にする例を示せます。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting guidance** セクションには、プロジェクトのコーディング標準が記載されていますが、コード内ドキュメントの規則はまだありません。次の手順で TSDoc コメントの規則を追加します。

## チームのガイダンスに合わせて指示ファイルを更新する

既存の指示ファイルは良い出発点ですが、改善の余地があります。`copilot-instructions.md` を変更し、新しく生成する TypeScript に TSDoc コメントを追加するよう指示します。

1. `.github/copilot-instructions.md` の **Code formatting guidance** セクション (35 行目付近) を探します。
2. 最後の箇条書きとして、次の行を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを適用する

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。指示を保存したら、機能のコンテキストを保ったままフィルター機能の会話を終了して再開します。

1. フィルター機能の会話がまだ開いている場合は、`/exit` を入力して終了します。Copilot CLI は会話を自動的に保存します。
2. リポジトリのルートにある `game-filters-cli` ブランチから、保存済みの会話を再開します。

   ```bash
   copilot --resume
   ```

   プロンプトが表示されたらフィルター機能の会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. Copilot に更新済みガイダンスを適用するよう依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力して、変更された TypeScript ファイルを確認します。追加された TSDoc コメントがコードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot が指示ファイルからコンテキストを取得する方法を確認し、新しい標準を機能に適用しました。具体的には、次の作業を行いました。

- リポジトリの指示とパスを限定した指示ファイルが、エージェントに適用される仕組みを確認しました。
- コーディング標準に従うよう指示ファイルを更新しました。
- 指示ファイルがコードに与える影響を確認しました。

次は、再利用可能な [quality-checks スキルをカスタマイズして実行し][next-lesson]、lint とテストを一貫して実行する方法を学びます。

## リソース

- [Copilot CLI にカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート][custom-instructions-support]
- [カスタム指示作成のベストプラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/

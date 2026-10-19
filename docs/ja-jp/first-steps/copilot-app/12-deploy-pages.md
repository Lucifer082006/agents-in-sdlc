---
title: "レッスン 12 - GitHub Pages でサイトをデプロイする"
description: "GitHub Copilot app を使って Pages ワークフローを設定し、プルリクエストをレビューして、Space Quiz の公開を確認します。"
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

GitHub Pages で Space Quiz をデプロイし、誰でもアクセスできるようにします。Copilot はワークフローの準備とデプロイを支援しますが、変更をレビューし、サイトが公開されたことを確認するのは自分の役割です。

このレッスンでは、次の内容を学習します。

- Copilot に GitHub Pages の設定を依頼する。
- マージする前にワークフローとプルリクエストをレビューする。
- デプロイの進行を確認し、公開されたサイトを検証する。

## シナリオ

Space Quiz はすでに GitHub リポジトリにありますが、簡単に試せる方法が必要です。単一ファイルのサイトを GitHub Pages で公開すれば、サーバーや実行時の依存関係を追加せずに、公開 URL からアクセスできるようになります。

## Copilot にサイトのデプロイを依頼する

`space-quiz` の Copilot app セッションで、次のプロンプトを送信します。

```plaintext
Enable GitHub Pages for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and merge it after required checks pass. Monitor the deployment until it succeeds, then report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![3 段階の図: マージ済みの space-quiz プルリクエスト、Build website、Upload artifact、Deploy to Pages の各ステップが成功した GitHub Actions の Deploy website 実行、yourname.github.io/space-quiz/ で公開された Space Quiz。](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> このプロンプトでは、エージェントにプルリクエストの作成とマージ、およびリポジトリの Pages 設定の変更を依頼します。続行する前に、差分、デプロイの権限、GitHub の承認要求を確認してください。

## ワークフローをレビューしてデプロイを確認する

1. リポジトリに README がない場合に限り、README が追加されていることを確認します。
2. ビルドとデプロイの手順を含め、プルリクエストの変更をレビューします。
3. 必須のプルリクエストチェックが成功した後にのみマージします。
4. GitHub Actions のデプロイ実行が成功するまで確認します。
5. 報告された URL を開き、Space Quiz が読み込まれることを確認します。

エージェントが Pages を有効にしたり、プルリクエストをマージしたりできない場合は、提示された手順に従ってください。デプロイが成功し、公開サイトが読み込まれるまで、タスクを完了とみなさないでください。

## まとめと次のステップ

Space Quiz をライブサイトとして公開し、エージェントの報告だけに頼らず、実際にデプロイを確認しました。[レッスン 13: 振り返りと次のステップ][next-lesson]に進みます。

[next-lesson]: ../13-review/

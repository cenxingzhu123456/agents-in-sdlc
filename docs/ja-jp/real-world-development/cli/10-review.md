---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用可能なカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使い、Tailspin Toys の一連のワークフローを実践しました。次のことを行いました。

- Codespace を準備し、Copilot CLI をインストールして、プロジェクトを確認し、登録済みの絞り込み issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- 絞り込み issue から始め、Plan モードで方針を定め、Autopilot モードで実装し、Interactive モードでレビューしました。
- カスタム指示でエージェントを導き、既存の `quality-checks` スキルをカスタマイズして、ユニットテスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーで絞り込みを確認しました。
- 品質保証 (QA) カスタムエージェントを作成して選択し、要件、カバレッジ、スキルの結果、ブラウザーでの検証結果を評価しました。
- 絞り込み機能の変更全体をレビューし、Agent Merge に絞り込み PR のマージを許可しました。
- コンテキスト、モデル、共有、任意のクラウドへの委任に使えるスラッシュコマンドを確認しました。

## 作成したもの

このワークショップでは、更新済みの `main` から開始する 2 つの PR を作成します。

1. **星評価:** 既存の `starRating` と未評価状態をゲームカードに表示します。
2. **絞り込みと品質ワークフロー:** 絞り込み機能を実装し、指示を更新して機能に適用し、`quality-checks` の報告形式をカスタマイズして、QA プロファイルと関連テストを追加します。

絞り込み機能の計画から PR 作成までは、同じ会話とブランチを使いました。ワークショップを簡潔にするため、その作業を 1 つの PR にまとめました。

## さまざまな検証方法

自動テスト、自分でのブラウザー確認、MCP を介した Copilot のブラウザー確認など、複数の方法でコードを検証しました。`quality-checks` スキルではユニットテスト、lint、型チェックを実行し、新しい形式で報告しました。QA はこれらの結果に要件とテストカバレッジのレビューを加え、PR を作成する前の確認をまとめました。

追加するテストは、実際の不足を補うものである必要があります。QA の実行で追加テストが不要と判断される場合もあります。マージを許可する前にコードと証拠を確認し、変更後は影響を受ける証拠を更新してください。

## ベストプラクティス

Copilot に与えるコンテキストとツールが、作業結果を左右します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを設定しました。これらのカスタマイズは会話をまたいで再利用し、チームのニーズに合わせて調整できます。指示は標準を設定し、スキルは繰り返し実行できるタスクを記述し、カスタムエージェントは専門家の役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合わせて**モードとモデル**を選びます。**Plan** は構築前の方針検討に、**Interactive** はやりとりしながら進める小さな変更に、**Autopilot** は範囲が明確なタスクに使います。定型的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選択します。

ツールやインフラストラクチャと同じくらい、コンテキストも重要です。構築する**もの**、その**理由**、望ましい**方法**を明確に説明すると、結果が大きく変わります。

## さらに試す

主要なワークフローを学びました。Copilot CLI には、他にも試す価値のある機能があります。

- `/review` で、コードレビューエージェントに変更を分析してもらいます。
- `/rubber-duck` で、問題について相談し、別の視点を得ます。
- `/fleet` で、独立したサブタスクを並行して調整します。
- `/worktree` で、別のタスクを独立した環境で進めます。
- `/delegate` で、タスクを Copilot cloud agent に送ります。

## 次のステップ

ツールを使い続けることが、活用方法を身につける一番の方法です。本番コード、趣味のコード、いつか作ろうと思っていた小さなアプリなどで試してみましょう。チームと学びを共有し、お互いの経験から学んでください。ドキュメントも引き続き確認しましょう。

ターミナルから Tailspin Toys を拡張する場合は、任意の[Foundry Backer Concierge シリーズ][foundry]に進んでください。他の環境も確認する場合は、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または[Copilot cloud agent ワークショップ][cloud]を参照してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のカスタマイズ][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests

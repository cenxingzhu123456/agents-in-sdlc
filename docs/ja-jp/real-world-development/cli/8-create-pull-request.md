---
title: "レッスン 8 - 機能の PR を作成してマージする"
description: "絞り込み機能、指示、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

絞り込み機能の実装、指示の更新、スキルの更新、品質保証 (QA) プロファイル、テストが、1 つのブランチに保存されています。すべてをまとめてレビューし、プルリクエストを作成しましょう。星評価のプルリクエスト (PR) は自分でマージしましたが、今回は **Agent Merge** にプロセスを任せます。

> [!NOTE]
> 通常は、機能、指示の更新、スキルの更新、QA エージェントをいくつかの PR に分けます。ワークショップを簡潔にするため、絞り込みと品質のワークフローを 1 つの会話とブランチにまとめ、この PR に含めました。

このレッスンでは、次のことを行います。

- Agent Merge とは何か、マージのライフサイクルをどのように自動化するかを学びます。
- 機能の変更全体と検証の証拠を確認します。
- 絞り込み機能の PR を作成します。
- レビュー後にのみ Agent Merge を有効にして、PR がマージされることを確認します。

## シナリオ

絞り込み機能のワークフローを通じて、Copilot で機能の計画、実装、検証を行いました。Tailspin Toys では、マージを許可する権限は開発者の管理下に置いたまま、残りの PR 作業を自動化したいと考えています。

## Agent Merge とは

**Agent Merge** は、プルリクエストのマージに必要な残りの作業を自動化します。有効にすると、Copilot は失敗した継続的インテグレーション (CI) チェックの修正、レビューコメントへの対応、必要な場合のリベースなど、PR を妨げている問題に対処します。その後、リポジトリで許可されていれば GitHub の自動マージを有効にします。

ここまでは自分で **プルリクエストをマージ**を選択しました。Agent Merge にその役割を任せられますが、有効にする前に作業をレビューし、準備ができていることを確認してください。

## Agent Merge で PR を管理する

コードが揃ったので、まとめてレビューし、PR を作成して、残りの処理を Agent Merge に任せましょう。

1. Codespace に戻ります。
2. `/agent` を入力してエージェントのダイアログを開きます。
3. 一覧から **Default** を選択し、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドで新しい PR を作成します。
5. `/pr agentmerge` を実行して Agent Merge を有効にします。
6. Copilot が PR の継続的インテグレーションの状況を監視します。すべてのチェックが成功すると、マージが実行されます。
7. 「PR #14 was squash-merged successfully.」などのメッセージが Copilot から表示されることを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージキュー、リポジトリの設定、不足している権限を回避しません。処理がブロックされた場合は報告された理由を確認し、リポジトリで許可される場合はレビュー済みの PR を手動でマージしてください。

## まとめと次のステップ

コードの生成、テストと検証、プルリクエストの処理など、開発プロセスの複数の作業を自動化しました。このレッスンでは、次のことを行いました。

- Agent Merge とは何か、マージのライフサイクルをどのように自動化するかを学びました。
- 機能の変更全体と検証の証拠を確認しました。
- 絞り込み機能の PR を作成しました。
- レビュー後にのみ Agent Merge を有効にして、PR がマージされることを確認しました。

次は、コンテキスト、モデル、共有、任意のクラウドへの委任に使える[便利な Copilot CLI スラッシュコマンド][next-lesson]を確認します。

## リソース

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference

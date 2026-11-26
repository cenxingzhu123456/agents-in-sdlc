---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前の数回の演習では、ユーザーエクスペリエンスの改善や機能追加を目的とする 3 つのタスクを Copilot cloud agent に依頼しました。cloud agent は非同期かつ自律的に作業しますが、タスクを監視することも重要です。

GitHub.com の[エージェントページ][agents-page]など、cloud agent に割り当てたタスクを管理する方法はいくつかあります。この mission control では、オープンなプルリクエスト (PR) があるすべてのエージェントタスクを確認できます。実行内容を調べたり、進行中のセッションに指示を追加したりできます。

このレッスンでは、次のことを行います。

- cloud agent のタスクを監視するために、エージェントページを確認します。
- 進行中のセッションに追加機能を依頼します。

## シナリオ

ハイコントラストモードを作成するようエージェントに依頼した後、チームはライトモードも追加する良い機会だと考えました。サイトのスタイルと切り替え機能を更新しているため、ライトモードも含めるのが自然です。エージェントの作業範囲を案内し、ハイコントラストに加えてライトモードも追加させます。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたすべてのタスクの現在の状態を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)を開きます。
2. メインペインと左ペインのタスクリストを確認します。Copilot に割り当てた次のタスクが表示されます。
   - コードベースのドキュメントを更新する。
   - ゲーム変更用の API を生成する。
   - Web サイトにハイコントラストモードを追加する。
3. 実行中のタスクを選択し、Copilot が実行したタスクを確認します。次のような操作が含まれる場合があります。
   - リポジトリからコードをチェックアウトする。
   - Copilot が作業する環境を作成する。
   - MCP サーバーをセットアップする。
   - 割り当てられたタスクを完了するための各種処理を行う。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や進め方によって異なります。

4. 右側に表示されるプルリクエスト (PR) ペインも確認します。PR と変更されたファイルを確認し、進行状況を監視できます。

## cloud agent に追加の指示を送る

実行中のタスクを確認したので、ハイコントラストモードの作業中にライトモードの切り替えも追加するよう Copilot に依頼します。

1. ハイコントラストモードを追加するセッションを選択します。エージェントが付けた名前や作業状況によって、タイトルは異なります。

    ![mission control の Accessibility セッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了して作業が始まるまで、数分間セッションを確認します。作業開始後は、次の手順で追加指示を送れます。
3. **Copilot の作業中にアクティブなセッションを案内**ダイアログに次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Copilot の作業中にアクティブなセッションを案内するダイアログを示すスクリーンショット](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを確認し、作業に取り込む様子を確認します。

## Copilot の作業を待つ

前と同様に、Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で実行中だった手順が終わると、新しい依頼をワークフローに取り込みます。

今回も完了まで数分かかります。ここまでに学んだことを振り返りましょう。

## まとめと次のステップ

Copilot cloud agent のタスクを監視して案内するための中心的な場所である、Copilot agents ページについて学びました。mission control から、進行中または完了したタスクを追跡し、実行された作業を確認して、作業中のタスクの範囲を変更できます。

このレッスンでは、次のことを行いました。

- mission control とエージェントページを使って、cloud agent のタスクを監視しました。
- 進行中のセッションに追加の機能を依頼しました。

Copilot がアクセシビリティ機能を完成させる間に、次は Copilot が作成したプルリクエストを[反復して改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents

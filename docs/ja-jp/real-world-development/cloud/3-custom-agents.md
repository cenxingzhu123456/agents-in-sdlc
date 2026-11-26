---
title: "演習 3 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフロー内の特定のタスクや分野に合わせて、専門化された AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーにある Markdown ファイルでエージェントを定義し、対象の作業を効果的に進めるための指示、ベストプラクティス、コーディングパターン、専門知識を Copilot に提供できます。チームの専門知識を再利用可能なエージェントとしてまとめることができます。たとえば、[WCAG][wcag] に沿ったアクセシビリティエージェント、安全なコーディングを支援するセキュリティエージェント、一貫したテストパターンを守るテストエージェントを作成できます。

カスタムエージェントは、プロジェクト内の `.github/agents` フォルダー、またはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには少なくとも `name` と `description` の YAML フロントマターがあり、その後にエージェントの動作、専門知識、指示を記載する Markdown プロンプトが続きます。

### カスタムエージェントと agent skills の違い

カスタムエージェントと[agent skills][agent-skills-concept]には、役割が一部重複しています。どちらも主に Markdown ファイルで定義し、AI に作業の進め方を伝えます。分かりやすく言うと、**カスタムエージェントは作業者、スキルはツール**です。

カスタムエージェントは独自のコンテキストウィンドウを持ち、スキルや他のエージェントを作業の一部として調整できます。このラボのアクセシビリティカスタムエージェントは、アクセシビリティガイドラインに沿ってサイトをレビュー・更新します。その作業の一部として、プルリクエストワークフローのスキルや、テストを実行・管理するスキルを呼び出せます。

> [!NOTE]
> カスタムエージェントの作成方法は 1 つに限られません。AI を使う場合と同様に、環境やシナリオに合わせて試行し、調整してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディングプラットフォームを利用できるようにしたいと考えています。最近のフィードバックによると、背景色と文字色のコントラストが不足しているため、現在のダークテーマが読みにくいユーザーがいます。このアクセシビリティ上の課題に対応するため、デザインチームはユーザーが切り替えられるハイコントラストモードの実装を依頼しました。

アクセシビリティは重要なため、できるだけ早く実装します。カスタムエージェントに機能を生成させましょう。

この演習では、次のことを学びます。

- カスタムエージェントの定義方法。
- タスクをカスタムエージェントに割り当てる方法。

## アクセシビリティカスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントは、あらかじめ作成されています。Copilot をどのように導くか、内容を確認しましょう。

Codespace に戻り、アクセシビリティカスタムエージェントのファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドがある YAML フロントマターを確認します。

> [!CAUTION]
> カスタムエージェントには、`name` と `description` を含むフロントマターが必要です。

3. 続くセクションを読み、次の内容を確認します。
   - アクセシブルな Web サイトのコードを生成する際の主な責務。
   - アクセシビリティのベストプラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある問題や間違い。

## issue を作成して割り当てる

Mission control は、環境内のエージェントを操作する場所です。Copilot cloud agent にタスクを割り当て、進捗を監視し、追加の指示で作業を案内できます。まず、ハイコントラストモードを追加するタスクを Copilot に割り当てます。

1. リポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次を入力します。

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当てダイアログを開きます。
9. カスタムエージェント一覧から **Accessibility agent** を選択します。

    ![アクセシビリティカスタムエージェントを選択した cloud agent 割り当て画面のスクリーンショット](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドで作業を開始します。

## まとめと次のステップ

GitHub Copilot の[カスタムエージェント][custom-agents]について学びました。カスタムエージェントは、特定のタスクや分野に合わせた専門的な AI アシスタントです。チームの専門知識や標準を再利用可能なエージェントにまとめ、Copilot が作業を効果的に進められるようにします。

この演習では、次のことを学びました。

- カスタムエージェントの定義方法。
- タスクをカスタムエージェントに割り当てる方法。

Copilot がハイコントラストモードを実装している間に、次は mission control から[エージェントセッションを監視して案内します][next-lesson]。

## リソース

- [カスタムエージェントについて][custom-agents]
- [組織でカスタムエージェントを使うための準備][org-custom-agents]
- [Enterprise でカスタムエージェントを使うための準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents

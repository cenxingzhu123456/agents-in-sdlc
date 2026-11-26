---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩んでいない組織は、ほとんどありません。未解決のセキュリティ問題、更新が必要なレガシーコード、実装する時間がなくバックログに残った機能要望などがあります。GitHub Copilot の cloud agent は、コードの更新や機能の追加などのタスクを自律的に実行できます。作業が完了すると、人間の開発者がレビューできるドラフト PR を作成します。これにより、面倒な作業を任せて開発を加速し、開発者はより大きな課題に集中できます。

Copilot cloud agent を使って次のことを学びます。

- コード生成環境をカスタマイズする方法。
- 操作を安全に行う方法。
- 範囲を明確に定義した issue の重要性。
- issue を Copilot に割り当てる方法。

## シナリオ

Tailspin Toys には解消したい技術的負債があります。最初のサイトを構築した外部委託チームがドキュメントを残しておらず、アプリケーションにはほとんど説明がありません。まず、エクスポートされたすべての関数に TSDoc のドキュメントコメントを追加します。

さらに、デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属する他のゲームを表示します。完成度の高い実装はまだ必要なく、UX の受け入れテストに使えるものを求めています。この作業は現時点ではブロッカーですが、優先度の高い他の issue もあります。

どちらも、優先順位が下がりやすいタスクです。Copilot cloud agent に割り当てると、開発者は別の作業を進め、後から Copilot の結果をレビューできます。

## GitHub Copilot cloud agent とは

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同様にバックグラウンドでタスクを実行できます。たとえば、[GitHub issue を Copilot に割り当て][assign-issue]ます。割り当てられると、Copilot は進行状況を追跡するドラフトプルリクエストを作成し、環境をセットアップして、作業を開始します。作業中も完了後もセッションを確認できます。提案された解決策のレビュー準備ができると、プルリクエストで通知されます。

## 範囲を明確にした指示の重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見簡単な操作でも、詳しく見ると複雑な場合があります。[Copilot cloud agent のタスク範囲を明確に定義][cloud-agent-best-practices]してください。AI ペアプログラマーとして扱い、段階的に作業し、学び、試し、適応します。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習ではカスタム指示と、Copilot を導く方法を学びました。すでにカスタム指示の演習を完了していれば `.github/copilot-instructions.md` を実際に確認しています。まだの場合は、cloud agent に作業を割り当てる前に、このリポジトリに含まれる指示ファイルを読み取り専用で確認してください。後で影響を確認できます。

GitHub の Web UI または起動済みの Codespace で、次のファイルを開きます。

- `.github/copilot-instructions.md` - **Code standards** セクションを確認します。特に TypeScript の規約と TSDoc コメントに関する指示を確認します。
- `.github/instructions/unit-tests.instructions.md` - `applyTo` フロントマターを確認します。`**/*.test.ts` ファイルに指示が適用されます。

次のセクションで `Code lacks documentation` issue を cloud agent に割り当てたら、生成されたプルリクエストで TSDoc コメント、TypeScript の規約、ファイルヘッダーを確認します。後の演習で PR をレビューするときに、改めて取り上げます。

## Copilot cloud agent の開発環境をセットアップする

誰がコードを書く場合でも、通常は特定の開発環境とセットアップスクリプトが必要です。タスクを Copilot に割り当てる場合も同様で、Copilot はソフトウェアエンジニアのように作業します。

Cloud agent は作業環境に [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` に設定した[セットアップワークフロー][setup-workflow]を作成すると、作業開始前に必要な開発ツールや依存関係を用意できます。ラボの進行を簡単にするため、事前に設定されています。単一の Astro アプリ用に Node.js、プロジェクトの依存関係、エンドツーエンドテスト用ブラウザー、マイグレーションおよびシード済みの SQLite データベースを利用できるようにします。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # *手順*に必要な最小限の権限を設定します。Copilot の操作には専用のトークンが使われます。
    permissions:
      # 依存関係のインストールなど、セットアップ手順でリポジトリをクローンする場合は `contents: read` 権限が必要です。セットアップ手順でクローンしない場合は、手順の完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体は Astro + Drizzle/libSQL です)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ビルドとテストでデータを使えるよう、ローカルの SQLite データベースをマイグレーションしてシードします。
      - name: Set up the database
        run: npm run db:setup
```

このファイルは一般的な GitHub ワークフローに似ていますが、主な点がいくつかあります。

- `copilot-setup-steps` というジョブが 1 つあります。このジョブは Copilot がプルリクエストの作業を始める前に、GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使うと、リポジトリの **Actions** タブからワークフローを手動実行できます。Copilot が実行するまで待たずに、正常に動くかテストできます。

## ドキュメントを追加する

ドキュメントの重要性は広く理解されていますが、古い情報しかない、あるいはドキュメントがまったくないプロジェクトもあります。このような技術的負債は放置されやすく、生産性を下げ、コードベースの保守や新しい開発者の受け入れを難しくします。Copilot はドキュメントの作成を得意とするため、cloud agent に割り当てるのに適した issue です。バックグラウンドで必要なドキュメントを作成し、後の演習で結果を確認します。

1. 新しいブラウザータブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当てダイアログを開きます。

    ![Copilot を issue に割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のような名前になります。PR が一覧に表示されない場合は、少し待ってからブラウザーを更新します。
12. 数分後、Copilot が To Do リストを作成したことを確認します。

> [!NOTE]
> Copilot の To Do リストが PR に表示されるまで、数分かかる場合があります。前述のワークフローを実行して環境を構築し、プロジェクトを分析して、問題に取り組む方法を決めています。

13. リストを読み、Copilot が実行するタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が issue の作業を開始したことを示す更新を確認します。
15. **View session** ボタンを選択します。

    ![Copilot セッションの表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された表示が見えるよう、ブラウザーを更新する必要がある場合があります。

16. ライブセッションをスクロールして Copilot の作業を確認します。コードを調査して状態を把握し、計画を考え、コードを作成する様子を確認できます。

完了まで数分かかる場合があります。Copilot cloud agent の主な目的は、作業を非同期で実行し、開発者が別のタスクに集中できるようにすることです。この機能を活用するため、もう 1 つのタスクを cloud agent に割り当ててから、アプリへの機能追加に取り組みます。

## ゲーム詳細ページに関連ゲームを追加する

GitHub Copilot cloud agent の利点の 1 つは、作業を分けられることです。デザインチームのために関連ゲームのセクションを追加する作業に時間がかからないとしても、その時間を別の作業に使えます。Copilot cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択します。
4. **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当てダイアログを開きます。

    ![Copilot を issue に割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

しばらくすると、issue の最初のコメントに 👀 が表示され、Copilot が作業を始めたことが分かります。

![issue で作業中を示す目の絵文字を Copilot が使う](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼に取り組み始めました。Copilot cloud agent はソフトウェアエンジニアと同じように作業するため、常に監視する必要はありません。完了後にレビューします。次はカスタムエージェントの作成と使用を確認しましょう。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents]について学びました。Cloud agent に issue を割り当て、非同期でタスクを実行させることができます。技術的負債の解消、新機能の作成、別のフレームワークからのコード移行などを依頼できます。

この演習では、次のことを行いました。

- コード生成環境をカスタマイズしました。
- 操作を安全に行う方法を確認しました。
- 範囲を明確に定義した issue の重要性を学びました。
- issue を Copilot に割り当てました。

Copilot cloud agent がバックグラウンドで作業している間に、次はカスタムエージェントを作成して使います。[Copilot cloud agent では MCP サーバー][cloud-agent-mcp]と、前のモジュールで学んだカスタム指示も利用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローに関するベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions

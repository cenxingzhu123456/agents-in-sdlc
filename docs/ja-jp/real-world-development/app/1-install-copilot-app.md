---
title: "レッスン 1 - GitHub Copilot app のインストール"
description: "GitHub Copilot app をインストールし、テンプレートから作成したリポジトリを接続して、ワークスペースを確認し、クイックチャットを試します。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot app][about-copilot-app]** は、エージェント主導の開発に使うデスクトップアプリケーションです。GitHub Copilot CLI を基盤とし、GitHub とネイティブに統合されているため、リポジトリ、ブランチ、CI パイプラインをすぐに利用できます。自分ですべての作業を行うのではなく、複数のエージェントをそれぞれ独立したワークスペースで並列に指示し、繰り返し作業を自動化するワークフロー向けに設計されています。Node.js のインストールとプロジェクトの準備ができたので、次はアプリをインストールしてリポジトリを接続します。

このレッスンでは、次のことを行います。

- GitHub Copilot app をインストールしてサインインします。
- GitHub リポジトリからプロジェクトをアプリに追加します。
- テンプレートによって用意されたバックログなど、ワークスペースの構成を確認します。
- クイックチャットを使って、アプリについて質問します。

## シナリオ

チームは、増え続けるバックログに対応するため AI エージェントを導入しています。Copilot app では、issue の選択、エージェントの実行、変更のレビュー、プルリクエストのマージを一か所から指示できます。このレッスンでは、アプリをインストールして接続し、プロジェクトについての会話を始める準備をします。

> [!NOTE]
> 対象となる Copilot プランが必要です。Copilot Free、Copilot Student、または有料プラン (Pro、Pro+、Business、Enterprise) を利用してください。Copilot Business または Copilot Enterprise の場合、管理者が **GitHub Copilot app** ポリシーを有効にしておく必要があります。

## GitHub Copilot app をインストールして構成する

GitHub Copilot app を使うには、まずインストールします。Windows、macOS、Linux 向けのバージョンがあります。アプリをインストールして認証し、Tailspin Toys リポジトリを追加しましょう。

1. ブラウザーで [GitHub Copilot app のランディングページ][download-app]を開きます。
2. 使用しているプラットフォーム向けのアプリをダウンロードし、ランディングページに記載された手順に従ってインストールします。
3. インストール後、アプリを開きます。
4. **Sign in to GitHub** を選択し、画面の指示に従って GitHub.com または GHE.com アカウントでサインインします。データ所在地に対応する GitHub Enterprise Cloud を使う場合は、**GitHub Enterprise Cloud** を選択し、求められたら `*.ghe.com` のホスト名を入力します。

   GitHub Enterprise Server を使う場合は、まずクラウドアカウントでオンボーディングを完了します。その後、**Settings** を開き、**Accounts**、**Add account**、**GitHub Enterprise Server** の順に選択します。[Enterprise Server のサインインガイド][ghes-sign-in]に従って、ブラウザーでアプリを認証します。

5. オンボーディングでリポジトリの選択が表示された場合は、テンプレートから作成した Tailspin Toys リポジトリを選択します。選択をスキップして、オンボーディング後にプロジェクトを追加することもできます。
6. テーマを選択してからオンボーディングを完了します。
7. サイドバーの **Sessions** の下にリポジトリが表示されていることを確認します。すでに表示されている場合は、再度追加する必要はありません。
8. リポジトリが未接続の場合は、**Sessions** の横にある **+** を選択して新しいセッションを作成し、**Add GitHub repository** を選択します。前提条件のレッスンで控えた所有者名とリポジトリ名を入力します。
9. サイドバーにリポジトリが表示されることを確認します。ここで使用するレイアウトでは、`tailspin-toys` などのリポジトリフォルダーが **Sessions** の下にグループ化され、それぞれのリポジトリの下に個々のセッションが表示されます。

## ワークスペースを確認する

プロジェクトを接続したら、ワークスペースの各領域を確認します。アプリのサイドバーには次の領域があります。

- **New** - 新しい Copilot チャットセッションを開始します。
- **Pull requests** - アプリの GitHub ネイティブ統合を通じてプルリクエストを表示します。アプリを離れずに、プルリクエストの参照や絞り込み、CI 状態の確認、レビューを行えます。
- **Issues** - issue を表示します。issue の参照や絞り込み、issue からのセッション開始を行えます。
- **Automations** — スケジュールまたはオンデマンドで実行する、保存済みのエージェントタスクです。To Do リストの管理、定期的なプロジェクトの保守、その他の面倒な作業を任せるのに役立ちます。まとめでは次のステップとして紹介しますが、ワークショップ内の演習には含まれません。
- **Customize** - MCP サーバー、プラグイン、スキルなどを使って Copilot app に機能を追加します。Playwright MCP の設定に使用します。
- **Chats** — 独自のブランチやワークスペースを必要としない、質問やブレインストーミング向けの簡易的な会話です。最初のチャットを開始すると表示されます。表示されない場合は、**Sessions** の横にある **+** を選択して最初のチャットを開始してください。
- **Sessions** — リポジトリフォルダーと、その中のエージェントセッションをまとめます。`tailspin-toys` などのリポジトリ名を選択するとリポジトリビューが開き、その下のセッションを選択すると会話に戻ります。セッションごとに独立したワークスペースで実行されるため、変更が競合することなく複数の作業を同時に進められます。星評価を追加する際に最初のセッションを開始します。

ワークショップを進めながら、ワークスペースを確認していきます。

> [!TIP]
> 迷ったときは Copilot に質問しましょう。操作方法が分からない場合や、何かが可能かどうか知りたい場合は、Copilot が案内してくれます。

### 用意されたバックログを確認する

バックログのないプロジェクトはほとんどなく、Tailspin Toys も例外ではありません。テンプレートからリポジトリを作成したときに用意されたバックログを確認しましょう。

1. サイドバーの **Sessions** の下で、個別のセッションではなく Tailspin Toys のリポジトリフォルダーを選択します。次に、リポジトリ上部のナビゲーションで **Issues** を選択します。
2. issue 番号を決めつけず、次のタイトルを探します。

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. issue を選択して詳細を読みます。それぞれの issue からエージェントセッションを開始できます。最初の小さな変更を終えた後、絞り込み issue から作業を開始します。

> [!NOTE]
> **Issues** と **Pull requests** の一覧は自動的に絞り込まれ、Copilot app に追加したリポジトリの項目だけが表示されます。他のリポジトリの作業項目を表示するには、そのリポジトリをアプリに追加してください。

## クイックチャットを試す

アプリに慣れるには、アプリ自体について質問するのが効果的です。**quick chat** は、ブランチや worktree を作成せずに質問やブレインストーミングを行えます。セッションを必要としない、その場限りの簡単な質問に適しています。

1. サイドバーで **Chats** の横にある **+** を選択し、新しいチャットを開きます。**Chats** がまだ表示されていない場合は、**Sessions** の横にある **+** を選択して最初のチャットを開始します。開始後に **Chats** が表示されます。
2. アプリのセッションの仕組みを尋ねます。

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. 会話ビューで回答を読みます。各セッションが独立した git worktree で実行されるため、変更が競合することなく複数のエージェントを並列に実行できます。会話は続けることも、いつでも新しいチャットを開始することもできます。

## まとめと次のステップ

GitHub Copilot app をインストールし、プロジェクトを接続して、ワークスペースを確認しました。次のことを学びました。

- アプリをインストールして GitHub にサインインする。
- GitHub リポジトリからプロジェクトをアプリに追加する。
- ワークスペースを確認し、**Issues** で用意されたバックログを見つける。
- クイックチャットを使って、その場限りの簡単な質問をする。

次は、最初のエージェントセッションを開始し、ゲームカードに星評価を表示する変更を加えます。[レッスン 2「星評価を追加する: 手軽な改善」][next-lesson]に進んでください。

## リソース

- [GitHub Copilot app について][about-copilot-app]
- [GitHub Copilot app の使用を開始する][getting-started]
- [GitHub Copilot app でエージェントセッションを操作する][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
[ghes-sign-in]: https://docs.github.com/enterprise-server@3.22/copilot/copilot-on-ghes/sign-in-on-copilot-app

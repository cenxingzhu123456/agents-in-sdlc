---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: Agent mode][previous-lesson] · [次のレッスン: カスタムエージェント →][next-lesson]

agent mode で絞り込み機能を構築しました。プルリクエストを作成する前に、ブラウザーで実際に動作することを確認します。自分でアプリを操作する代わりに、**Playwright MCP サーバー**を接続し、Copilot に実際のブラウザーを操作させて機能をテストします。その後、ブランチを公開して PR を作成します。

この演習では、次のことを行います。

- Model Context Protocol (MCP) と、MCP サーバーによって Copilot にツールを追加する仕組みを学びます。
- Playwright MCP サーバーをワークスペースに追加します。
- Copilot に Playwright MCP を使って、ブラウザーから絞り込み機能を手動テストさせます。
- ブランチを公開し、絞り込み作業のプルリクエストを作成します。

## Model Context Protocol (MCP) とは

Agent mode は、エディターの外部に接続できるとさらに強力になります。Copilot は Model Context Protocol (MCP) を使います。これは、エージェントが外部のツールやサービスと通信するための標準的な方法です。

![agent mode の内部構造と、コンテキスト、LLM、MCP サーバーや VS Code 拡張機能が提供するツールとのやりとりを示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) は、AI エージェントが外部のツールやサービスと通信するための仕組みです。MCP を利用すると、AI エージェントは外部サービスとリアルタイムに通信できます。リソースを使って最新の情報にアクセスしたり、ツールを使って依頼された操作を実行したりできます。

これらのツールやリソースには、AI エージェントと外部サービスの間を橋渡しする MCP サーバーを介してアクセスします。MCP サーバーは、AI エージェントと外部ツール (既存の API や npm パッケージなどのローカルツール) との通信を管理します。各 MCP サーバーは、エージェントが利用できる異なるツールやリソースを提供します。

よく使われる MCP サーバーには、次のものがあります。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API にアクセスできます。新しいリポジトリの作成、既存リポジトリの更新、issue やプルリクエストの管理などをエージェントが行えます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright を使ったブラウザー自動化機能を提供します。Web ページへの移動、フォームの入力、ボタンの選択などをエージェントが行えます。

他にも、さまざまなツールやリソースにアクセスできる MCP サーバーがあります。GitHub は、エコシステム内の MCP サーバーを見つけやすくし、貢献できるように [MCP レジストリ](https://github.com/mcp)をホストしています。

> [!CAUTION]
> セキュリティの観点から、MCP サーバーはプロジェクト内の他の依存関係と同様に扱ってください。使用前にソースコードをよく確認し、発行元を検証して、セキュリティへの影響を検討します。信頼できる MCP サーバーだけを使い、機密情報や操作へのアクセスを許可するときは注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースで利用する MCP サーバーを設定します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. `github` サーバーがすでに設定されていることを確認します。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

`github` の設定はプロジェクトテンプレートに含まれています。そのため、前の演習で Copilot が issue のバックログを読み取れました。この設定は[リモート GitHub MCP サーバー][remote-github-mcp-server]を使います。ローカルにインストールする必要はありません。VS Code は HTTP 経由で接続し、Copilot が初めてツールを使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

次に、2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]は Copilot に操作可能なブラウザーを提供するため、機能のテストに適しています。

1. `.vscode/mcp.json` で `github` と並べて `playwright` のエントリを追加し、ファイルを次のようにします。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモートサーバーの `github` とは異なり、`playwright` は **ローカル**サーバーです。VS Code が `npx @playwright/mcp@latest` を実行して起動します。`--headless` フラグは Playwright に画面のないブラウザーで実行するよう指示します。デスクトップ画面がない Codespace では、この設定が必要です。

> [!NOTE]
> Tailspin Toys プロジェクトではエンドツーエンドテストに Playwright を使うため、Playwright が必要とするブラウザーはすでにインストールされているはずです。Copilot がブラウザーの不足を報告した場合は、`npx playwright install chromium` を実行してから再試行します。

## サーバーを起動して信頼する

VS Code は必要に応じて MCP サーバーを起動します。Copilot がサーバーのツールを初めて使うとき、VS Code がサーバーを起動し、信頼するかどうかを確認します。

1. `.vscode/mcp.json` で `playwright` のエントリの上に **Start** 操作が表示されます。選択してサーバーを起動し、構成が正しいことを確認します。
2. VS Code からサーバーを信頼するか確認されたら、構成をレビューして信頼することを選択し、サーバーを起動します。

手動で起動する必要はありません。省略した場合は、次の手順で Copilot がツールを必要としたときに VS Code が起動し、信頼するか確認します。

## 絞り込み機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]は、Copilot が操作できる実際のブラウザーを提供します。エージェントはアプリを開き、ページを移動してフィルターを適用し、結果を読み取って要約します。会話を離れずに機能が期待どおりに動作することを確かめる、すばやい方法です。

Playwright MCP サーバーは、スクリーンショットではなくページの[アクセシビリティツリー][playwright-mcp-server]を使って操作します。ボタン、リンク、リスト項目など、ラベル付きの構造化された要素を基にエージェントが処理します。これは支援技術と同じように情報を扱うため、簡単な機能確認でアクセシビリティもある程度確認できます。

サーバーを接続してアプリを実行した状態で、次のプロンプトを Copilot に送信し、作成した絞り込み機能を試してもらいます。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバーを介してブラウザーを起動し、各手順を実行して結果を報告します。報告内容を issue の受け入れ基準と照らし合わせます。問題がある場合は、プルリクエストを作る前に質問を追加するか、コードを修正するよう依頼します。

> [!NOTE]
> このテストには `http://localhost:4321` でアプリが実行されている必要があります。開発サーバーを停止した場合は、プロンプトを送る前に再起動します。Copilot が Playwright MCP サーバーを初めて使うとき、ブラウザーをダウンロードする必要がある場合があります。ブラウザーがないと報告されたら、`npx playwright install chromium` を実行して再試行するよう依頼します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能が動作することを確認できたので、チームがレビューできるようプルリクエスト (PR) を作成します。まず `filtering-vscode` ブランチを公開します。

1. Codespace の **ソース管理**パネルを開き、Copilot が行った変更をレビューします。
2. **+** アイコンを選択して変更をステージングします。
3. **Sparkle** ボタンを使ってコミットメッセージを生成します。

   ![変更内容を表示する Source Control パネルのスクリーンショット](../../../_images/ex3-source-control-changes.png)

4. **公開**を選択して、ブランチをリポジトリにプッシュします。

## プルリクエストを作成する

github.com や GitHub コマンドライン インターフェイス (CLI) など、プルリクエストを作成する方法はいくつかあります。GitHub Copilot を使っているので、PR の作成も依頼しましょう。Copilot は関連 issue を見つけて、それに関連付けた PR を作成できます。

1. Copilot Chat パネルを開き、**新しいチャット**を選択して新しいセッションを開始します。
2. エージェントのドロップダウンで **Agent** が選択されていることを確認します。Copilot が GitHub ツールを使えるようにします。
3. 次のプロンプトで PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 必要に応じて **続行**を選択し、情報の収集や操作の実行を許可します。GitHub ツールを初めて使うときに認証を求められた場合は、表示される手順に従います。
5. Copilot が issue を検索して該当の issue を見つけ、PR を作成したことを確認します。
6. Copilot が作成したリンクを選択して PR をレビューします。ただし、**まだマージしないでください**。

## まとめと次のステップ

おめでとうございます。この演習では、次のことを行いました。

- Model Context Protocol (MCP) と、MCP サーバーによって Copilot にツールを追加する仕組みを学びました。
- Playwright MCP サーバーをワークスペースに追加しました。
- リリース前に、ブラウザーで絞り込み機能を手動でテストしました。
- ブランチを公開し、絞り込み機能のプルリクエストを作成しました。

MCP サーバーで機能を**テスト**しましたが、MCP は Copilot を拡張する方法の 1 つにすぎません。次は、アクセシビリティレビューなど、対象を絞ったタスクを効率化する[カスタムエージェント][next-lesson]を作成します。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration

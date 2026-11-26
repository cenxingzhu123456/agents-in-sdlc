---
title: "演習 2 - Copilot Agent Mode で機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの単純な更新でも、通常は複数のファイルを変更し、テストなどを実行する必要があります。開発者は必要なファイルを探し、変更し、テストを実行し、問題をデバッグし、変更漏れのファイルを見つけて再度更新します。作業は次々に続きます。

ここで Copilot Agent Mode が役立ちます。

Copilot Agent Mode は、IDE 内でより自律的に作業できるように設計されています。開発者と同様に既存のプロジェクト構造を調べ、必要な変更を行い、テストなどのタスクを実行し、見つかった問題を自動的に修正します。Agent Mode を使ってサイトに機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、agent mode と cloud agent は異なる用途向けに設計されています。Agent mode は IDE 内で動作するため、すばやいフィードバックを得ながら対話できます。Cloud agent はチームメンバーのように非同期で作業し、issue とプルリクエストを通してやりとりします。

この演習では、次のことを学びます。

- Copilot Agent Mode がプロジェクトを調べ、関連するファイルを見つけて、複数の変更を調整する方法。
- GitHub Copilot Agent Mode が UI とデータレイヤーにまたがる新機能を実装する方法。
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューする方法。

## シナリオ

ゲームの一覧が増えてきたため、カテゴリと販売元で絞り込めるようにします。前の演習で販売元ヘルパーを追加しました。残りのデータレイヤー、UI、テストの作業を Copilot Agent Mode で完了します。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の Web サイトを確認し、現在の機能を把握しましょう。

この Web サイトは、開発者をテーマにしたボードゲームのクラウドファンディングプラットフォームです。ゲームの一覧と詳細を表示できます。単一の Astro アプリで、ビルド時にページを静的 HTML としてレンダリングします。ページは Drizzle ORM を通じてフロントマターからローカルの SQLite データベースを直接参照します。独立したバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータアクセスヘルパーは `src/lib/` にあり、インタラクティブな動作には標準 DOM API を使った小規模で範囲を限定した Astro `<script>` を使用します。

### Web サイトを起動する

Web サイトを簡単に起動できるよう、単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。次の手順で GitHub Codespace から実行します。

1. Codespace に戻ります。前の演習の後なので `main` ブランチにいるはずです。
2. Codespace 内で <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 絞り込み機能用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーの準備ができると、次のような URL を示すメッセージが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くように求めるダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルで <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら Astro サーバーのアドレス `http://localhost:4321` を選択し、Web サイトを開きます。

> [!NOTE]
> Codespace のターミナルで localhost URL のリンクを選択すると、`https://<your-codespace-name>-4321.app.github.dev/` に自動的に転送されます。これは Web サーバーを実行中の Codespace へのプライベートトンネルです。

### Web サイトを確認する

Web サイトが起動したら機能を確認します。主なページは次のとおりです。

- **ホームページ**: ボードゲームのタイトル、画像、説明を一覧表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、販売元、カテゴリなどの情報を表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**新しいチャット**を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な手順で説明します。

Web サイトの初期実装は動作しますが、さらに機能を追加したいと考えています。まずバックログを確認しましょう。テンプレートからプロジェクトを作成すると、issue のバックログも自動的に用意されます。GitHub Copilot に内容を表示してもらいます。

1. Chat ビューのエージェントのドロップダウンから **Agent** を選択します。Agent は複数のファイルにまたがる変更を自律的に計画・実装し、ターミナルコマンドを実行してツールを呼び出します。

   ![Chat ビューのエージェント選択を示すスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデル一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者が特定のモデルを推奨するものではありません。ワークショップ作成時に Claude Sonnet 4.5 を使用したため、手順にも記載しています。提案の一貫性を高め、スムーズに進めるためですが、LLM は確率的に動作するため、説明と異なる提案が表示される場合があります。これは通常の動作です。

> [!NOTE]
> LLM の確率的な性質により、Copilot が異なる MCP コマンドを使うことがありますが、タスクは完了できるはずです。

3. 次のプロンプトを Copilot に送信して、issue のバックログを表示してもらいます。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **続行**を選択して、すべての issue を一覧表示するコマンドを実行します。
5. 生成された issue の一覧を確認します。

Copilot が、ユーザーに最も役立つと思われる項目を優先順位付けしたことを確認してください。

## 指示ファイルを確認する

Copilot にコードを生成させる前に、作業のコンテキストを与える指示ファイルを確認します。Web サイトに機能を追加する方法を説明する [UI 指示ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)を使います。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加に関する全体的なガイダンスを確認します。次の内容が含まれます。
   - アーキテクチャの概要。
   - コンポーネント設計、テスト容易性、アクセシビリティの原則。
   - Astro、Tailwind CSS、Drizzle などのファイル形式に関する指示へのリンク。

> [!TIP]
> 指示ファイルから、他の指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルからの相対パスです。指示を再利用し、複雑な内容を扱いやすい小さな単位に分け、例やテンプレートを示せます。

## 絞り込み機能を実装する

絞り込みを完成するには、少なくとも 3 つの変更が必要です。

- データレイヤー (`src/lib/`) の絞り込みロジックを追加または改善します。
- 絞り込みの動作を確認するテストを追加または更新します。
- ゲーム一覧ページに絞り込み UI を追加します。

コードベースにマージする前にテストを実行して成功させる必要もあります。Copilot Agent Mode にこれらの作業を依頼しましょう。

1. Copilot との現在の会話を続けるか、**新しいチャット**を選択して新しい会話を始めます。
2. **コンテキストの追加**、**指示**、**ui** の順に選択して指示ファイルを指定します。

   ![UI 指示ファイルを選択する例を示すスクリーンショット](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェントのドロップダウンで、引き続き **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント選択を示すスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

4. モデルが引き続き **Claude Sonnet 4.5** であることを確認します。
5. バックログの関連 issue に基づいて機能を実装するよう、次のプロンプトを送信します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、関連するファイルを探し、データレイヤーのヘルパー、UI、テストを見つけて変更し、テストを実行する様子を確認します。

   ![Copilot がプロジェクトファイルを調べる様子を示すスクリーンショット](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトを調べ、ファイルを変更し、テストを実行します。タスクやコードベースの複雑さによっては数分かかることがあります。作業中、コードエディターに **保持**と**元に戻す**が表示される場合があります。Copilot が完了すると、すべての変更に対して操作を選べるため、作業中に選択する必要はありません。

7. Copilot に求められたら、**続行**を選択してテストを実行します。

   ![Copilot Chat でテスト実行の確認を求めるダイアログのスクリーンショット](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストに失敗したりする場合があります。問題ありません。Copilot はタスクを完了し、エラーがないと判断するまで、コード生成とテストを繰り返します。

   ![Copilot Agent Mode との会話が完了した状態を示すスクリーンショット](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを開いたブラウザーに戻り、新機能を試します。
11. 動作とコードを確認したら、Copilot Chat ウィンドウで **保持**を選択します。

## まとめと次のステップ

おめでとうございます。この演習では、GitHub Copilot Agent Mode を使って Tailspin Toys の Web サイトに機能を追加しました。次のことを学びました。

- GitHub Copilot Agent Mode が UI とデータレイヤーにまたがる機能を実装する方法。
- Copilot Agent Mode がプロジェクトを調べ、関連ファイルを見つけて、複数の変更を調整する方法。
- コードベースにマージする前に、Agent Mode が生成した変更とテストをレビューする方法。

次は、[Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを作成します。

### 追加の演習: ページネーションを実装する

ゲームの一覧が増えた場合、ページネーションが必要です。この演習で学んだことを使い、Copilot にページネーションの実装を依頼します。次の点を検討してください。

- 既存の指示ファイルを使うなど、現在のベストプラクティスに従います。
- ページサイズをユーザーが選べるようにするか、固定値にするかを決めます。
- 希望する実装に必要なガイダンスを、プロンプトに含めます。
- Copilot に変更を依頼し、コンテキストを追加しながら反復する必要がある場合があります。これは Copilot を使う通常のワークフローです。

## リソース

- [Copilot の ask、edit、agent mode: それぞれの役割と使いどころ][choose-mode]
- [VS Code の agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode

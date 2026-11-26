---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Agent mode →][next-lesson]

生成 AI を利用するうえで、コンテキストは重要です。タスクを特定の方法で進める必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにします。特定のコンテキストを共有する方法はいくつかあります。特に重要なのが、コード生成の指針を伝える[指示ファイル][instruction-files]です。

この演習では、次のことを行います。

- リポジトリのカスタム指示やパス別の指示ファイルを使い、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準を Copilot に伝える方法を確認します。
- 現在の指示を使って、絞り込み機能の最初のデータスライス (販売元ヘルパー) を生成します。
- `.github/copilot-instructions.md` に、リポジトリ全体に適用する標準を追加します。
- 同じプロンプトをもう一度送り、生成されたコードが新しい標準に従うかを確認します。
- 次の演習で Copilot が更新したガイダンスを使えるよう、指示の更新と絞り込み機能の一部を `main` にコミットします。

> [!CAUTION]
> 生成コードが設定した標準から外れる場合があります。Copilot の動作は確率的です。この演習では、指示の更新後に動作がどのように変化するかを見ることが目的であり、生成結果を文字単位で一致させることではありません。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発手法に関するガイドラインと要件があります。具体的には、次のとおりです。

- データレイヤーには、必ずユニットテストを用意します。
- UI はダークモードで、現代的な印象にします。
- TSDoc のドキュメントコメントをコードに追加します。
- 各ファイルの先頭に、ファイルの役割を説明するコメントブロックを追加します。

指示ファイルを使い、Copilot がこれらの開発慣行に沿ってタスクを実行できるようにします。

### カスタム指示

カスタム指示を使うと、コーディングスタイルや要件を Copilot に伝え、より適切な提案やコードスニペットを得られます。優先する規約やライブラリ、コメントの書き方などを指定できます。リポジトリ全体に適用する指示も、特定の種類のファイルに適用する指示も作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべての**リクエストで Copilot に送られます。Copilot に送るチャットや CLI の多くに関連する、プロジェクト全体の情報を記載します。技術スタック、開発中のものの概要、ベストプラクティス、全般的なガイダンスなどが該当します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル形式に対する指示です。TypeScript や Astro などの言語、UI コンポーネントの作成やユニットテストの追加といったタスクのガイドラインを記載できます。

> [!NOTE]
> IDE で指示ファイルがコード生成に使われるのは Copilot Chat の場合のみです。コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリレベルのファイルと `applyTo` フロントマターがある `*.instructions.md` ファイルの両方を使ってコードを生成します。
>
> Copilot は AGENTS.md や CLAUDE.md など、他の標準形式の指示ファイルにも対応しています。詳細は[カスタム指示のサポート][custom-instructions-support]を参照してください。

### 指示ファイルを管理するためのベストプラクティス

指示ファイルの作成方法を詳しく扱うことは、このワークショップの範囲外です。サンプルプロジェクトの例は、一般的な方法を示しています。基本方針は次のとおりです。

- `copilot-instructions.md` には、構築中のもの、プロジェクトの構造、全体的なコーディング標準など、プロジェクトレベルのガイダンスを記載します。
- `*.instructions.md` には、ユニットテスト、Astro コンポーネント、データレイヤーなどのファイル形式や、特定のタスクに対する指示を記載します。
- 自然な言葉で、明確なガイダンスを記述します。コードの良い例と悪い例を示します。

指示ファイルの作成方法は 1 つに限られず、AI の使い方も同様です。プロジェクトやシナリオに合わせて試行し、効果的な方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトでは、十分な指示ファイルを用意することをお勧めします。このプロジェクトには、[UI 更新][ui-instructions]や [Astro][astro-instructions] など、多くのタスクを対象としたファイルがあります。
>
> Copilot に指示ファイルを作成してもらうこともできます。操作方法は環境によって異なります (例: VS Code の **チャットの構成 → エージェントの指示を生成**、Copilot CLI の `/init`)。このワークショップでは、必要な箇所で各環境の操作を説明します。
>
> テンプレートや出発点を探している場合は、指示ファイル、カスタムエージェント、その他のリソースをまとめた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに含まれる指示ファイルを読みます。基本となる `copilot-instructions.md` と、さまざまなタスクを対象とする `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内にある **GitHub Actions Workflows** の指針も確認してください。これらの指示は Copilot とのあらゆるやりとりに適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データレイヤー、テストなどの指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、適用対象をリポジトリのルートからの glob で指定します。ここでは、たとえば `**/*.test.ts` に一致する TypeScript のテストファイルが対象です。
5. このプロジェクトでユニットテストを作成するための指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などの他の指示ファイルや、プロジェクト内の既存ファイルへのリンクを確認します。大きな指示を再利用可能な小さなファイルに分け、Copilot が参考にできる例を示せます (ここにあるパスはリポジトリのルートではなく、指示ファイルからの相対パスです)。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメント化はまだ必須ではありません。次の手順で TSDoc コメントとファイルヘッダーの規則を追加します。

## 変更用のブランチを作成する

ベストプラクティスに沿って、変更用のブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する**前に** Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**新しいチャット**を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な手順で説明します。

カスタム指示の効果を確認するため、まずは現在の指示ファイルを使ってプロンプトを送信します。その後、指示を更新して同じプロンプトをもう一度送り、違いを確認します。

1. Copilot が必要なコンテキストだけを使うよう、前の演習で開いたエディターのタブを閉じます。
2. ヘルパーの配置場所を Copilot に示すため、`src/lib/publishers.ts` を開きます。
3. Chat ビューのエージェントのドロップダウンから **Agent** を選択し、ファイルの変更を適用できるようにします。

   ![Chat ビューのエージェント選択を示すスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot はプロジェクトを調べてコードを変更します。ヘルパーファイルとテストなど、複数のファイルに変更が及ぶ場合があります。
6. 提案されたヘルパーが、`db` クライアントを最初の引数に取り、型付きの販売元配列を返す関数であることを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータレイヤー規約に基づいています。
7. 提案されたコードに TSDoc ドキュメントコメントとファイルレベルのコメントヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot の動作は確率的なため、指示がなくてもドキュメントコメントを追加する可能性があります。その場合も問題ありません。指示を更新した後に動作が一貫することが、この演習の目的です。

## リポジトリ全体に適用する標準を追加する

前述のとおり、`.github/copilot-instructions.md` は Copilot にプロジェクト全体の情報を提供します。コードの提案を改善するため、リポジトリのコーディング標準を記載しましょう。

1. `.github/copilot-instructions.md` を開き直します。
2. **Code formatting requirements** セクションを探します (27 行目付近)。コーディング標準は記載されていますが、コード内のドキュメントに関するルールがありません。
3. 既存の標準のすぐ下に次の Markdown を追加し、ファイルコメントヘッダーと TSDoc コメントを追加するよう Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで学んだように、リポジトリレベルの `.github/copilot-instructions.md` は全体的な指針に、`*.instructions.md` は特定の言語、ファイル形式、タスクに対する指示に使えます。今回追加したドキュメントコメントのルールのように、プロジェクト全体の標準はリポジトリレベルのファイルに記載します。

## プロンプトを再送信して変化を確認する

1. Copilot Chat に戻り、**新しいチャット**を選択してコンテキストをクリアします。
2. `src/lib/publishers.ts` を再度選択し、Copilot の対象ファイルを明確にします。
3. 先ほどと**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. ここで提案されるファイルの先頭に、次のようなコメントブロックがあることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが含まれることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

新しいプロジェクト標準に従うよう Copilot に指示し、次の演習で拡張する実際のコードに適用できました。

## 指示ファイルをコミットしてブランチをプッシュする

指示ファイルもリポジトリ内の他のファイルと同様に、ソース管理で管理します。変更をコミットしてブランチをプッシュしましょう。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押し、新しいターミナルを開きます。
2. ターミナルで次のコマンドを実行し、指示ファイルとヘルパーの変更を確認します。

   ```bash
   git status
   ```

3. 指示の更新と、ヘルパーの基礎部分に Copilot が加えたすべての変更をステージングしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status にテストなどの追加変更が表示された場合は、それらのファイルも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、バックログにあるドキュメント標準の issue に関連付けてプルリクエストを作成します。手動で行う代わりに、GitHub ツールに接続済みの Copilot に依頼できます。該当 issue を探し、PR を作成してマージするよう依頼します。

> [!NOTE]
> この演習では、Copilot が初めて GitHub 上で操作します。プロジェクトのテンプレートでは GitHub ツールがすでにワークスペースに接続されています。Copilot が初めてツールを使うときに、VS Code から GitHub へのサインインを求められる場合があります。表示される手順に従ってください。後の演習では、この接続の仕組みである Model Context Protocol について学びます。

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェントの選択で **Agent** が選ばれていることを確認します。
3. 指示ファイルの更新に関する issue を探し、現在のブランチから PR を作成して、指示の変更と販売元ヘルパーの基礎部分を両方説明するよう Copilot に依頼します。

   ```text
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が issue の検索と PR の作成を始めます。
5. Copilot による GitHub 操作を **許可**するよう求められたら、コマンドを確認して必要に応じて **許可**を選択します。
6. PR が作成されたら、次のプロンプトでマージを依頼し、ローカルのブランチを `main` に戻します。

   ```text
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot による GitHub 操作とシェルコマンドの実行を **許可**するよう求められたら、コマンドを確認して必要に応じて **許可**を選択します。

GitHub Copilot の支援でプルリクエストを作成し、マージできました。

## まとめと次のステップ

Copilot がこのプロジェクトの指示ファイルからコンテキストを得る方法を確認し、Copilot Chat を使って次のことを行いました。

- 既存の指示でコード生成プロンプトを送り、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加しました。
- 同じプロンプトを再送信し、提案されたコードが新しい標準に従うことを確認しました。
- 次の演習で作業を引き継げるよう、指示とヘルパーの基礎部分を `main` にコミットしました。

次は [agent mode][next-lesson] を使い、コードベース全体にわたる新機能の追加に指示を適用します。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [効果的なカスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/

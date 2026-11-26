---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を利用するうえで、コンテキストは重要です。タスクを特定の方法で進める必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにします。[指示ファイル][instruction-files]を使うと、Copilot に求めることだけでなく、その進め方も伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示やパス別の指示ファイルを使い、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準を Copilot に伝える方法を確認します。
- `.github/copilot-instructions.md` に、リポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code および CLI のハーネスとは異なり、この演習では変更前後のプロンプトを比較しません。Copilot cloud agent は GitHub issue を非同期で処理するため、リアルタイムに並べて効果を示すのは困難です。後の演習で cloud agent が作成したプルリクエストをレビューするときに、指示ファイルの影響を確認します。

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
6. `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などの他の指示ファイルや、プロジェクト内の既存ファイルへのリンクを確認します。大きな指示を再利用可能な小さなファイルに分け、Copilot がコード生成時に参考にできる例を示せます (ここにあるパスはリポジトリのルートではなく、指示ファイルからの相対パスです)。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメント化はまだ必須ではありません。次の手順で TSDoc コメントとファイルヘッダーの規則を追加します。

## リポジトリ全体に適用する標準を追加する

ここで行う編集は 1 つだけです。コード内のドキュメントを TSDoc コメントとファイルレベルのコメントヘッダーで記述する、プロジェクト全体のルールを追加します。後の演習で issue を割り当てると、cloud agent がこのルールを使います。

編集の前に、新しいブランチを作成します。cloud agent は issue の対象ブランチにある指示を読み込みますが、この演習では編集内容をきちんとコミットします。

1. Codespace のターミナルから新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

前述のとおり、`.github/copilot-instructions.md` は Copilot にプロジェクト全体の情報を提供します。コードの提案を改善するため、リポジトリのコーディング標準を記載しましょう。

1. `.github/copilot-instructions.md` を開き直します。
2. **Code formatting requirements** セクションを探します (27 行目付近)。プロジェクトのコーディング標準が記載されていますが、コード内のドキュメントに関するルールがないため、生成されたヘルパーにドキュメントコメントがありません。
3. 既存の標準のすぐ下に次の Markdown を追加し、ファイルコメントヘッダーと TSDoc コメントを追加するよう Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで学んだように、リポジトリレベルの `.github/copilot-instructions.md` は全体的な指針に、`*.instructions.md` は特定の言語、ファイル形式、タスクに対する指示に使えます。今回追加したドキュメントコメントのルールのように、プロジェクト全体の標準はリポジトリレベルのファイルに記載します。

## 指示の変更をコミット、プッシュ、マージする

Cloud agent は、issue の対象ブランチから指示ファイルを読み込みます。次の演習で issue を割り当てる際、Copilot は `main` からブランチを作成します。cloud agent が指示を使えるよう、変更を `main` にマージする必要があります。

1. ステージングしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを作成し、マージします。次の演習で issue に割り当てたとき、cloud agent がこの指示を読み込みます。

> [!TIP]
> このワークショップで `main` に直接作業する場合は、ブランチを省略して `main` に直接コミットできます。ここでは実際のプロジェクトでの作業に近づけるため、ブランチを使っています。

## まとめと次のステップ

Copilot がプロジェクトの指示ファイルからコンテキストを得る方法を確認し、`.github/copilot-instructions.md` に新しい標準を追加しました。このハーネスの後半で cloud agent が作成するプルリクエストをレビューするときに、この標準が使われていることを確認できます。

次は、Copilot cloud agent に[最初の issue を割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [効果的なカスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/

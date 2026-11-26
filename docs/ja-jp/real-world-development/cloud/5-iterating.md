---
title: "演習 5: GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザーエクスペリエンスの改善や機能追加を目的とする複数のタスクを GitHub Copilot に依頼してきました。コードにドキュメントを追加し、デザインチームが検討する関連ゲーム機能を構築し、ハイコントラストとライトモードの切り替えなどのアクセシビリティ機能を実装するよう依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを送って改善しましょう。

### シナリオ

何度も説明してきたように、生成 AI を導入してもソフトウェア設計と DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って作業します。チームメンバーがレビューを始める前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能について GitHub Copilot が提案した変更を確認します。

## GitHub Copilot cloud agent のセキュリティ

Copilot cloud agent は非同期に、監督者なしでタスクを実行するため、安全性を維持するためのセキュリティ上の制約があります。

- Copilot はリポジトリへの読み取りアクセスのみを持ち、書き込みできるのは作業に使うブランチ**のみ**です。
- Cloud agent は GitHub Actions 内で実行され、作業用の独立した一時環境を作成します。
- GitHub Actions ワークフローを実行するには、人間による承認が必要です。
- [既定では外部リソースへのアクセスを制限][agent-firewall]しています。MCP サーバーも対象です。

## 生成されたドキュメントをレビューする

まず、コードにドキュメントを追加する GitHub Copilot cloud agent の最初のプルリクエスト (PR) を確認します。GitHub.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR を確認すると、GitHub Copilot がファイアウォールでブロックされたという警告が表示される場合があります。Copilot は、外部リソース (外部 MCP サーバーへの呼び出しなど) へのアクセスが既定で制限されているため、これは**想定どおり**です。必要であれば、Copilot cloud agent の[ファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して PR の一覧を開きます。
3. **Add missing documentation** など、同様のタイトルが付いたプルリクエストを開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルに **[WIP]** が付いています。Copilot が作業を終えるまで待ちます。数分かかることがあるため、休憩したり、これまでに学んだことを振り返ったりしてください。

4. PR の準備ができたら **Files changed** タブを選択し、変更を確認します。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. TSDoc のドキュメントコメントやその他のドキュメントを含む、更新されたコードを確認します。変更内容は状況によって異なります。

    コードを確認するときは、TSDoc コメント、TypeScript の規約、コメントヘッダーに注目してください。演習 2 の最初に確認したカスタム指示ファイルが反映されています。

6. 更新内容を確認して問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示す表示を確認します。
8. ワークフローの承認待ちがある場合は、**Approve and run workflows** を選択します。

   ![ワークフローの承認と実行](../../../_images/shared-approve-workflows.png)

9. PR のチェック欄にワークフローが追加されます。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

プルリクエストで Copilot と共同作業できます。チームメンバーと同様に、PR やコード行のコメントで Copilot にメンションできます。Copilot はコメントを確認し、対応するセッションを開始します。結果は確率的に変わるため、固定のプロンプトはありません。次のような変更を依頼できます。

- 各コードファイルの先頭に、内容を簡単に説明するコメントヘッダーを追加する。
- TypeScript と Astro ファイルに TSDoc コメントを追加する。
- Astro アプリの構造を説明する README を作成する。

1. ドキュメントの変更を依頼するコメントを追加し、他のユーザーと同じように **@copilot** をメンションします。上記の例から選ぶか、コードベースに必要なドキュメントについて別の変更を依頼します。
2. **View Session** を選択して Copilot の作業を確認します。変更を行う新しいセッションが始まります。
3. **Back to pull request** を選択すると PR に戻れます。

    ![プルリクエストに戻る](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が終わると、プルリクエストに新しいコミットが表示されます。
5. **Files changed** タブを選択して変更をレビューします。

満足するまで反復して調整できます。問題がなければ、ドラフト PR をレビュー可能な状態に変更して `main` ブランチにマージします。

![PR をレビュー可能な状態にする](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する issue を解決するため、Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** など、同様のタイトルが付いた PR を選択します。
4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 更新内容を確認して問題がなければ **Conversation** タブに戻り、下にスクロールします。
6. ワークフローの承認待ちを示す表示を確認します。
7. ワークフローの承認待ちがある場合は、**Approve and run workflows** を選択します。

   ![ワークフローの承認と実行](../../../_images/shared-approve-workflows.png)

8. PR のチェック欄にワークフローが追加されます。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。
9. **任意:** Codespace でこのブランチに切り替え、関連ゲーム機能を手動でテストできます。Codespace に移動してターミナルを開き、Copilot が作成したブランチ名 (例: **copilot/fix-8**) に置き換えて次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を実装しました。前と同様に、Copilot cloud agent に追加の変更を依頼して反復できます。たとえば、表示する関連ゲームの数を調整したり、コメントヘッダーと TSDoc コメントを追加したりできます (この issue は、カスタム指示を更新する前に割り当てたことを思い出してください)。**Conversation** タブにコメントを追加すると、Copilot が内容を確認して新しいセッションを始めます。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティカスタムエージェントで実装した機能を確認します。この PR には、演習 3 で依頼したハイコントラストモードと、演習 4 で mission control から追加を依頼したライトモードの両方が含まれます。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** など、同様のタイトルが付いた PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルに **[WIP]** が付いています。Copilot が作業を終えるまで待ちます。数分かかる場合があります。

4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 実装を確認し、特に次の点に注意します。
   - モードを切り替える UI コンポーネント。
   - ユーザー設定を保存するローカルストレージの利用。
   - ハイコントラストとライトモードの CSS またはスタイルの変更。
   - アクセシビリティ属性 (ARIA ラベル、キーボード操作など)。
   - モードの切り替えを管理する JavaScript/TypeScript コード。

6. 更新内容を確認して問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示す表示を確認します。
8. ワークフローの承認待ちがある場合は、**Approve and run workflows** を選択します。

   ![ワークフローの承認と実行](../../../_images/shared-approve-workflows.png)

9. PR のチェック欄にワークフローが追加されます。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。
10. **任意:** Codespace でこのブランチに切り替え、アクセシビリティ機能を手動でテストできます。Codespace に移動してターミナルを開き、Copilot が作成したブランチ名に置き換えて次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    その後、アプリケーションを起動し、ブラウザーでハイコントラストとライトモードを切り替えます。ページを再読み込みしても設定が保存されることを確認します。

アクセシビリティカスタムエージェントが、アクセシビリティのベストプラクティスに沿って Copilot の作業を案内したことを確認します。改善点があれば、前の PR と同様にコメントで **@copilot** をメンションして変更を依頼できます。

## 任意の演習 — 作業の委任を続ける

Cloud agent は、実際のバックログ項目を渡して別の作業に集中できると効果的です。リポジトリに issue を追加して Copilot に割り当て、この習慣を身につけましょう。次のようなタスクを試せます。

- ゲーム詳細ページに支援者向けの関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセスヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent ハーネスを完了しました。レッスンを通じて、次のことを行いました。

- **このリポジトリに含まれるカスタム指示ファイルを確認し**、後で cloud agent の出力にどのように影響するかを確かめました。
- **issue を Copilot cloud agent に割り当て**、環境のセットアップ、計画、非同期の実行を確認しました。
- **アクセシビリティ用のカスタムエージェントを作成して使い**、ハイコントラストとライトモードの切り替えを追加しました。
- **Copilot agents ページを mission control として使い**、進行中のアクセシビリティセッションを監視して案内しました。
- **cloud agent のプルリクエストをレビューして反復し**、`@copilot` に変更を依頼してワークフローを承認しました。

## 振り返りと次のステップ

Cloud agent ハーネスを完了しました。他の環境で学ぶ場合は、次のハーネスを参照してください。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE から Copilot Chat agent mode と MCP 統合を試します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan と Autopilot モード、agent skills、カスタム QA エージェント、Playwright MCP、Agent Merge、任意の `/delegate` を使い、ターミナルからレビュー済みの変更を 2 つリリースします。

自分のリポジトリでは、次のアイデアを試してください。

- リファクタリングやテストカバレッジの issue を cloud agent に割り当てます。
- 別の分野を担当するカスタムエージェントを作成します。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定します。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agents について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローに関するベストプラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/

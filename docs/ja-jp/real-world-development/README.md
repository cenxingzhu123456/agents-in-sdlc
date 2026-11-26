---
title: "実践的な開発"
slug: ja-jp/real-world-development
authors:
  - geektrainer
lastUpdated: 2026-09-16
---

現実的なシナリオ、完成したアプリケーション、共有バックログを使って、ソフトウェア開発ライフサイクル全体で GitHub Copilot を活用します。変更の計画、コードの作成、テスト、プルリクエストのレビュー、エージェントを使った作業管理を繰り返し実践します。

## シナリオ

あなたは、ボードゲームのクラウドファンディングを提供する架空の会社 Tailspin Toys に入社したばかりの開発者です。チームのバックログは GitHub issue として登録済みで、すぐに着手できます。カテゴリやページネーションによる絞り込みなどの機能開発に加えて、アクセシビリティやコーディング規約などの品質改善も含まれています。

## 作業環境を選択する

GitHub Copilot は、どの環境でも利用できます。開発に使う環境を選んでください。各ワークショップでは独自のセットアップを行い、共通の Tailspin Toys シナリオを使用します。

### [GitHub Copilot CLI][cli]

ターミナルでエージェント型アシスタントを使います。Plan モードと Autopilot モード、指示ファイル、スキル、カスタムエージェント、Playwright MCP、Agent Merge、便利なスラッシュコマンドを活用します。

### [GitHub Copilot app][app]

デスクトップアプリでエージェントの複数セッションを並行して実行し、セッションモードを切り替え、Canvas で共同作業し、GitHub issue とプルリクエストを管理します。

### [GitHub Copilot cloud agent][cloud]

GitHub issue を非同期エージェントに割り当て、作業を案内し、進行状況を監視して、作成されたプルリクエストをレビューします。

### [Visual Studio Code][vscode]

Visual Studio Code と GitHub Codespaces で GitHub Copilot を使います。エディターを離れずに Copilot Chat の agent mode、MCP サーバー、カスタムエージェントを利用します。

[cli]: cli/
[app]: app/
[cloud]: cloud/
[vscode]: vscode/

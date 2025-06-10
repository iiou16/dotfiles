# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Referenced Documents

プロジェクトの開発タイプに応じて以下のガイドラインを参照してください：

- Python開発: @python_dev.md
- TypeScript開発: @typescript_dev.md  
- Webアプリケーション開発（FastAPI + React）: @web_app_dev.md

## Base Guidelines

- 必ず日本語で回答してください
- 不明点があったりファイル/ディレクトリが見つからないときは処理を止めてユーザーに質問して下さい
- ファイルを編集するときは、修正内容を簡潔に述べてから修正を開始してください
- プログラムを実行した結果エラーが出た場合には、エラーを修正することを優先してください。もしアプローチを変更する必要がある場合は、必ず一度確認を取ってください

## Development Philosophy

### Test-Driven Development (TDD)

- 原則としてテスト駆動開発（TDD）で進める
- 期待される入出力に基づき、まずテストを作成する
- 実装コードは書かず、テストのみを用意する
- テストを実行し、失敗を確認する
- テストが正しいことを確認できた段階でコミットする
- その後、テストをパスさせる実装を進める
- 実装中はテストを変更せず、コードを修正し続ける
- すべてのテストが通過するまで繰り返す

## Commit Message Guidelines

コミットメッセージは、変更内容を明確に伝えるための重要な要素です。
以下のガイドラインに従って、適切なコミットメッセージを作成してください。

### 1. Standard Commit Message Format

```
<type>: <subject>

<body>

```

### 2. Description of Each Element

#### Type

- feature: 新機能
- fix: バグ修正
- docs: ドキュメントのみの変更
- style: コードの意味に影響を与えない変更（空白、フォーマット、セミコロンの追加など）
- refactor: バグ修正や機能追加のないコードの変更
- test: テストの追加・修正
- chore: ビルドプロセスやドキュメント生成などの補助ツールやライブラリの変更

#### Subject

- 変更内容を簡潔に要約

#### Body

- 変更の詳細な説明
- 改行して複数行で記述可能
- なぜその変更が必要だったのかの背景も含める
- 72文字で改行

### 3. Commit Message Examples

```
feature: ドキュメントレビュー承認機能を追加

- レビュー承認ワークフローを実装
- 承認条件のバリデーションを追加
- 承認履歴の追跡機能を実装

```

### 4. Important Notes

- 1つのコミットでは1つの論理的な変更のみを含める
- 複数の変更がある場合は複数のコミットに分割する
- コミットメッセージは日本語で記述可能

# setup-docs.md

## 目的

このプロジェクトの不変情報を `docs/` に整理し、以降の `Codex` 作業で参照しやすくする。

## 出力対象

- `docs/functional-design.md`
- `docs/architecture.md`
- `docs/repository-structure.md`
- `docs/development-guidelines.md`
- `docs/glossary.md`

## Codex への渡し方

```text
AGENTS.md と project-brief.md と .codex/commands/commands_setup_tsuwano_tourisum/setup-docs.md を読んで、docs/ を順に整備して。
```

## 実行フロー

1. `project-brief.md`、`README.md`、既存コード、既存 docs を読む
2. ドキュメントの責務を分ける
   - functional-design: 何を伝えるサイトか
   - architecture: 技術構成と責務分離
   - repository-structure: ディレクトリの意味
   - development-guidelines: 実装 / レビュー / テスト規約
   - glossary: 用語集
3. 1 ファイルずつ下書きを作る
4. 各ファイルごとに、要点を短く要約して確認する
5. 問題なければ次のファイルへ進む
6. 完了後、`.steering/_setup-progress.md` を更新する

## ルール

- brief と矛盾する説明を書かない
- `Codex` 固有の操作手順は `development-guidelines` や `agent-shared` へ分離し、機能設計には混ぜない
- 実装がまだ無い部分は、未決定と明示する
- 将来変わりやすい情報と不変情報を混ぜすぎない

## 完了条件

- 5 ファイルが存在する
- 用途の重複が少ない
- brief と README の説明と矛盾しない
- `verify-setup.md` で確認できる状態になっている

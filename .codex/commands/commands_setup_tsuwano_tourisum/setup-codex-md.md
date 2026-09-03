# setup-codex-md.md

## 目的

`AGENTS.md` を中心に、`Codex` 向けの共有参照文書と `.steering/_template/` を整える。

## 出力対象

- `AGENTS.md`
- `docs/agent-shared.md`
- `.steering/_template/requirement.md`
- `.steering/_template/design.md`
- `.steering/_template/tasklist.md`
- `.steering/_template/blockers.md`
- `.steering/_template/decisions.md`

## 実行フロー

1. `docs/` 配下と `project-brief.md` を読む
2. `AGENTS.md` は短く保つ
   - repo の目的
   - 最優先ルール
   - 作業時の基本方針
   - よく参照するファイル
3. `docs/agent-shared.md` を作る
   - プロジェクト概要
   - ディレクトリ概要
   - 実装 / テスト / レビューの基本
   - `docs/external-tools.md` へのリンク
4. `.steering/_template/` の各ファイルに最小限テンプレートを置く
5. 進捗ファイルを更新する

## ルール

- `AGENTS.md` に長文の設計詳細を詰め込みすぎない
- 変化しやすい詳細は `docs/` に逃がす
- `AGENTS.md` と `docs/agent-shared.md` の責務を分ける
- `Codex` 固有の運用説明は `AGENTS.md`、プロジェクト固有の不変情報は `docs/agent-shared.md`

## 完了条件

- `AGENTS.md` が短く分かりやすい
- `docs/agent-shared.md` がリンクハブとして機能する
- `.steering/_template/` の雛形が揃っている

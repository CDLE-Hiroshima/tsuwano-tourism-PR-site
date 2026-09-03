# AGENTS.md

このリポジトリで Codex が作業を始めるときは、最初に `project-brief.md` を読むこと。
このブリーフが、津和野PRサイトの SSOT です。

## 最優先ルール

- `project-brief.md` の空欄 `【 】` は推測で埋めない。空欄があれば 1 問ずつユーザーに確認する。
- ⑩「避けたい表現・扱い」は絶対条件。殉教の歴史を軽く扱わない。
- ⑧「声のトーン」と ⑥「来てほしいたった一人」を、文章と画面設計の判断基準にする。

## 変更時の基本方針

- `README.md` を触るときは `<!-- brief:* -->` のマーカーを残す。
- `web/` を作るときは、まず MVP の 1 ページを成立させる。いきなり過剰に広げない。
- 大きな変更の前に、brief のどの項目をどう使うかを短く整理してから着手する。
- 実装後は可能な範囲で `npm run build` などの検証を行い、結果を報告する。

## 補助ファイル

- `.codex/prompts/bootstrap.md`: ブリーフ補完と README 更新のテンプレート
- `.codex/prompts/build-site-mvp.md`: MVP サイト構築のテンプレート
- `.codex/commands/commands_setup_tsuwano_tourisum/README.md`: Codex 版 commands_setup の案内

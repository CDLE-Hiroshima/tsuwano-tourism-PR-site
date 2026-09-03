# commands_setup_tsuwano_tourisum for Codex

このディレクトリは、津和野PRサイト教材の `Codex` 用 `commands_setup` 一式です。

## 前提

- `Codex` には、リポジトリ内の markdown を自動で slash command として登録する仕組みはありません。
- そのため各ファイルは「Codex に読ませて実行させる運用ドキュメント」として使います。
- 使い方は共通です。

```text
まず AGENTS.md を読んで。
次に .codex/commands/commands_setup_tsuwano_tourisum/<file>.md を読んで、その指示に従って進めて。
```

## 構成

| Phase | File | 役割 |
|---|---|---|
| 0 | `bootstrap.md` | `project-brief.md` 補完、README 同期、進捗台帳の初期化 |
| MVP | `build-site-mvp.md` | 津和野PRサイトの最初の 1 ページを作る |
| 1 | `setup-docs.md` | 永続ドキュメント群を作る |
| 2 | `setup-marketplace.md` | Codex で使う外部機能の方針を整理する |
| 3 | `setup-codex-md.md` | `AGENTS.md`、共有文書、`.steering` を整える |
| 4 | `setup-skills.md` | 再利用用の playbook / reference を作る |
| 5 | `setup-agents.md` | 専門役割ごとの agent prompt を作る |
| 6 | `setup-commands.md` | 日常開発用の Codex prompt command を作る |
| 7 | `setup-hooks.md` | hook の代わりになる guardrail script / checklist を作る |
| 8 | `verify-setup.md` | 全体の整合性を検証する |
| ref | `skill-empirical-prompt-tuning.md` | prompt / playbook の実測改善手法 |

## この構成の特徴

- repo の主要指示書として `AGENTS.md` を使う
- command 定義は `.codex/commands` に置く
- repo 内 hook 自動実行ではなく、明示実行の verify script と checklist を使う
- repo 内 skill は自動ロード前提にせず、`.codex/references/` の再利用 playbook として管理する

## 推奨順序

1. `bootstrap.md`
2. `build-site-mvp.md`
3. 必要なら `setup-docs.md` 以降を順に実行

勉強会で最短でサイトを作るだけなら、`bootstrap.md` と `build-site-mvp.md` だけで十分です。

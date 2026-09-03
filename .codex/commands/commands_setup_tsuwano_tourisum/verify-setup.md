# verify-setup.md

## 目的

`Codex` 版 `commands_setup` の構築物が揃っているか、相互参照が破綻していないかを確認する。

## 確認対象

- `AGENTS.md`
- `project-brief.md`
- `README.md`
- `docs/functional-design.md`
- `docs/architecture.md`
- `docs/repository-structure.md`
- `docs/development-guidelines.md`
- `docs/glossary.md`
- `docs/agent-shared.md`
- `docs/external-tools.md`
- `.steering/_setup-progress.md`
- `.steering/_template/*`
- `.codex/commands/commands_setup_tsuwano_tourisum/*`
- `.codex/references/*`
- `.codex/agents/*`
- `.codex/commands/common/*`

## 実行フロー

1. ファイル存在確認をする
2. `README.md` / `project-brief.md` / `AGENTS.md` の整合を確認する
3. stale な旧構成参照が無いか検索する（例: `.claude`, `CLAUDE.md`）
4. `docs/` 間で矛盾が無いか見る
5. `commands`, `agents`, `references` の責務分離を確認する
6. 進捗ファイルを更新し、未完了 phase を列挙する

## 品質チェック観点

- brief を SSOT として扱っているか
- `AGENTS.md` が肥大化しすぎていないか
- `Codex` では実現できない前提を docs に書いていないか
- `README.md` の導線が現在の構成と一致しているか

## 完了条件

- 必須ファイルが揃っている
- stale な旧構成前提が無い
- どこから何を読めばいいか迷わない

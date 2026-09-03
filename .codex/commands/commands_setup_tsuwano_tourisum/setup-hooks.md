# setup-hooks.md

## 目的

この phase では、自動 hook の代わりに、`Codex` で運用可能な verify script と checklist を整備する。

## 前提

- repo 内設定だけで `Codex` の session hook を自動登録することは想定しない
- 代わりに、明示実行できる安全装置を作る

## 出力対象の例

- `scripts/verify/preflight.sh`
- `scripts/verify/post-edit.sh`
- `scripts/verify/self-check.sh`
- `.codex/checklists/session-start.md`
- `.codex/checklists/pre-commit.md`

## 実行フロー

1. この repo で毎回確認したいことを洗い出す
   - brief を読んだか
   - build が通るか
   - 禁止事項に触れていないか
   - lint / format が必要か
2. 自動化できるものは `scripts/verify/` に寄せる
3. 人間判断が残るものは checklist にする
4. `README.md` か `docs/development-guidelines.md` に実行方法を書く

## ルール

- 壊れやすい IDE 依存 hook を前提にしない
- まずは手動でも回る verify を作る
- false positive の多い guard は避ける

## 完了条件

- 明示実行できる verify script がある
- セッション開始時 / 作業終了時の確認項目が明確
- `Codex` 固有の制約に無理なく合っている

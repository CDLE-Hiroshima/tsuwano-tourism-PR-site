# setup-agents.md

## 目的

`Codex` の subagent や役割分担を使うときのために、
専門役割ごとの prompt card を `.codex/agents/` に置く。

## 想定する agent prompt 例

- `file-finder.md`
- `dependency-checker.md`
- `impact-analyzer.md`
- `code-reviewer.md`
- `test-analyzer.md`
- `security-checker.md`
- `test-runner.md`
- `build-executor.md`
- `log-analyzer.md`

## 各ファイルに書くこと

- いつ使うか
- 何を見るか
- どこまでやるか
- 返答フォーマット
- やってはいけないこと

## 実行フロー

1. 日常的に切り分けたい作業役割を決める
2. 役割ごとに 1 ファイル作る
3. 返答フォーマットを揃える
4. `setup-commands.md` 側から参照しやすいように命名を整える

## 注意

- これは repo 内の reusable prompt card 集であり、自動登録前提ではない
- repo 内の reusable prompt card として使う
- 自動化しすぎず、まずは数を絞る

## 完了条件

- `.codex/agents/` が存在する
- 主要な役割の prompt card がある
- どの役割をどの場面で使うかが明確

# setup-commands.md

## 目的

`Codex` に繰り返し渡す作業指示を、`.codex/commands/common/` の prompt command として整える。

## 想定する command prompt

- `start-task.md`
- `add-feature.md`
- `fix-bug.md`
- `refactor.md`
- `review-changes.md`
- `reimagine.md`
- `smart-compact.md`
- `finish-task.md`

## 使い方の前提

これらは slash command ではない。次のように使う。

```text
AGENTS.md を読んでから .codex/commands/common/add-feature.md を読んで、その流れで進めて。
```

## 各ファイルに含めるべき要素

- 目的
- 入力
- 実行手順
- 出力形式
- 完了条件
- アンチパターン

## 実行フロー

1. 日常作業で何度も使う依頼を洗い出す
2. 1 command = 1 目的で分ける
3. `.codex/references/` や `.codex/agents/` を参照する設計にする
4. 重要な command は `skill-empirical-prompt-tuning.md` で評価する

## 完了条件

- `.codex/commands/common/` に日常 command がある
- 目的ごとに流れが分かれている
- 「何を読んでから着手するか」が書かれている

# skill-empirical-prompt-tuning.md

## 目的

`Codex` に渡す prompt、command markdown、reference playbook、agent prompt card の品質を、
実測ベースで改善する。

## 対象

- `.codex/commands/common/*.md`
- `.codex/commands/commands_setup_tsuwano_tourisum/*.md`
- `.codex/references/*.md`
- `.codex/agents/*.md`
- `AGENTS.md` の重要節

## 基本原則

- 書き手の自己評価ではなく、別の実行者視点で詰まりを観測する
- 1 回で完成扱いにしない
- 量的指標より、不明瞭点の発見を重視する

## 最小ワークフロー

1. 対象 prompt を 1 つ選ぶ
2. その prompt を使う具体的シナリオを 1 つ決める
3. 要件チェックリストを 3〜5 個作る
4. `Codex` にその prompt を読ませて実行させる
5. 次を記録する
   - どこで迷ったか
   - どこを推測で補ったか
   - 何が足りなかったか
   - 成果物は要件を満たしたか
6. 最小修正だけ入れて再実行する

## 向いている対象

- 何度も使う command prompt
- 他人にも渡す運用ドキュメント
- 曖昧さが事故につながる instruction

## 向いていない対象

- 一度しか使わない雑多な依頼
- 成否判定がほぼ自明な短文

## 完了条件

- 修正前より詰まりどころが減っている
- 指示の責務分離が明確になっている
- 他人が読んでも流れを再現しやすい

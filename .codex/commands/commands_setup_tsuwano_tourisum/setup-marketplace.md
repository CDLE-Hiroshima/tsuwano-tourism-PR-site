# setup-marketplace.md

## 目的

ここでは plugin 前提に寄せすぎず、`Codex` で使う外部機能や補助能力を棚卸しし、
`docs/external-tools.md` に記録する。

## このフェーズで扱うもの

- `Codex` から既に使える skill / connector / subagent
- 将来使う可能性がある外部サービス
- この教材であえて使わないもの

## 出力対象

- `docs/external-tools.md`

## `docs/external-tools.md` の推奨構成

| Capability | Source | Use when | Constraints |
|---|---|---|---|
| Web research | built-in web | 最新情報が必要な時 | 出典確認が必要 |
| Image generation | image tool | プレースホルダ画像やムード検討 | 実写素材の代替扱い |
| GitHub / deploy tools | if available | 公開や PR 作成時 | 事前認証が必要な場合あり |

## 実行フロー

1. 現在の作業で本当に必要な外部能力を洗い出す
2. すでに使える能力と、今は使わない能力を分ける
3. `docs/external-tools.md` に記録する
4. 以後の phases で重複説明しないよう、ここを参照元にする

## ルール

- ない能力をある前提で書かない
- 使えるか未確認のものは「候補」と明記する
- 単なる wish list ではなく、用途と制約を書く

## 完了条件

- `docs/external-tools.md` がある
- 何を `Codex` 標準で行い、何を外部機能に頼るかが分かる

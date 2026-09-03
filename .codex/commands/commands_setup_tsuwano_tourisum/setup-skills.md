# setup-skills.md

## 目的

`Codex` で再利用したい知見を、repo 内の自動 skill ではなく
`.codex/references/` の playbook として整備する。

## 出力対象の例

- `.codex/references/index.md`
- `.codex/references/implementation-workflow.md`
- `.codex/references/copywriting-from-brief.md`
- `.codex/references/cloudflare-pages-deploy.md`
- `.codex/references/content-markdown-cms.md`

## 実行フロー

1. このプロジェクトで繰り返し使う判断を洗い出す
2. 1 playbook = 1 目的で分ける
3. 各 playbook に最低限次を書く
   - いつ使うか
   - 必要入力
   - 手順
   - 出力
   - 失敗しやすい点
4. `index.md` で一覧化する
5. `setup-commands.md` や `setup-agents.md` から参照しやすくする

## ルール

- `Codex` が自動で読む前提では書かない
- 人が見ても流れが追えるようにする
- ツール名より判断基準を重視する
- 汎用論ではなく、この repo で再利用する前提で書く

## 完了条件

- `.codex/references/` が存在する
- よく使う作業の型が playbook 化されている
- `Codex` に「まずこの reference を読んで」と渡せる状態になっている

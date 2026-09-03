# build-site-mvp.md

## 目的

`project-brief.md` の内容を使い、`web/` に津和野PRサイトの MVP を作る。
ここでのゴールは「動く 1 ページ」であり、公開や CMS 化までは含めない。

## Codex への渡し方

```text
AGENTS.md と project-brief.md と .codex/commands/commands_setup_tsuwano_tourisum/build-site-mvp.md を読んで実行して。
```

## 事前確認

1. `project-brief.md` の空欄がないか確認する
2. `node --version` と `npm --version` を確認する
3. `web/` が存在する場合は、流用か再作成かを確認する

## 実行フロー

1. brief の要点を整理する
   - ⑪: ヒーロー見出し
   - ②: リード文
   - ③ / ⑤ / ⑨: 魅力紹介
   - ⑦: 締めの一文
   - ⑧: 配色 / 余白 / 文体
   - ⑩: 禁止事項
2. `web/` が無ければ Next.js を最小構成で作る
3. `app/page.tsx` と `app/globals.css` を brief に合わせて更新する
4. `app/layout.tsx` の metadata に brief の要旨を反映する
5. ⑩の禁止事項に違反していないか自己照合する
6. `cd web && npm run build` を実行する
7. 結果を `.steering/_setup-progress.md` に記録する

## 実装ルール

- 最初は 1 ページだけに絞る
- 画像が無ければプレースホルダで進める
- 一般的な観光サイトの文言に逃げず、brief の言葉を優先する
- 「小京都」「映え」のような安直な表現は避ける

## 仕上がりの最低条件

- ヒーローに ⑪がある
- ②が分かりやすく見える
- ③ / ⑤ / ⑨ を使った魅力紹介がある
- ⑦が余韻として機能している
- スマホでも読める
- `npm run build` が通る

## 次に伸ばす例

- 「まず project-brief.md を読んで、⑨のモチーフを使ったセクションを 1 つ追加して」
- 「まず project-brief.md を読んで、⑧と⑩に沿って文章を推敲して」
- 「まず project-brief.md を読んで、Cloudflare Pages へ出す前提で構成を整えて」

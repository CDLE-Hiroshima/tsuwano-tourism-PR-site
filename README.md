<!--
================================================================================
 README テンプレート（AIエージェント勉強会 / 津和野 観光PRサイト）
================================================================================
 このファイルは「参加者ごとに違う表紙」になります。
 /bootstrap の Step 4.5 が、あなたの project-brief.md の内容を
 本文中の brief マーカー（コメント形式: brief:xxx）の位置に差し込んで、
 あなた専用の README を作ります。
 手で埋めても構いません。マーカーは消さないでください（再生成で使います）。
 ※ このコメント内では、入れ子コメントで途中終了しないようマーカー記号を省いて表記しています。
================================================================================
-->

# 津和野 観光PRサイト — AIエージェントと作る

> <!-- brief:hero -->（ここに project-brief.md ⑪「トップの第一声」が入ります）

これは **AIエージェント勉強会** の教材です。あなたは Claude Code（AIエージェント）と一緒に、
**津和野町の観光PRサイトを今日この場で作り上げます**。技術スタックは **Next.js / React**。
作るサイトの中身は、あなたが `project-brief.md` に込めた「想い」で一人ひとり変わります。

- **リポジトリ**: https://github.com/CDLE-Hiroshima/tsuwano-tourism-PR-site.git
- **発注書 (SSOT)**: [`project-brief.md`](./project-brief.md) — 何を・どんな気持ちで作るかの唯一の正
- **参加者**: <!-- brief:author -->（お名前 / ニックネーム）

---

## このサイトが伝えたいこと

<!-- brief:about -->
（ここに project-brief.md ②「15秒で言うと」＋⑦「一番贈りたい気持ち」が入ります）

### 誰に届けるか

<!-- brief:target -->
（ここに project-brief.md ⑥「来てほしい、たった一人」が入ります）

### 声のトーン

<!-- brief:tone -->
（ここに project-brief.md ⑧「声のトーン（3語）」が入ります）

### AI への禁止事項（レビューの絶対基準）★最重要

<!-- brief:forbidden -->
（ここに project-brief.md ⑩「避けたい表現・扱い」が入ります。生成物は必ずこれと照合します）

---

## 進め方（勉強会の流れ）

初心者は **コマンド駆動で MVP まで** → その後は **Claude への直接依頼** で伸ばします。

### ステップ 0 — 発注書を完成させる

```
/bootstrap
```

AI が `project-brief.md` の空欄【　】を 1 問ずつ質問して、あなたの言葉で埋めます。
そのままこの README もあなた専用に生成されます。

### ステップ 1 — 動く最初の1枚（MVP）を作る

```
/clear
/build-site-mvp
```

`project-brief.md` を入力に、Next.js の津和野PRサイトが `web/` に生成されます。
ブラウザで確認:

```bash
cd web && npm run dev   # → http://localhost:3000
```

### ステップ 2 — Claude に直接お願いして伸ばす

MVP のあとは、Claude Code に **直接お願い**して 1 手ずつ育てます。コツは
**「まず `project-brief.md` を読んでから、○○して」** と、発注書を起点にさせること。例:

- 「⑨のモチーフでセクションを1つ追加して」
- 「⑧の声のトーンと⑩の禁止事項に沿って文章を推敲して」
- 「写真ギャラリーを追加して（実写はプレースホルダで）」
- 「スマホ対応とアクセシビリティを整えて」

### ステップ 3 — 公開する / 運用する（発展）

- **プライベート公開**: **Cloudflare Pages** に載せ、**ユーザー名＋パスワード**で
  関係者だけが入れる形にする。
  → 「web/ を Cloudflare Pages にユーザー名＋パスワード付きでプライベート公開して」
- **保守・運用**: サイトの文章を **git 内の Markdown** にし、**Sveltia CMS** の管理画面から
  非エンジニアでも編集できるようにする。
  → 「web/ のコンテンツを Markdown 化して Sveltia CMS で編集できるようにして」

### （任意・上級）AIエージェントの土台づくりを学ぶ

環境構築そのものを学びたい人は、`/bootstrap` の後に `/setup-docs` から Phase 1〜9
（docs / Skill / サブエージェント / コマンド / Hook）を順に構築できます。
勉強会でサイトを作るだけなら **飛ばして構いません**。

---

## 今日のゴール

- **今日のゴール**: <!-- brief:goal -->（MVP まで / セクション追加まで / デプロイまで）
- **公開先**: <!-- brief:deploy -->（Cloudflare プライベート公開 / ローカル確認 / 未定）

---

## ディレクトリの見取り図

```
tsuwano-tourism-PR-site/
├── project-brief.md          # ★発注書（あなたの想い。まずここを埋める）
├── README.md                 # このファイル（あなた専用の表紙）
├── web/                      # ★作る津和野PRサイト（Next.js。/build-site-mvp で生成）
├── .steering/
│   └── _setup-progress.md    # 進捗記録（/clear をまたいで続きから）
└── .claude/commands/commands_setup_tsuwano_tourisum/
    ├── bootstrap.md          # /bootstrap（入口・発注書補完・README生成）
    ├── build-site-mvp.md     # /build-site-mvp（MVP を作る）
    └── setup-*.md            # 環境構築 Phase 1〜9（任意・上級）
```

---

## 困ったら

- サイトが表示されない → `cd web && npm run dev` のエラーメッセージをそのまま Claude Code に貼る
- 何を頼めばいいか分からない → 「まず project-brief.md を読んで、次にやるべきことを提案して」と聞く
- 大きく作り直したい → `project-brief.md` を書き換えてから、「更新した project-brief.md に合わせて直して」
- 途中で崩れた → 大きな作業の前に `/clear` でリセットすると安定します

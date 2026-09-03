<!--
================================================================================
 README テンプレート（AIエージェント勉強会 / 津和野 観光PRサイト）
================================================================================
 このファイルは「参加者ごとに違う表紙」になります。
 Codex に `project-brief.md` を読ませながら、本文中の brief マーカー
 （コメント形式: brief:xxx）の位置を更新して使います。
 手で埋めても構いません。マーカーは消さないでください（再利用しやすくするため）。
 ※ このコメント内では、入れ子コメントで途中終了しないようマーカー記号を省いて表記しています。
================================================================================
-->

# 津和野 観光PRサイト — AIエージェントと作る

<!-- brief:hero -->
> **ここだけにしかない、自分自身と向き合える静寂の空間を。**

これは **AIエージェント勉強会** の教材です。あなたは Codex（AIエージェント）と一緒に、
**津和野町の観光PRサイトを今日この場で作り上げます**。技術スタックは **Next.js / React**。
作るサイトの中身は、あなたが `project-brief.md` に込めた「想い」で一人ひとり変わります。

- **リポジトリ**: https://github.com/CDLE-Hiroshima/tsuwano-tourism-PR-site.git
- **発注書 (SSOT)**: [`project-brief.md`](./project-brief.md) — 何を・どんな気持ちで作るかの唯一の正
- **参加者**: <!-- brief:author -->☆卍 絶対領域豆腐メンタルﾏﾝ 卍☆

---

## このサイトが伝えたいこと

<!-- brief:about -->
鯉の泳ぐ掘割とSL、そして静かな教会が残る、森のなかの宿場町。
その静けさのなかで、来た人の**呼吸音がそっと戻ってくるような安堵感**を残したい。

### 誰に届けるか

<!-- brief:target -->
東京都在住、30代後半。週末ごとに騒がしい場所ばかりで、心がどんどんすり減っていくのを感じている。
SNSより本が好き。静かに自分と向き合いたい——そんな、たった一人へ。

### 声のトーン

<!-- brief:tone -->
**静かで・誠実で・少し文学的。**
（活かしたいモチーフ: 鯉／朝霧／畳の教会／源氏巻／SLの汽笛／青野山）

### AI への禁止事項（レビューの絶対基準）★最重要

<!-- brief:forbidden -->
- **殉教の歴史を"映えスポット"として軽く扱わない。**（津和野を凡庸にしないための絶対基準。すべての生成物はこれと照合する）

---

## 進め方（勉強会の流れ）

初心者は **テンプレート駆動で MVP まで** → その後は **Codex への直接依頼** で伸ばします。

### ステップ 0 — 発注書を完成させる

まず [`AGENTS.md`](./AGENTS.md) を置いた上で、Codex に次のどちらかを渡します。

- [`.codex/prompts/bootstrap.md`](./.codex/prompts/bootstrap.md) をそのまま読ませる
- もしくは「まず `project-brief.md` を読んで。空欄があれば 1 問ずつ質問して埋め、終わったら `README.md` の brief セクションも更新して」と依頼する

これで `project-brief.md` の空欄【　】を埋めながら、この README も参加者専用に更新できます。

### ステップ 1 — 動く最初の1枚（MVP）を作る

Codex に [`.codex/prompts/build-site-mvp.md`](./.codex/prompts/build-site-mvp.md) を読ませるか、
次のように依頼します。

```text
まず project-brief.md を読んで。Next.js で web/ に津和野PRサイトのMVPを作って。
⑧の声のトーンと⑩の禁止事項を守り、最後に npm run build まで確認して。
```

`project-brief.md` を入力に、Next.js の津和野PRサイトが `web/` に生成されます。
ブラウザで確認:

```bash
cd web && npm run dev   # → http://localhost:3000
```

### ステップ 2 — Codex に直接お願いして伸ばす

MVP のあとは、Codex に **直接お願い**して 1 手ずつ育てます。おすすめは
**「まず `project-brief.md` を読んでから、○○して」** と、発注書を起点にさせることです。例:

- 「⑨のモチーフでセクションを1つ追加して」
- 「⑧の声のトーンと⑩の禁止事項に沿って文章を推敲して」
- 「写真ギャラリーを追加して（実写はプレースホルダで）」
- 「スマホ対応とアクセシビリティを整えて」

必要なら issue を切ってから Codex に読ませても構いません。外部調査が必要な改善は、
「先に課題を整理してから Web 上の一次情報を確認して」と明示すると精度が上がります。
大きい改善を分けて進めたいときは、`.steering/` に観点を残して新しいセッションで続けると安定します。

### ステップ 3 — 公開する / 運用する（発展）

- **プライベート公開**: **Cloudflare Pages** に載せ、**ユーザー名＋パスワード**で
  関係者だけが入れる形にする。
  → 「web/ を Cloudflare Pages にユーザー名＋パスワード付きでプライベート公開して」
- **保守・運用**: サイトの文章を **git 内の Markdown** にし、**Sveltia CMS** の管理画面から
  非エンジニアでも編集できるようにする。
  → 「web/ のコンテンツを Markdown 化して Sveltia CMS で編集できるようにして」

### （任意・上級）Codex 向けの土台づくりを学ぶ

より運用を固めたい人は、`AGENTS.md` や `.codex/prompts/` に加えて、
[`.codex/commands/commands_setup_tsuwano_tourisum/README.md`](./.codex/commands/commands_setup_tsuwano_tourisum/README.md)
から `Codex` 版 `commands_setup` を順に使えます。
この教材では、まず `project-brief.md` を SSOT にして MVP を作ることを優先します。

---

## 今日のゴール

- **今日のゴール**: <!-- brief:goal -->デプロイまで
- **公開先**: <!-- brief:deploy -->Cloudflare Pages にプライベート公開（ユーザー名＋パスワード）

---

## ディレクトリの見取り図

```
tsuwano-tourism-PR-site/
├── AGENTS.md                 # Codex に最初に読ませる repo 方針
├── project-brief.md          # ★発注書（あなたの想い。まずここを埋める）
├── README.md                 # このファイル（あなた専用の表紙）
├── web/                      # ★作る津和野PRサイト（Next.js。Codex が生成）
├── .codex/
│   ├── prompts/
│   │   ├── bootstrap.md      # 発注書補完と README 更新用のテンプレート
│   │   └── build-site-mvp.md # MVP 構築用のテンプレート
│   └── commands/
│       └── commands_setup_tsuwano_tourisum/
│           ├── README.md     # Codex 版 commands_setup の入口
│           ├── bootstrap.md  # Phase 0
│           ├── build-site-mvp.md
│           └── setup-*.md    # 任意の環境整備フェーズ
├── .steering/
│   └── _setup-progress.md    # 進捗記録（任意。大きい作業の引き継ぎ用）
└── docs/                     # 補足ドキュメント置き場
```

---

## 困ったら

- サイトが表示されない → `cd web && npm run dev` のエラーメッセージをそのまま Codex に貼る
- 何を頼めばいいか分からない → 「まず project-brief.md を読んで、次にやるべきことを提案して」と聞く
- 大きく作り直したい → `project-brief.md` を書き換えてから、「更新した project-brief.md に合わせて直して」
- 途中で崩れた → 新しい Codex セッションを開き、`AGENTS.md` と `project-brief.md` を読ませ直すと安定します

---
description: >
  津和野町 観光PRサイトを AIエージェント勉強会で作るための入口コマンド。
  まずディレクトリ直下の project-brief.md（津和野の想いを言語化した SSOT）を読み込み、
  空欄【　】が残っていれば参加者に 1 問ずつ質問して埋めてから、環境チェック・
  進捗記録ファイル作成・参加者ごとの README 生成・構築計画の提示を行う。
  勉強会を始める時に最初に実行する。他の setup-* コマンドはすべてこのコマンドの
  実行後に順番に実行される前提。技術スタックは Next.js / React。
allowed-tools: Read, Write, Edit, Glob, Grep, Bash(mkdir *), Bash(git *), Bash(date *), Bash(ls *), Bash(cat *), Bash(find *), AskUserQuestion
---

# /bootstrap — 津和野PRサイト勉強会の入口

> 一発でやろうとしない。9 段階 (Phase 0〜8) に分け、各セッションで一つずつ完璧に仕上げる。
> Let's think step by step.

## このコマンドは何のためにあるか（勉強会ファシリテーターへ）

これは **「AIエージェント勉強会」の教材** であり、参加者が Claude Code (AIエージェント) と一緒に
**津和野町の観光PRサイトを今日この場で作り上げる** ための出発点です。

- **成果物**: Next.js / React で作る津和野観光PRサイト（1 人 1 サイト、参加者ごとに内容が違う）
- **入力 (発注書)**: ディレクトリ直下の `project-brief.md` — 津和野の「想い・らしさ・禁止事項」を
  参加者自身の言葉で埋めた **唯一の正 (SSOT)**。全エージェントがこれを起点に動く。
- **リポジトリ**: https://github.com/CDLE-Hiroshima/tsuwano-tourism-PR-site.git

このコマンドは **参加者ごとに違う「想い」を最初に引き出し**、それを `project-brief.md` に
書き込み、その内容を反映した **参加者専用の README.md** を生成した上で、環境構築へ進みます。

## このコマンドの目的

環境構築の **唯一の手動配置ファイル** として機能する。このコマンドが他のすべての構築コマンドの実行を統括し、進捗を `.steering/_setup-progress.md` に記録することで、`/clear` を挟んでも次のセッションが前回の続きから始められる。加えて本コマンドは、**`project-brief.md` の空欄補完** と **参加者専用 README.md の生成** という、勉強会に固有の 2 つの下ごしらえを担う。

## 設計方針 (公式 Skill 連携対応版)

このセットアップは Claude Code 単独運用を前提とし、任意で以下を統合できる:

- **anthropics/skills 公式 plugin marketplace** — pdf/docx/xlsx/pptx (proprietary)、claude-api、mcp-builder、skill-creator、webapp-testing、web-artifacts-builder、frontend-design、theme-factory 等の公式 Skill を Phase 2 で導入。自前 Skill との重複を Phase 4 で回避する。

marketplace を使わない構成も可能。各 Phase 冒頭に skip 可否を明記する。

## 環境チェックブロック（必須）

このコマンドを実行する前に、以下を順番に確認してください。**一つでも満たしていない場合は中断し、ユーザーに通知してください。**

### Check 1: コンテキスト予算

ユーザーに以下を実行するよう依頼:

```
/context
```

使用率が **30% を超えている場合は中断**し、「`/clear` でセッションをリセットしてから再実行してください」と通知。

### Check 2: モデル

ユーザーに以下を実行するよう依頼:

```
/model
```

現在のモデルを確認。**Opus でない場合は警告**を出し、「設計判断のため `/model opus` への切り替えを強く推奨します」と通知。ユーザーが Sonnet で続けたい意思を示した場合のみ進む。

### Check 3: Plan Mode

Plan Mode に入っているかをユーザーに確認。入っていない場合は「`Shift+Tab` を 2 回押して Plan Mode に入ることを推奨します」と通知。

### Check 4: 作業ディレクトリ

```bash
pwd
git rev-parse --is-inside-work-tree 2>/dev/null
ls project-brief.md 2>/dev/null
```

**確認したいこと**: (a) git リポジトリの中にいるか、(b) `project-brief.md` が直下にあるか。

- **git リポジトリでない場合** — 勉強会では次のどちらかを参加者に案内する（中断はしない）:
  - 教材リポジトリをクローンして始める:
    `git clone https://github.com/CDLE-Hiroshima/tsuwano-tourism-PR-site.git`
  - すでに `project-brief.md` があるこのフォルダで新規に始める: `git init`
- **`project-brief.md` が無い場合** — 中断する。「このフォルダは津和野PRサイト教材ではありません。教材リポジトリのルートで実行してください」と通知。`project-brief.md` は本コマンドの入力 (SSOT) なので必須。

### Check 5: 既存の Claude Code 環境

```bash
ls -la .claude/ 2>/dev/null
ls -la docs/ 2>/dev/null
ls CLAUDE.md 2>/dev/null
```

既存の構築物がある場合、ユーザーに「既存の構築物が見つかりました。上書きしますか?それとも追記モードで進みますか?」と確認。

### Check 6: gh CLI（任意・Phase 2 の marketplace 導入で使用）

```bash
gh --version 2>&1 | head -1
```

**警告止まり** (block しない):

- `gh` 未インストールの場合: 「Phase 2 (`/setup-marketplace`) で `gh api` を使います。marketplace を導入するなら後でインストールしてください」

Phase 2 の冒頭で再確認するため、ここでは進行を止めない。

## 実行フロー

### Step 1: プロジェクトの初期調査

`file-finder` サブエージェントが存在しない（このコマンドが最初の実行）ため、ここではメインエージェントが直接以下を実行:

```bash
ls -la
cat README.md 2>/dev/null | head -50
find . -maxdepth 2 -type f -name "*.toml" -o -name "*.json" -o -name "*.yaml" 2>/dev/null
```

構成を簡単に把握する。**深く読まない**。津和野PRサイト教材なので技術スタックは **Next.js / React 固定**（改めて聞かない）。

### Step 1.5: project-brief.md の読み込みと空欄補完（勉強会の核）

> このステップが勉強会の肝。**参加者一人ひとりの「想い」を引き出して発注書 (`project-brief.md`) を完成させる。**
> ここで埋めた内容が、以降のすべてのエージェント／サイト実装の入力になる。

#### 1.5.1 現状の読み込み

```bash
cat project-brief.md
```

`project-brief.md` を Read で読み、`【　】`（全角スペースだけの空欄）が残っている項目を洗い出す。
項目番号は ①〜⑪（記入者名・記入日を含む）。

#### 1.5.2 埋め方の原則

- **すでに埋まっている項目は尊重してそのまま使う。** 勝手に上書き・改変しない。
- **空欄の項目だけ**、参加者に質問して埋める。
- ブリーフ冒頭のコメント（AIエージェントへの注意書き）と、各項目のガイド文（`>` 引用）を
  質問文にそのまま活かす。特に **C-⑩「避けたい表現・扱い（禁止事項）」は最重要**。

#### 1.5.3 質問の進め方

**一度に 1 問ずつ**。選択肢が自然な項目（例: ⑧声のトーン）は `AskUserQuestion` ツールで
選択式にしてよいが、①③⑤⑥⑦⑨⑩⑪ のような自由記述は素直にテキストで聞く。

空欄の項目を上から順に、例えば次のように問いかける（ガイド文を要約して添える）:

- ①「『小京都』を使わずに、津和野を一言で名指すと？」
- ②「津和野を全く知らない都会の人に、言葉だけで15秒で説明すると？」
- ⑤「津和野の『人が少ない』を、欠点ではなく魅力として言い換えると？」
- ⑥「来てほしいたった一人は、どんな人ですか？（年齢・住む場所・休日の過ごし方・悩みまで）」
- ⑦「その人に一番贈りたい『気持ち』をひとつだけ挙げると？」
- ⑧「サイト全体の声のトーンを形容詞 3 つで。（例: 静かで・誠実で・少し文学的）」
- ⑩「AI が絶対にやってはいけないこと（禁止事項）は？（例:『小京都』多用禁止、殉教史を映えスポット扱いしない）」
- ⑪「トップに最も大きく置く第一声（この町の全部を一行に）は？」

> 参加者が「思いつかない」場合は、ガイド文の例を **たたき台** として提示し、そこから
> 参加者自身の言葉に直してもらう。空欄のまま放置しない（一言でも入れる）。

#### 1.5.4 project-brief.md への書き戻し

回答を得るたびに、`Edit` ツールで該当する `【　】` を参加者の言葉に置き換える。
記入者名（冒頭）と記入日も埋める:

```bash
date +%Y-%m-%d
```

全項目を埋め終えたら、`project-brief.md` を通しで Read し直し、
**空欄が 1 つも残っていないこと** を確認して参加者に全文を提示する。

> **記入日はコマンド実行日を使う。** ハードコードしない（`date` の結果を使う）。

### Step 2: 勉強会の運営ヒアリング（brief 以外の実務事項）

`project-brief.md` で「何を・どんな気持ちで作るか」は埋まった。ここでは **運営上の実務** だけを
補足で聞く。**一度に 1 問ずつ**。

1. お名前 / ニックネーム（README とコミットに使います。brief 記入者名と同じでよいか）
2. AIエージェント / コーディングの経験レベルは?（初めて / 少し触った / 普段から書く）
   → 経験レベルは README とサポートの手厚さの調整に使う（全員まず環境構築 `/setup-docs` へ進む）
3. 今日のゴールはどこまで?（MVP まで / セクション追加まで / デプロイまで）
4. 公開先の希望はありますか?（Vercel / GitHub Pages / ローカルで確認だけ / 未定）

回答を `.steering/_setup-progress.md` の「プロジェクト概要」セクションに記録する（次のステップで作成）。
技術スタック（Next.js / React）とプロダクト（津和野観光PRサイト）は固定なので聞き直さない。

### Step 3: ディレクトリ構造の作成

```bash
mkdir -p .claude/commands
mkdir -p .claude/agents
mkdir -p .claude/skills
mkdir -p .claude/hooks
mkdir -p .steering/_template
mkdir -p docs
```

### Step 4: 進捗記録ファイルの作成

`.steering/_setup-progress.md` を以下の内容で作成:

```markdown
# Claude Code 環境構築進捗

> このファイルは構築の進捗を記録する。各 setup-* コマンドが完了するたびに更新される。
> セッションを跨いだ継続のための引き継ぎ情報として機能する。

## プロジェクト概要

- **名称**: 津和野町 観光PRサイト（AIエージェント勉強会）
- **発注書 (SSOT)**: `project-brief.md`（Step 1.5 で参加者が記入済み。全エージェントが起点に読む）
- **目的**: [project-brief.md の「このサイトの最終目的」を転記]
- **技術スタック**: Next.js / React（固定）
- **リポジトリ**: https://github.com/CDLE-Hiroshima/tsuwano-tourism-PR-site.git
- **記入者 / 参加者**: [Step 2-1 の回答]
- **経験レベル**: [Step 2-2 の回答]
- **今日のゴール**: [Step 2-3 の回答]
- **公開先**: [Step 2-4 の回答]
- **重視する品質特性**: 想いの忠実な再現 / project-brief.md ⑩（禁止事項）の遵守
- **構築開始日**: [YYYY-MM-DD]

## 構築進捗

- [ ] **Phase 0: Bootstrap** (このコマンド)
  - 完了日時: -
  - 備考: -
- [ ] **Phase 1: /setup-docs** — 永続ドキュメント
  - 完了日時: -
  - 作成ファイル: -
- [ ] **Phase 2: /setup-marketplace** — anthropics/skills 公式 plugin の導入
  - 完了日時: -
  - 導入 plugin: -
  - ライセンス同意: -
- [ ] **Phase 3: /setup-claude-md** — CLAUDE.md / docs/agent-shared.md / .steering
  - 完了日時: -
  - 作成ファイル: -
- [ ] **Phase 4: /setup-skills** — プロジェクト固有 Skill 群（公式 Skill 重複回避）
  - 完了日時: -
  - 作成 Skill: -
- [ ] **Phase 5: /setup-agents** — サブエージェント群
  - 完了日時: -
  - 作成エージェント: -
- [ ] **Phase 6: /setup-commands** — ワークフローコマンド群
  - 完了日時: -
  - 作成コマンド: -
- [ ] **Phase 7: /setup-hooks** — Hook 群（3 層 + 健康診断 self-check）
  - 完了日時: -
  - 作成 Hook: -
- [ ] **Phase 8: /verify-setup** — 整合性検証
  - 完了日時: -
  - 検証結果: -

## サイト実装トラック（環境構築とは別。ここが勉強会の本番）

> Phase 1〜8 は「AIエージェントを動かす土台」。**環境構築を終えてから、実際の津和野PRサイトを作る。**
> 環境構築の完了後、**コマンド駆動 (`/build-site-mvp`)** で MVP まで一気に到達し、その後は
> **Claude への直接依頼**（「まず project-brief.md を読んで、○○して」）で伸ばす。

- [ ] **MVP: /build-site-mvp** — project-brief.md を入力に、Next.js の津和野PRサイトを
      「動く最初の1枚」まで作る（初心者向けコマンド駆動）
  - 完了日時: -
  - 生成物: `web/`（Next.js プロジェクト）, トップページ, 第一声(⑪), 声のトーン(⑧)反映
- [ ] **反復・公開・運用**（MVP 後、Claude に個別依頼で進める）
  - セクション追加 / コピー推敲 / ギャラリー / スマホ対応
  - 公開: **Cloudflare Pages にプライベート公開**（ユーザー名＋パスワード）
  - 運用（発展）: **git内Markdown + Sveltia CMS** で非エンジニアも編集可能に

## 次に実行すべきコマンド

- **全参加者共通**: まず環境構築へ進む。`/clear` の後、`/setup-docs` から Phase 1〜8 を
  順番に実行して AIエージェントを動かす土台を整える。
- 環境構築（Phase 1〜8）が終わったら `/build-site-mvp` で MVP を作り、その後は
  Claude への直接依頼で伸ばす。

## 各コマンド実行前のチェックリスト

各 setup-* コマンドを実行する前に、以下を必ず確認してください:

1. `/context` で使用率が 30% 以下か
2. 適切なモデルに切り替えてあるか（設計系は Opus、実装系は Sonnet）
3. 前のコマンドが完了し、`/clear` でセッションがリセットされているか
4. このファイル (`.steering/_setup-progress.md`) を Read で読んで進捗を確認したか

## 構築物の相互参照マップ

このセクションは各 setup-* コマンドが完了するたびに更新される。

### Skill → Agent 参照
（/setup-agents 完了時に記入）

### Agent → Command 参照
（/setup-commands 完了時に記入）

### Hook → Command 参照
（/setup-hooks 完了時に記入）
```

### Step 4.5: 参加者専用 README.md の生成

> 勉強会では README は「全員共通の説明書」ではなく **「その参加者の想いを映した表紙」** にする。
> `project-brief.md`（Step 1.5 で埋めた内容）から要点を引用して、参加者ごとに違う README を作る。

リポジトリ直下に `README.md` があるか確認する:

```bash
ls README.md 2>/dev/null
```

- **無い場合** — 教材同梱の雛形（本リポジトリの `README.md`）を土台に生成する。
- **ある場合** — 既存を Read し、`<!-- brief:xxx -->` のような差し込み位置があればそこだけ更新。無ければ「この参加者向けに再生成しますか?」と確認。

生成する README には、`project-brief.md` から以下を **転記** して参加者色を出す:

- タイトル下のリード文 ← ⑪「トップの第一声」
- 「このサイトが伝えたいこと」← ②「15秒で言うと」＋⑦「一番贈りたい気持ち」
- 「声のトーン」← ⑧（3 語）
- 「AI への禁止事項（レビュー基準）」← ⑩（最重要。そのまま箇条書き）
- 「誰に届けるか」← ⑥「来てほしいたった一人」

README には勉強会の進め方（Phase 0 → MVP → 貼り付け式プロンプト）へのリンクも含める。
生成後、参加者に全文を提示して承認を得る。

### Step 5: 他の構築コマンドファイルの配置

ユーザーに以下を通知:

「このコマンドの後、環境構築として `/setup-docs` から `/verify-setup` まで 8 つのコマンドを順番に実行します。それぞれのコマンドファイルは別途配布されているので、`.claude/commands/` に配置してください。環境構築（Phase 1〜8）が終わってから `/build-site-mvp` でサイト作りに進みます。」

配置すべきファイル一覧:
- `setup-docs.md` ← Phase 1
- `setup-marketplace.md` ← Phase 2 (anthropics/skills 導入)
- `setup-claude-md.md` ← Phase 3
- `setup-skills.md` ← Phase 4
- `setup-agents.md` ← Phase 5
- `setup-commands.md` ← Phase 6
- `setup-hooks.md` ← Phase 7
- `verify-setup.md` ← Phase 8
- `build-site-mvp.md` ← MVP（環境構築 Phase 1〜8 の完了後に実行）

### Step 6: 構築計画の提示

ユーザーに以下の構築計画を表で提示:

```markdown
## 構築計画

| Phase | コマンド | 推奨モデル | 推奨所要時間 | 主な成果物 |
|---|---|---|---|---|
| 1 | `/setup-docs` | Opus | 1-2h | docs/ 配下 5 ファイル |
| 2 | `/setup-marketplace` | Sonnet | 30m | 公式 plugin install, docs/external-skills.md |
| 3 | `/setup-claude-md` | Opus | 45m | CLAUDE.md, docs/agent-shared.md, .steering/ |
| 4 | `/setup-skills` | Sonnet | 1-2h | .claude/skills/ (公式重複回避) |
| 5 | `/setup-agents` | Sonnet | 1-1.5h | .claude/agents/ × 9 |
| 6 | `/setup-commands` | Sonnet | 1-1.5h | .claude/commands/ × 8 |
| 7 | `/setup-hooks` | Sonnet | 45m | .claude/hooks/ + settings.json (3 層 + self-check) |
| 8 | `/verify-setup` | Sonnet | 30m | 検証レポート |
| **MVP** | **`/build-site-mvp`** | **Sonnet** | **30-60m** | **`web/`（Next.js 津和野PRサイト・動く最初の1枚）** |
| 反復・公開・運用 | Claude に直接依頼 | Sonnet | 任意 | セクション追加・推敲・Cloudflare 公開・Sveltia CMS |

環境構築（Phase 1〜8）合計: 6-9 時間（1 日強）。

**重要**: 各コマンドの間で必ず `/clear` を実行してください。
**基本ルート**: `/bootstrap` の直後は環境構築へ進む →
`/setup-docs` から Phase 1〜8 を順番に実行 → 完了後に `/build-site-mvp` →
その後は「まず project-brief.md を読んで、○○して」と Claude に直接頼んで 1 手ずつ伸ばす。
```

### Step 7: Grill Me ステップ（Bootstrap 自体の自己レビュー）

ここまでの作業を振り返り、以下を自己点検:

- [ ] 環境チェックは適切だったか
- [ ] ユーザーへのヒアリングで聞き漏らした重要事項はないか
- [ ] `.steering/_setup-progress.md` に記録した情報は次のコマンドで役立つか
- [ ] ディレクトリ構造に抜けはないか

問題があれば修正し、ユーザーに最終確認を求める。

### Step 8: 進捗記録の更新

`.steering/_setup-progress.md` の Phase 0 を完了マーク:

```markdown
- [x] **Phase 0: Bootstrap** (このコマンド)
  - 完了日時: [YYYY-MM-DD HH:MM]
  - 備考: 環境チェック完了、ディレクトリ構造作成済み
```

### Step 9: 完了通知と次のステップ

ユーザーに以下を通知:

```
Bootstrap 完了です。

できたこと:
- project-brief.md の空欄をあなたの言葉で補完（発注書 完成）
- あなた専用の README.md を生成
- .steering/_setup-progress.md（進捗）を用意

次のステップ: 環境構築（Phase 1）に進みます

1. `/clear` でリセット
2. `/model opus` に切り替え、`Shift+Tab` ×2 で Plan Mode
3. `/setup-docs` を実行 → 環境構築（Phase 1〜8）を順番に進める

環境構築（Phase 1〜8）が終わったら、`/build-site-mvp` で
Next.js の津和野PRサイトを「動く最初の1枚」まで作り、
その後は「まず project-brief.md を読んで、○○して」と Claude に直接頼んで伸ばします
（公開は Cloudflare でプライベート、運用は Sveltia CMS、という発展も可能）。

詳細な進捗は `.steering/_setup-progress.md` で確認できます。
```

## 完了条件

- [ ] 環境チェック 5 項目すべてをパスしている
- [ ] **`project-brief.md` の空欄【　】がすべて埋まっている**（Step 1.5。埋済項目は尊重）
- [ ] 運営ヒアリング（名前・経験レベル・今日のゴール・公開先）が完了している
- [ ] **参加者専用の `README.md` が生成され、brief の要点（⑧⑩⑪等）が反映されている**
- [ ] 必要なディレクトリがすべて作成されている
- [ ] `.steering/_setup-progress.md` が作成され、プロジェクト概要が記入されている
- [ ] 構築計画（MVP コマンド `/build-site-mvp` を含む）がユーザーに提示されている
- [ ] Phase 0 が完了マークされている

## アンチパターン

- ❌ **`project-brief.md` を読まずに / 空欄を埋めずに先へ進む**（想いが入力されないと全エージェントが空回り）
- ❌ **空欄でない項目を勝手に上書き・改変する**（参加者の言葉を尊重せよ）
- ❌ **技術スタックを聞き直す**（Next.js / React で固定。プロダクトも津和野PRサイトで固定）
- ❌ 環境チェックをスキップする
- ❌ 運営ヒアリングを省略する
- ❌ **README を全員共通の使い回しにする**（参加者ごとに brief の内容を映すのが目的）
- ❌ `.steering/_setup-progress.md` を作らない（次のコマンドが進捗を引き継げない）
- ❌ ディレクトリだけ作って構築計画を提示しない
- ❌ **bootstrap の直後に `/build-site-mvp`（MVP）へ飛ばす**（次は環境構築 `/setup-docs` から。MVP は Phase 1〜8 の後）
- ❌ Sonnet で実行する（Opus 推奨）

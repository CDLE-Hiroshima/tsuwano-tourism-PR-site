---
description: >
  anthropics/skills 公式 plugin marketplace を導入し、プロジェクトで利用可能な公式 Skill を確定する。
  pdf, docx, xlsx, pptx (proprietary)、claude-api, mcp-builder, skill-creator, webapp-testing,
  web-artifacts-builder, frontend-design 等から、プロジェクトの性質に応じて選択的にインストールする。
  proprietary plugin は明示的なライセンス同意を取得する。/setup-docs の完了後、/setup-claude-md の前に実行する。
allowed-tools: Read, Write, Glob, Bash(gh *), Bash(ls *), Bash(cat *), Bash(mkdir *), Bash(date *), Bash(base64 *)
---

# /setup-marketplace — anthropics/skills 公式 plugin の導入

> Phase 2 of 8. Let's think step by step.

## このコマンドの目的

公式 plugin で網羅できる領域 (PDF 操作、Office 文書生成、Web Artifacts、UI デザイン、Claude API 実装、MCP server 開発、Skill 自動生成、Web アプリテスト) を **自前 Skill 作成より前に確定** する。これにより:

1. Phase 4 (`/setup-skills`) で重複開発を避ける (例: 公式に `pdf` があるなら自前 PDF Skill を作らない)
2. Phase 3 (`/setup-claude-md`) で生成される CLAUDE.md に公式 Skill 一覧とライセンスを反映できる

## このコマンドを skip できる場合

- 全ての Skill を自前で作る方針のプロジェクト
- ネット環境がなく `gh` / `git clone` ができない閉域環境
- Anthropic 利用規約 (proprietary plugin の利用条件) を組織として受け入れられない

skip する場合は `.steering/_setup-progress.md` の Phase 2 を `[~] skipped` でマークし、理由を備考欄に書いて Phase 3 へ進む。**ただし `docs/external-skills.md` は Phase 3 で空テンプレが必ず生成される** ため (`docs/external-skills.md` は公式 Skill とライセンスの一覧の単一の真実源として後続 Phase が参照する契約になっている)、後段は壊れない。Phase 4 の重複回避は「公式 Skill が無い」という状態として正しく解釈される。

## 環境チェックブロック

### Check 1: 進捗ファイル

```bash
cat .steering/_setup-progress.md
```

Phase 1 完了を確認。されていない場合は中断し「先に `/setup-docs` を完了してください」と通知。

### Check 2: gh CLI と GitHub 認証

```bash
gh --version
gh auth status
```

`gh` 未インストール: `brew install gh` を依頼して中断。
未認証: `gh auth login` を依頼して中断。

### Check 3: コンテキスト予算

`/context` で 30% 以下。

### Check 4: モデル

Sonnet 推奨。選択判断は重い推論を要さない。Opus でも可。

### Check 5: 既存の plugin 状態

```bash
ls -la ~/.claude/plugins/ 2>/dev/null
```

既に anthropics/skills 由来の plugin が install されている場合、ユーザーに「再導入しますか?それとも現状を継承しますか?」と確認。

## 実行フロー

### Step 1: anthropics/skills marketplace の取得

gh API で marketplace.json を取得し、利用可能な plugin を確認:

```bash
mkdir -p /tmp/anthropics-skills-cache
gh api repos/anthropics/skills/contents/.claude-plugin/marketplace.json \
  --jq '.content' | base64 -d > /tmp/anthropics-skills-cache/marketplace.json
cat /tmp/anthropics-skills-cache/marketplace.json
```

> **キャッシュ**: 同じセッション内で再実行する場合は再 fetch しない。1 日以内のキャッシュは有効と見なす。

### Step 2: 利用可能な plugin の整理

marketplace.json から以下を抽出してユーザーに表として提示:

| plugin name | 含まれる Skill | License | 主用途 |
|---|---|---|---|
| `document-skills` | pdf, docx, xlsx, pptx | **Proprietary** | Office 文書 / PDF の生成・編集 |
| `claude-api` | claude-api | Apache 2.0 | Anthropic SDK 実装ガイド (Python / TS / Java / Go / Ruby / C# / PHP) |
| `example-skills` | algorithmic-art, brand-guidelines, canvas-design, doc-coauthoring, frontend-design, internal-comms, mcp-builder, skill-creator, slack-gif-creator, theme-factory, web-artifacts-builder, webapp-testing 等 | Apache 2.0 | UI/Artifacts/汎用 |

> 構成は marketplace.json の現状に追従する。表中の Skill 一覧は実際のレスポンスから動的に組み立てる。

### Step 3: ユーザーへの選択ヒアリング

**質問は一度に 1 つずつ。複数まとめない。**

#### 質問 1: document-skills (proprietary) を導入しますか?

用途確認: 「PDF / Word / Excel / PowerPoint の生成・編集を Claude にやらせる必要がありますか?」

**重要なライセンス制約を明示** (省略不可):

> document-skills (pdf / docx / xlsx / pptx) は **Proprietary** ライセンスです:
>
> - Anthropic Services 内での利用に限定 (Anthropic Consumer/Commercial Terms に従属)
> - Services から抽出して外部保存することは禁止
> - 自動キャッシュ以外の複製、派生著作物の作成は禁止
>
> 上記制約を理解した上で導入しますか? (Y/n)

`n` の場合は document-skills をスキップ。`Y` の場合は同意ログを `.steering/_setup-progress.md` の Phase 2 セクションに残す (日時 + ユーザー確認済み)。

#### 質問 2: claude-api を導入しますか?

用途: Anthropic SDK / Claude API を使うコードがプロジェクトに含まれる場合のみ推奨。
License: Apache 2.0、商用利用可。

#### 質問 3: example-skills を導入しますか? (該当する Skill だけサブ選択)

`example-skills` plugin は 13 Skill 程度を **一括 install** する設計。サブセット選択は不可。導入する場合、以下の Skill すべてが入ることを確認:

- 強く推奨 (汎用):
  - `skill-creator` — Phase 4 で自前 Skill を作るときの補助役。**ほぼ必須**
- プロジェクト性質に応じて推奨:
  - `webapp-testing` — Playwright ベース E2E テスト (Web フロント案件)
  - `web-artifacts-builder` — React + Tailwind + shadcn Artifact (UI 自動生成案件)
  - `frontend-design` / `theme-factory` — UI デザイン規約統一
  - `mcp-builder` — MCP server 開発案件
- ニッチ:
  - `brand-guidelines` — Anthropic 公式ブランド (社外配布物には不要)
  - `doc-coauthoring`, `internal-comms` — 文書ワークフロー特化
  - `slack-gif-creator`, `algorithmic-art`, `canvas-design` — クリエイティブ用途

ユーザーが「skill-creator だけ欲しい」と希望する場合、**個別 Skill コピー方式** を案内 (Step 5 末尾参照)。

回答内容を `.steering/_setup-progress.md` の Phase 2 セクションにメモ。

### Step 4: marketplace の追加

ユーザーに以下を **手動実行** するよう依頼:

```
/plugin marketplace add anthropics/skills
```

> **理由**: `/plugin` は Claude Code 組み込みのスラッシュコマンドであり、本コマンドからは直接呼び出せない。ユーザーに依頼するのが正規のパターン。

実行後、確認:

```bash
ls -la ~/.claude/plugins/marketplaces/ 2>/dev/null
```

`anthropics/skills` 相当のディレクトリが現れていれば成功。

### Step 5: 選択された plugin の install

Step 3 で選ばれた plugin について、ユーザーに 1 つずつ実行依頼:

```
/plugin install document-skills@anthropic-agent-skills      ← 選んだ場合のみ
/plugin install claude-api@anthropic-agent-skills           ← 選んだ場合のみ
/plugin install example-skills@anthropic-agent-skills       ← 選んだ場合のみ
```

実行ごとに以下で確認:

```bash
ls -la ~/.claude/plugins/ 2>/dev/null
```

#### example-skills のサブセット導入 (オプション)

「skill-creator だけ欲しい」など個別 Skill 導入を希望する場合の代替手順:

```bash
git clone --depth=1 https://github.com/anthropics/skills.git /tmp/anthropics-skills-src
mkdir -p .claude/skills/
cp -r /tmp/anthropics-skills-src/skills/skill-creator .claude/skills/
```

この方式は plugin 形式ではなく **プロジェクトローカル Skill** として扱う。`.gitignore` の方針はユーザーに確認。

### Step 6: install 結果の集約確認

```bash
ls -la ~/.claude/plugins/ 2>/dev/null
ls -la .claude/skills/ 2>/dev/null
```

または Claude Code 内で:

```
/plugin list
```

期待した Skill がすべて見えていることを確認。漏れがあれば Step 4-5 を再実行。

### Step 7: docs/external-skills.md の生成

`docs/external-skills.md` を以下の形式で作成 (実際の導入結果に合わせて表の行を組み立てる)。

> **このファイルは公式 Skill とライセンスの一覧の単一の真実源 (Single Source of Truth) である。**
> Phase 3 (`/setup-claude-md`) で agent-shared.md からリンク参照され、Phase 4 (`/setup-skills`)
> で重複回避に使われる。列の値とフォーマット (Proprietary / Apache 2.0) は厳密に維持すること。

```markdown
# 外部 (公式) Skill 一覧

このファイルは anthropics/skills marketplace から導入された公式 Skill のメタデータを記録する。
Phase 3 (`/setup-claude-md`) と Phase 4 (`/setup-skills`) はこのファイルを動的に読んで、
CLAUDE.md への反映と自前 Skill との重複回避を実施する。

## 導入済み plugin

| plugin | Skill | License | 主用途 |
|---|---|---|---|
| document-skills | pdf | Proprietary | PDF 操作 |
| document-skills | docx | Proprietary | Word 文書 |
| document-skills | xlsx | Proprietary | Excel |
| document-skills | pptx | Proprietary | PowerPoint |
| claude-api | claude-api | Apache 2.0 | Anthropic SDK 実装ガイド |
| example-skills | skill-creator | Apache 2.0 | Skill 自動生成 |
| example-skills | webapp-testing | Apache 2.0 | Playwright E2E テスト |
| example-skills | web-artifacts-builder | Apache 2.0 | React Artifact 生成 |
| ... | ... | ... | ... |

## 自前 Skill 作成時のガイドライン (Phase 4 で参照)

Phase 4 で自前 Skill を作るときは、上表が既にカバーする領域を **作らない**:

- PDF 操作 → `pdf` を使う
- Office 文書生成 → `docx` / `xlsx` / `pptx` を使う
- Skill 自動生成 → `skill-creator` を活用
- E2E テスト (web) → `webapp-testing` を使う
- React Artifact → `web-artifacts-builder` を使う
- Claude API 実装 → `claude-api` を参照

該当する公式 Skill が無い領域 (プロジェクト固有のドメインルール、フレームワーク固有のパターン等) のみ自前で作る。

## proprietary Skill のライセンス上の注意

document-skills (pdf / docx / xlsx / pptx) は **Proprietary** ライセンスであり、以下のライセンス上の一般的注意を守ること:

- Anthropic Services 内での利用に限定 (Anthropic Consumer/Commercial Terms に従属)
- Services から抽出して外部保存することは禁止
- 自動キャッシュ以外の複製、派生著作物の作成は禁止
```

### Step 8: ユーザー承認 + Grill me

**ユーザー承認**: 「`docs/external-skills.md` の内容で問題ないですか? proprietary plugin の制約は把握できていますか?」

**Grill me ステップ**:

> external-skills.md を批判的にレビューします:
> - 全ての導入 plugin が表に列挙されているか? (`ls ~/.claude/plugins/` の結果と一致するか)
> - License カラムが正確か? (proprietary は document-skills のみ)
> - 自前 Skill 作成ガイドラインが具体的か? (「適切に」のような曖昧語を使っていないか)
> - Phase 4 で参照されることが明記されているか?
> - 個別 Skill 単位 (`.claude/skills/skill-creator/`) でローカル導入したものも表に入っているか?

問題があれば修正。

### Step 9: 進捗ファイル更新

`.steering/_setup-progress.md` の Phase 2 を完了マーク:

```markdown
- [x] **Phase 2: /setup-marketplace** — anthropics/skills 公式 plugin の導入
  - 完了日時: [YYYY-MM-DD HH:MM]
  - 導入 plugin:
    - document-skills (4 Skill, Proprietary, ライセンス同意済み YYYY-MM-DD)
    - claude-api (1 Skill, Apache 2.0)
    - example-skills (13 Skill, Apache 2.0)
  - 個別 Skill コピー: なし / [skill 名]
  - 作成ファイル:
    - docs/external-skills.md
  - ライセンス同意ログ: あり (proprietary 利用のライセンス制約を理解)
```

「構築物の相互参照マップ」セクションに公式 Skill リストを追記:

```markdown
### 公式 Skill リスト (anthropics/skills 経由)
- pdf, docx, xlsx, pptx (proprietary)
- claude-api
- skill-creator
- webapp-testing
- ...
```

### Step 10: 完了通知

```
Phase 2 完了です。

導入 plugin: [N] 個 (Proprietary [N], Apache 2.0 [N])
個別 Skill 数: [合計 N]
作成ファイル: docs/external-skills.md

次のステップ:
1. `/clear` でリセット
2. `/model opus` に切り替え (Phase 3 は CLAUDE.md 設計のため Opus 推奨)
3. `/setup-claude-md` を実行 — external-skills.md を動的に読み込んで CLAUDE.md に反映します
```

## 完了条件

- [ ] gh CLI と GitHub 認証が確認済み
- [ ] anthropics/skills marketplace が `/plugin marketplace add` で追加されている
- [ ] 選択された plugin がすべて install 済み (`/plugin list` で確認)
- [ ] proprietary plugin を導入した場合、ライセンス同意ログが残されている
- [ ] `docs/external-skills.md` が生成され、plugin / Skill / License / 主用途 の列が含まれている
- [ ] Grill me ステップ実施済み
- [ ] Phase 2 が完了マーク済み
- [ ] 「構築物の相互参照マップ」に公式 Skill 一覧が追記されている

## アンチパターン

- ❌ proprietary plugin を license 警告なしに導入する
- ❌ external-skills.md に License カラムを書かない (Phase 4 の重複回避・ライセンス判断ができない)
- ❌ 必要のない plugin を「念のため」全部 install する (保守コスト増、外部依存増、disk 使用量増)
- ❌ Step 4-5 をユーザーに実行依頼せず、メインエージェントが Bash で plugin install を試みる (`/plugin` は組み込み slash command なので不可)
- ❌ marketplace.json を毎回 fetch する (gh API rate limit 消費。`/tmp` などにキャッシュ)

## メモ: 将来的な拡張

- marketplace.json の version pinning (どのコミットの Skill 集合を使ったかを `.steering/_setup-progress.md` に記録、再現性確保)
- 個別 Skill 単位の install (現在 example-skills は一括 install のみ)
- 自動更新フロー (`/setup-marketplace --update` で marketplace.json を再 fetch して差分を提示)

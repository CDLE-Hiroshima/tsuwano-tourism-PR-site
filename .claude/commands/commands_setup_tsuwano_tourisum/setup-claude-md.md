---
description: >
  CLAUDE.md / docs/agent-shared.md の 2 ファイル体制と、作業記録ディレクトリ .steering/
  の構造を構築する。CLAUDE.md は Claude Code 固有指示 (150 行以下のポインタ型)、
  docs/agent-shared.md は Claude Code 用の共通コンテキスト
  (アーキテクチャ / 規約 / テスト / git ワークフロー) とリンクハブ。CLAUDE.md は
  @docs/agent-shared.md と @docs/external-skills.md を参照することで重複を排除する。
  .steering/_template/ にタスク用 5 ファイルテンプレートを配置する。/setup-marketplace の完了後に実行する。
allowed-tools: Read, Write, Glob, Bash(mkdir *), Bash(wc *), Bash(cat *), Bash(grep *)
---

# /setup-claude-md — CLAUDE.md / docs/agent-shared.md と作業記録構造の構築

> Phase 3 of 8. Let's think step by step.

## 環境チェックブロック

### Check 1: 進捗ファイル

```bash
cat .steering/_setup-progress.md
```

Phase 1 (`/setup-docs`) と Phase 2 (`/setup-marketplace`) の完了マークを確認:

- Phase 1 完了 → docs/ 配下 5 ファイルが揃っている前提
- Phase 2 完了 (または `[~] skipped`) → `docs/external-skills.md` が存在する前提 (skip 時は無くてもよいが、後段の動的読み込みで「未導入」として扱う)

されていない場合は中断し、対応する setup-* コマンドの再実行を依頼。

### Check 2: docs/ の存在 と external-skills.md の契約確定

```bash
ls docs/
ls docs/external-skills.md 2>/dev/null
```

5 つのドキュメント (functional-design / architecture / repository-structure / development-guidelines / glossary) が揃っていることを確認。

`docs/external-skills.md` (Phase 2 の成果物) の扱い:

- **存在する場合**: 中身を Read で読み、agent-shared.md の「公式 Skill」節からリンク参照する
- **存在しない場合 (Phase 2 を skip した場合)**: 空テンプレを **必ず生成** する。これによって agent-shared.md からの参照が破綻しない。下記内容で作成:

  ```markdown
  # 外部 (公式) Skill 一覧

  > このプロジェクトは Phase 2 (`/setup-marketplace`) を skip しました。
  > 公式 Skill は導入されていません。

  ## 導入済み plugin

  | plugin | Skill | License | 主用途 |
  |---|---|---|---|
  | (なし) | - | - | - |

  ## 自前 Skill 作成時のガイドライン

  公式 Skill は未導入のため、Phase 4 ですべての必要 Skill を自前で作成する。
  ```

> **設計原則**: `docs/external-skills.md` は **公式 Skill 一覧・ライセンス表** として、agent-shared.md / 各 Skill が参照する。空でも存在することを保証する。

### Check 3: コンテキスト予算

`/context` で 30% 以下を確認。

### Check 4: モデル

`/model` で Opus を確認。Sonnet でも実行可能だが、CLAUDE.md / agent-shared.md の責務分割は設計判断を伴うため Opus 推奨。

## 設計方針: 2 ファイル分担

このコマンドが生成する 2 ファイルの責務を明確に分ける:

| ファイル | 読み手 | 中身 | 行数目安 |
|---|---|---|---|
| `CLAUDE.md` | Claude Code | Claude Code 固有 (Plan mode / Skill / subagent 委譲 / モデル選択 / context 管理 / 禁止事項)。冒頭で `@docs/agent-shared.md` を参照。 | ≤ 150 行 |
| `docs/agent-shared.md` | Claude Code | プロジェクト共通 (概要 / アーキテクチャ要約 / 主要ディレクトリ / 規約サマリ / テスト方針 / git ワークフロー基本)。docs/ の各詳細ドキュメントへのリンクハブ。 | ≤ 200 行 |

**重複は agent-shared.md 経由でしか起こさない**。CLAUDE.md は固有事項に集中する。

外部 Skill 情報 (`docs/external-skills.md`) は agent-shared.md から参照リンク 1 行で繋ぎ、CLAUDE.md には直書きしない (Phase 2 で内容が変動するため)。

## 実行フロー

### Step 1: 既存 docs の精査

`docs/` 配下のファイルを Read で読む:

```
docs/functional-design.md
docs/architecture.md
docs/repository-structure.md
docs/development-guidelines.md
docs/glossary.md
docs/external-skills.md   ← Phase 2 で生成 (skip 時は存在しない)
```

これらの内容を頭に入れた状態で 2 ファイルを構築する。`agent-shared.md` は **これらへのリンクハブ** として、`CLAUDE.md` は **Claude Code 固有の動作プロトコル** として機能する。

`docs/external-skills.md` が存在する場合は、後段で agent-shared.md にその参照リンクを 1 行追加する。

### Step 2: CLAUDE.md のドラフト作成

**最重要ルール**: 150 行以下に収める。これを超えそうになったら情報を `docs/agent-shared.md` または `docs/` 配下に退避する。

以下のテンプレートをベースにプロジェクト固有の内容で埋める:

```markdown
# [プロジェクト名] — CLAUDE.md

## このファイルについて

このファイルは Claude Code がセッション開始時に自動で読み込む指示書です。
**Claude Code 固有の動作プロトコル** に集中して書きます。

- プロジェクト共通の情報 (概要 / アーキテクチャ / 規約 / テスト / git ワークフロー) は
  `@docs/agent-shared.md` を参照してください。

このファイルの目的は **Claude が探索を始める起点を示すこと** であり、
プロジェクトの全情報を含むことではありません。

## 共通コンテキスト

@docs/agent-shared.md

(プロジェクト概要、アーキテクチャ、規約、テスト方針、git ワークフロー基本、
公式 Skill 一覧 (`@docs/external-skills.md`) はここから参照されます)

## セッション開始時の行動規範

タスク/セッション開始時、メインエージェントは以下を **必ずこの順** で実行してください。
全部自分でやろうとしないこと。

1. **コマンド選定**: `.claude/commands/` を glob で確認し、今回の作業に該当する
   スラッシュコマンド（`/start-task`, `/add-feature`, `/fix-bug`, `/refactor`
   等）があれば **必ずそれを起動する**。該当コマンドを使わずに作業を始めない。
2. **Skill 参照**: 実装・レビュー・テスト等、該当する Skill があれば
   `.claude/skills/[name]/SKILL.md` を Read で参照してから手を動かす。
   公式 Skill (`@docs/external-skills.md` 参照) で済む領域は自前 Skill を作らず公式を使う。
3. **サブエージェントへの委譲**: 以下はメインエージェントではなくサブエージェントで実行する。
   - 複数ファイル横断の探索・読み込み → `file-finder`
   - 影響範囲調査 → `impact-analyzer`
   - コードレビュー → `code-reviewer` (Phase 5 以降)
   - テスト実行 → `test-runner`
   - セキュリティ確認 → `security-checker`
4. **破壊と構築の判断**: 設計判断・アーキテクチャ選択・難しいバグなど高難度タスクでは
   `/reimagine` をプラン段階で起動し、初回案を一旦破棄してゼロから再生成した案と比較する。
5. **Skill の品質検証**: エージェントが Skill の指示通りに動かない、同じ Skill を使った
   タスクで繰り返し問題が起きる、Skill の記述が古くなった疑いがある場合は
   `empirical-prompt-tuning` Skill を起動し、新規 subagent で客観的に検証・改善する。

このルールに従うことで、メインエージェントのコンテキストを節約し、各領域の専門性を活かせます。

## 作業記録の運用ルール（重要）

**すべての実装作業は `.steering/` 配下に記録を残してください。**

### ディレクトリ命名規則

\`\`\`
.steering/[YYYYMMDD]-[task-name]/
\`\`\`

例:
- `.steering/20260407-add-user-authentication/`
- `.steering/20260408-fix-login-bug/`

### 各タスクで作成するファイル

| ファイル | 必須/任意 | 内容 |
|---|---|---|
| `requirement.md` | 必須 | 今回の作業の背景、ゴール、受け入れ条件 |
| `design.md` | 必須 | 実装アプローチ、変更対象、テスト戦略 |
| `tasklist.md` | 必須 | チェックボックス形式の具体的タスクリスト |
| `blockers.md` | 任意 | 発生したブロッカーと対処方法 |
| `decisions.md` | 任意 | 重要な設計判断と根拠 |

### 作成タイミング

1. **作業開始時**: `/start-task` コマンドで自動作成（Phase 6 で構築）
2. **実装中**: `tasklist.md` のチェックボックスを更新、必要に応じて追記
3. **作業完了時**: `/finish-task` コマンドで最終化（Phase 6 で構築）

### テンプレート

`.steering/_template/` 配下に各ファイルのテンプレートが配置されています。
新規タスク時はそこからコピーして使ってください。

## モデル選択ルール

| タスク種別 | モデル | 理由 |
|---|---|---|
| Plan Mode、設計判断 | Opus | 推論深度が必要 |
| 実装、テスト、リファクタ | Sonnet | バランス最良 |
| リネーム、フォーマット | Haiku | 速度優先 |
| 大規模変更（10ファイル以上） | Sonnet[1m] | 1M コンテキスト |

## コンテキスト管理ルール

### 黄金律

1. **50% ルール**: 使用率が 50% を超えたら次の区切りで `/smart-compact`
2. **タスク切り替え時は `/clear`**: 文脈が変わるなら `/compact` ではなく `/clear`
3. **`/context` を見る習慣**: セッション開始、実装前、compact 前

### Plan → Execute の徹底

複雑なタスクでは省略しない:
1. `Shift+Tab` 2 回で Plan Mode
2. `/model opus` に切り替え
3. 計画を立てさせる（コードはまだ書かせない）
4. レビューして承認
5. `/model sonnet` で実装

## 禁止事項

- 既存のテストを無断で削除しない
- ドキュメント化されていない設計判断を勝手に変更しない
- `.steering/` への記録を省略しない
- 50% を超えてもセッションを続ける
- 曖昧な指示に対して推測で実装する（質問する）

## 利用可能な独自コマンド

このプロジェクトには以下のスラッシュコマンドが定義されています:

- `/start-task` — 新規タスクの開始
- `/add-feature` — 新機能追加ワークフロー
- `/fix-bug` — バグ修正ワークフロー
- `/refactor` — リファクタリングワークフロー
- `/reimagine` — プラン段階での「破壊と構築」（初回案を破棄し再生成して比較）
- `/review-changes` — 変更レビュー
- `/smart-compact` — 情報保持型 compact
- `/finish-task` — タスク完了処理

詳細は `.claude/commands/` を参照。

## 利用可能なサブエージェント

- 情報収集: `file-finder`, `dependency-checker`, `impact-analyzer`
- レビュー: `code-reviewer`, `test-analyzer`, `security-checker`
- 実行: `test-runner`, `build-executor`, `log-analyzer`

詳細は `.claude/agents/` を参照。
```

### Step 2.5: docs/agent-shared.md のドラフト作成

CLAUDE.md が `@docs/agent-shared.md` で参照する **共通コンテキスト** を作成する。

**最重要ルール**: 200 行以下。詳細は `docs/` 配下の各ファイルへリンクハブする。

```markdown
# プロジェクト共通コンテキスト

このファイルは Claude Code が読む共通コンテキスト / リンクハブです。
プロジェクトの **不変的な事実** だけを記載してください。Claude Code 固有の動作プロトコルは
書きません (CLAUDE.md に書きます)。

## プロジェクト概要

- **名称**: [プロジェクト名]
- **目的**: [1 行で]
- **主要技術**: [言語、フレームワーク]
- **チーム規模**: [個人 / 小規模 / 中規模]
- **重視する品質特性**: [パフォーマンス / セキュリティ / 保守性 / etc.]

## アーキテクチャ要約

3-5 行で書く。詳細は `docs/architecture.md` を参照。

## 主要ディレクトリ

- `src/` — [何が入っているか]
- `tests/` — [何が入っているか]
- `docs/` — 永続ドキュメント (5 ファイル + external-skills.md)
- `.steering/` — タスク作業記録
- `.claude/` — Claude Code 用 (skills / agents / commands / hooks)

詳細は `docs/repository-structure.md` を参照。

## コーディング規約サマリ

- 命名規則: [例: snake_case for Python, camelCase for TS]
- 1 関数 1 責任、長さ目安 [N] 行
- エラーハンドリング: [方針 1 行]

詳細は `docs/development-guidelines.md` を参照。

## テスト方針サマリ

- 単体テスト必須: [対象]
- 統合テスト: [対象]
- E2E: [使うか / 使わないか]

詳細は `docs/development-guidelines.md` (テスト節) を参照。

## git ワークフロー基本

- ブランチ命名: [例: feature/* / fix/* / refactor/*]
- コミット: 1 コミット 1 論理単位
- PR タイトル: [例: Conventional Commits]

## 詳細ドキュメント (リンクハブ)

| ファイル | いつ読むか |
|---|---|
| `docs/functional-design.md` | 機能の意図や要件を確認したい時 |
| `docs/architecture.md` | アーキテクチャや技術選定を確認したい時 |
| `docs/repository-structure.md` | ファイル配置や命名規則を確認したい時 |
| `docs/development-guidelines.md` | コーディング規約やレビュー基準を確認したい時 |
| `docs/glossary.md` | 用語の意味を確認したい時 |
| `docs/external-skills.md` | 公式 (anthropics/skills) 由来の Skill 一覧、ライセンス |

## 公式 Skill (Phase 2 で導入したもの)

@docs/external-skills.md
```

### Step 3: 行数チェック

```bash
wc -l CLAUDE.md docs/agent-shared.md
```

それぞれの目安:

| ファイル | 上限 | 退避先 |
|---|---|---|
| CLAUDE.md | 150 行 | モデル選択ルール → `docs/development-guidelines.md`、コンテキスト管理 → `docs/claude-code-operations.md` 新設 |
| docs/agent-shared.md | 200 行 | 詳細はもともと docs/ 各ファイルにあるはずなので、リンクハブ化を徹底 |

100-150 行 (CLAUDE.md) / 100-200 行 (agent-shared.md) を目標にする。

### Step 4: ユーザー承認 + Grill me

**ユーザー承認**: 「この 2 ファイル (CLAUDE.md / docs/agent-shared.md) で問題ないですか? 行数は CLAUDE.md=[N] / agent-shared.md=[K] です」

**Grill me ステップ (2 ファイル横断)**:

> **CLAUDE.md / docs/agent-shared.md を批判的にレビューします:**
>
> 重複検出:
> - agent-shared.md と docs/ 配下の各詳細ドキュメントに重複がないか? (agent-shared.md はリンクハブに徹する)
> - external-skills.md の中身を CLAUDE.md に直書きしていないか? (リンク 1 行で済ます)
>
> 責務分離:
> - CLAUDE.md は Claude Code 固有事項のみか?
> - agent-shared.md は共通事項 (概要 / アーキテクチャ / 規約 / テスト / git) のみか? (Claude Code 固有の動作プロトコルが混入していないか)
>
> 行数:
> - CLAUDE.md ≤ 150 行か?
> - agent-shared.md ≤ 200 行か?
>
> 参照検証:
> - CLAUDE.md に `@docs/agent-shared.md` 参照が存在するか?
> - `grep -c '@docs/agent-shared.md' CLAUDE.md` で 1 以上か?
>
> 内容品質:
> - 「禁止事項」が具体的か? (抽象的な「丁寧に」とか書いていないか)
> - 毎ターン消費されることを意識した記述になっているか?
> - サブエージェント / Skill / Hook の名前が後段の Phase で実際に作られるものと一致しているか?

問題があれば修正。両ファイルで同じ問題が出る場合は agent-shared.md の構造から見直す。

### Step 5: .steering/ ディレクトリの構築

#### 5.1 .steering/README.md の作成

```markdown
# .steering/

このディレクトリは Claude Code との作業記録を保管します。

## 構造

\`\`\`
.steering/
├── README.md              # このファイル
├── _setup-progress.md     # 環境構築の進捗記録
├── _template/             # 新規タスク用テンプレート
│   ├── requirement.md
│   ├── design.md
│   ├── tasklist.md
│   ├── blockers.md
│   └── decisions.md
└── [YYYYMMDD]-[task-name]/   # 各タスクの作業記録
    ├── requirement.md
    ├── design.md
    ├── tasklist.md
    ├── blockers.md (任意)
    └── decisions.md (任意)
\`\`\`

## 新規タスクの開始

`/start-task` コマンドを使うと、自動的にディレクトリとテンプレートが配置されます。

## ファイルの役割

- **requirement.md** — 何をするか（背景、ゴール、受け入れ条件）
- **design.md** — どうやるか（アプローチ、変更対象、テスト戦略）
- **tasklist.md** — 具体的なタスクのチェックリスト
- **blockers.md** — 詰まったポイントとその解決方法（任意）
- **decisions.md** — 重要な設計判断とその根拠（任意）
```

#### 5.2 _template/ にテンプレートを配置

各ファイルを以下の内容で作成:

**`.steering/_template/requirement.md`**:

```markdown
# [タスク名]

## 背景

なぜこのタスクが必要か。何が問題で、何を解決したいのか。

## ゴール

何を達成すれば「完了」とするのか。

## スコープ

### 含むもの
- ...

### 含まないもの
- ...

## 受け入れ条件

- [ ] 条件 1
- [ ] 条件 2
- [ ] 条件 3

## 関連ドキュメント

- docs/architecture.md の [該当セクション]
- ...
```

**`.steering/_template/design.md`**:

```markdown
# 設計

## 実装アプローチ

採用する方針と、その理由。

## 変更対象

### 修正するファイル
- `path/to/file1.py` — 何を変えるか
- `path/to/file2.py` — 何を変えるか

### 新規作成するファイル
- `path/to/new_file.py` — 役割

### 削除するファイル
- ...

## 影響範囲

この変更が影響する箇所と、その対処。

## 既存パターンとの整合性

既存のコードベースのどのパターンに従うか。

## テスト戦略

- 単体テスト: 何をテストするか
- 統合テスト: 何をテストするか
- E2E テスト: 必要か

## ロールバック計画

変更が問題を起こした場合の戻し方。
```

**`.steering/_template/tasklist.md`**:

```markdown
# タスクリスト

## 準備
- [ ] 関連する docs を読む
- [ ] 影響範囲を file-finder で調査

## 実装
- [ ] タスク 1
- [ ] タスク 2
- [ ] タスク 3

## テスト
- [ ] 単体テストを追加
- [ ] 統合テストを追加（必要なら）
- [ ] テストが通ることを確認

## レビュー
- [ ] code-reviewer によるレビュー
- [ ] HIGH 指摘への対応

## ドキュメント
- [ ] docs の更新（必要なら）
- [ ] glossary への用語追加（必要なら）

## 完了処理
- [ ] design.md の最終化
- [ ] decisions.md の作成（重要な判断があった場合）
- [ ] git commit
```

**`.steering/_template/blockers.md`**:

```markdown
# ブロッカー記録

## ブロッカー 1: [タイトル]

- **発生日時**:
- **症状**:
- **試したこと**:
  1. ...
  2. ...
- **原因**:
- **解決方法**:
- **教訓**: 次回同じ状況に遭遇したらどうすべきか
```

**`.steering/_template/decisions.md`**:

```markdown
# 重要な設計判断

## 判断 1: [タイトル]

- **判断日時**:
- **背景**: なぜこの判断が必要だったか
- **選択肢**:
  - A: [説明]
  - B: [説明]
  - C: [説明]
- **採用**: B
- **理由**:
- **トレードオフ**: 何を諦めたか
- **影響範囲**: この判断がどこに影響するか
- **見直しタイミング**: どんな状況になったら再検討すべきか
```

### Step 6: .gitignore の確認

ユーザーに `.steering/` を git で追跡するか尋ねる:

- **追跡する場合**: 何もしない（共有可能なナレッジとして残る）
- **ローカルのみ**: `.gitignore` に追加

推奨は **追跡する**。チームで共有できるし、過去の作業履歴が貴重なナレッジになる。

### Step 7: 進捗ファイルの更新

`.steering/_setup-progress.md` の Phase 3 を完了マーク:

```markdown
- [x] **Phase 3: /setup-claude-md** — CLAUDE.md / docs/agent-shared.md / .steering
  - 完了日時: [YYYY-MM-DD HH:MM]
  - 作成ファイル:
    - CLAUDE.md ([N] 行 / 上限 150 行)
    - docs/agent-shared.md ([K] 行 / 上限 200 行)
    - .steering/README.md
    - .steering/_template/ × 5 ファイル
  - 参照検証: `grep '@docs/agent-shared.md' CLAUDE.md` で検出
  - .gitignore 更新: あり / なし
```

### Step 8: 完了通知

```
Phase 3 完了です。

作成したファイル:
- CLAUDE.md ([N] 行 / 上限 150 行)
- docs/agent-shared.md ([K] 行 / 上限 200 行)
- .steering/README.md
- .steering/_template/requirement.md
- .steering/_template/design.md
- .steering/_template/tasklist.md
- .steering/_template/blockers.md
- .steering/_template/decisions.md

2 ファイルの責務分離:
- CLAUDE.md → Claude Code 固有 (Plan mode / Skill / .steering ワークフロー / モデル選択)
- docs/agent-shared.md → 共通 (概要 / 規約 / テスト / git ワークフロー)

次のステップ:
1. `/clear` でセッションをリセット
2. `/model sonnet` に切り替え（Phase 4 は Sonnet で十分）
3. `/setup-skills` を実行
```

## 完了条件

- [ ] CLAUDE.md が 150 行以下で作成されている
- [ ] docs/agent-shared.md が 200 行以下で作成されている
- [ ] CLAUDE.md がポインタ型になっている (詳細は agent-shared.md / docs/ にある)
- [ ] CLAUDE.md から `@docs/agent-shared.md` が参照されている (`grep -c '@docs/agent-shared.md' CLAUDE.md` で 1 以上)
- [ ] CLAUDE.md の責務が分離されている (Claude Code 固有事項のみ)
- [ ] docs/external-skills.md が存在する場合、agent-shared.md からリンク参照されている
- [ ] Grill me ステップ (2 ファイル横断) を実施済み
- [ ] .steering/README.md が作成されている
- [ ] .steering/_template/ に 5 つのテンプレートが配置されている
- [ ] .gitignore の方針がユーザーと合意されている
- [ ] Phase 3 が完了マークされている

## アンチパターン

- ❌ CLAUDE.md にプロジェクトの全情報を詰め込む (agent-shared.md / docs/ 配下に退避すべき)
- ❌ docs/ の内容を CLAUDE.md にコピーする
- ❌ 150 / 200 行の上限を超える
- ❌ 共通事項を CLAUDE.md に直書きする (agent-shared.md に集約せよ)
- ❌ agent-shared.md に Claude Code 固有の動作プロトコル (Plan mode / Skill / .steering ワークフロー) を書く
- ❌ external-skills.md の内容を CLAUDE.md に直書き (リンク参照のみ)
- ❌ テンプレートを省略する
- ❌ Grill me を省略する

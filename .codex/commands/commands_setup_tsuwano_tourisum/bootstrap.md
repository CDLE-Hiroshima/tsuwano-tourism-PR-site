# bootstrap.md

## 目的

`Codex` でこの教材を始めるときの入口。`project-brief.md` を SSOT として完成させ、
`README.md` の brief セクションと同期し、以降の作業で使う最小構成を作る。

## Codex への渡し方

```text
AGENTS.md と .codex/commands/commands_setup_tsuwano_tourisum/bootstrap.md を読んで実行して。
```

## 事前確認

1. カレントディレクトリ直下に `project-brief.md` があるか確認する
2. `README.md` があるか確認する
3. 既存の `web/` や `.steering/_setup-progress.md` があれば、上書きではなく現状確認から入る

## 実行フロー

1. `project-brief.md` を読む
2. `【 】` や空欄が残っていれば、1 問ずつ質問して埋める
3. すでに埋まっている文言は勝手に言い換えない
4. `README.md` の `<!-- brief:* -->` セクションを brief に合わせて更新する
5. 次のディレクトリが無ければ作る
   - `.codex/commands`
   - `.codex/agents`
   - `.codex/references`
   - `.steering/_template`
   - `docs`
6. `.steering/_setup-progress.md` が無ければ作る
7. 参加者の今日のゴールと公開方針を brief か progress に記録する
8. 次にやるべきファイルとして `build-site-mvp.md` を案内する

## `.steering/_setup-progress.md` の最低限テンプレート

```md
# Codex Setup Progress

## Project
- Name: Tsuwano Tourism PR Site
- SSOT: project-brief.md
- Goal today: <user goal>
- Deployment target: <user choice>

## Phases
- [x] Phase 0: bootstrap
- [ ] MVP: build-site-mvp
- [ ] Phase 1: setup-docs
- [ ] Phase 2: setup-marketplace
- [ ] Phase 3: setup-codex-md
- [ ] Phase 4: setup-skills
- [ ] Phase 5: setup-agents
- [ ] Phase 6: setup-commands
- [ ] Phase 7: setup-hooks
- [ ] Phase 8: verify-setup
```

## 注意

- ⑩「避けたい表現・扱い」は最優先ルールとして扱う
- 埋める内容が曖昧なら、推測せず質問する
- README では brief マーカーを消さない

## 完了条件

- `project-brief.md` の空欄が無い
- `README.md` の brief セクションが同期されている
- `.steering/_setup-progress.md` がある
- `Codex` 用ディレクトリの土台がある

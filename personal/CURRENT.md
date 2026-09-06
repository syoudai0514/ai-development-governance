# Personal AI Development — CURRENT

Status: CANONICAL CURRENT
Last updated: 2026-09-07

このファイルだけがPersonalガバナンスのCURRENT状態を表す。

## Current phase

Wave 1 — Foundation / Accident Prevention

## Accepted direction

- `ai-development-governance/personal/` をPersonal開発標準の将来唯一のcanonical homeとする。
- `syoudai0514/.github/agent-rules/` はlegacy Pilot / migration sourceへ降格する。
- Active personal repositoryは原則PR-onlyとし、branch/ruleset protectionを目標状態とする。
- CURRENT GitHub fresh readを作業開始時・重要判断前に必須とする。
- `START-HERE` と `CURRENT` は各authority scopeで1つだけにする。
- exact-head review、Design Gate、CI validation-only、canonical syncをWave 1の標準統制とする。

## Current implementation status

- Canonical Personal entrypoint: this PRで導入中
- Single CURRENT: this PRで導入中
- Wave 1 canonical standard: this PRで導入中
- Branch protection / ruleset: 未適用。GitHub設定変更が必要
- Active repositoriesへの配布: 未着手
- Legacy `.github` rulesの移行完了: 未完了
- CIによるcanonical sync / governance validation: 未着手

## Required next actions

1. このWave 1 foundation PRを独立レビューする。
2. GO後にmainへmergeする。
3. `ai-development-governance/main` 自体をbranch/ruleset protectionする。
4. active personal repoを優先順位順にPR-onlyへ移行する。
5. Wave 1標準をテンプレート/CIへ機械化する。
6. legacy `.github` rulesの重複を整理し、authority二重化を解消する。

## Prohibited interpretation

- `_inbox`をCURRENT/requirement/designとして使わない。
- 過去handoffやPR本文をCURRENTとして使わない。
- CI GREENだけでproduction readyと断定しない。
- 未確認のbranch/HEAD/deploy状態を推測しない。

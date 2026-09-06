# Personal AI Development — START HERE

Status: CANONICAL

このファイルは、個人開発ガバナンスを読むときの唯一の開始地点です。

## Authority order

1. この `personal/START-HERE.md`
2. `personal/CURRENT.md`
3. `personal/standards/` の Accepted / Canonical 文書
4. `personal/archetypes/` の種類別標準
5. 各プロジェクト固有の canonical requirements / design / Accepted ADR
6. CURRENT GitHub source / schema / workflow / deployment configuration
7. Issue / PR / handoff / review comment は履歴・作業管理
8. `personal/_inbox/` は非正本

競合した場合、都合のよい資料を選ばない。上位authorityを優先し、同じauthority階層で解決不能なら実装を止め、競合そのものを課題化する。

## Mandatory fresh-read sequence

作業開始時と重要判断前に、過去チャットやhandoffではなくCURRENTをfresh確認する。

- repository identity
- default / base / working / deployment branch
- actual HEAD
- PR status and actual head
- latest relevant CI/workflow
- changed files
- CURRENT source/schema/migrations
- canonical requirements/design/Accepted ADR
- production/deployment state when relevant

古いHEAD、過去CI、PR本文、handoff、以前のCURRENT記述をCURRENTとして再利用しない。

## Canonical current state

現在状態は `personal/CURRENT.md` だけを正とする。別のCURRENT文書を作らない。

## Core Wave 1 standards

- `personal/standards/WAVE1-FOUNDATION.md`

## Non-canonical area

`personal/_inbox/` は観測事実・候補・調査メモの保存場所であり、実装判断の根拠にしない。正本へ昇格した内容は `_inbox` から整理・削除する。

## Legacy source

`syoudai0514/.github/agent-rules/` は過去Pilotの移行元・履歴であり、Personal標準の将来正本ではない。移行完了後は新規判断をそちらへ追加しない。

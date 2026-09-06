# Long-running PR Continuation — IDEA / NOT CANONICAL

## Problem

1つのPRが1ターンで終わらない場合、過去チャットやhandoffに依存すると、古いHEAD・古いCI・古い進捗率をCURRENTと誤認しやすい。長時間化するほどcontext saturation、anchoring、scope driftが起きる。

## Candidate protocol

毎ターン、CURRENT PRから即再開する。

### Mandatory start

- actual PR status
- actual branch
- actual HEAD
- latest CI/workflow
- changed files
- unresolved review findings
- canonical requirements/design/Accepted ADR
- 前ターン以降のHEAD/仕様変更

をfresh readする。

### Work rule

- CURRENT PRの既存scope内で、1ターンに安全かつ正確に完了できる最大量を実装する。
- 実装 → test → document同期 → commit → push → CURRENT再確認まで進める。
- 単なる実装漏れ、test failure、CI failure、既存scope内bugでは止まらない。
- 仕様変更が必要なら勝手に変えずMISSING/blockerとして残す。
- 過去handoff、PR本文、過去HEAD、過去CIはCURRENT扱いしない。

### Progress rule

acceptance matrixを母数にして算出する。

- MATCH = 完全適合
- PARTIAL = 一部適合だが未完
- MISSING = 未実装/要求不適合

PARTIALをMATCHとして数えない。推測で進捗率を盛らない。

### Turn-end fixed output

```text
進捗率: XX%
MATCH-PARTIAL-MISSING: XX / XX / XX
Gate A-D進捗: A=XX / B=XX / C=XX / D=XX
CURRENT head: <actual SHA>
今ターン完了:
- ...
残り主要項目:
- ...
残り見込みターン数: X〜Y
次の1項目: ...
```

終了直前にもCURRENT GitHubを再確認する。

## Candidate improvement

PRごとに再計算可能なprogress ledgerを置く案。

```yaml
matrix:
  total: 34
  match: 21
  partial: 8
  missing: 5
last_verified_head: abc123
next:
  - screen-22
```

ただしledgerは正本にしない。CURRENT source/testから毎回再検証して更新する。

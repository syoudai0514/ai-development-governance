# Family Ops Lessons — NOT CANONICAL

## Observed development style

- 初期: Production/実機で問題発見 -> 小PR/hotfix -> regression追加。
- 後半: canonical requirements -> design/current -> ADR -> implementation -> independent review -> production gateへ進化。
- Q1〜Q112のような大規模conformance closeoutを一つのPRで扱った。

## What worked well

- 実際のLINE入力・iPhone PWA挙動をregressionへ落とした。
- exact-head reviewの考え方を強化した。
- requirements/design/schema/Edge/UIを横断してconformanceを見た。
- idempotency、pending state、conversation context isolation等をテスト対象にした。
- Productionでのみ出る履歴データ差分からProduction-shaped testの必要性が分かった。

## Problems observed

- 要求が会話、Issue、PR、実装、設計へ分散した時期があり、後からcanonical化が必要になった。
- `CURRENT`やhandoff文書が古くなっても強い命令文を持ったまま残る。
- 巨大PR化によりcontext saturationとreview負荷が増えた。
- CI GREENでもProduction履歴データで不具合が出た。
- source/backend conformanceを詰めても、最終mockのUI/interactionがProductionへ十分入っていない問題が残った。
- Vercel Preview deploymentが非main branchごとに発生し、deployment回数を大量消費した。

## Candidate improvements

- canonical requirementsを最初から用意し、会話上の重要判断は即時反映する。
- Work Packageを小さくし、Closeoutは確認中心にする。
- visual/interaction contractを早いGateからacceptanceへ入れる。
- clean DB testだけでなく、過去migrationと履歴データを含むProduction-shaped regressionを持つ。
- real-provider / iPhone / PWA / LINE gateを必要な段階で設ける。
- main-only deployment等の禁止事項はVercel/GitHub設定で機械的に強制する。
- stale CURRENT/handoffを自動検知・history化する。

## Promotion candidates

Personal common候補:

- exact-head review
- Production-shaped regression
- real-provider gate
- visual contract gate
- long-running PR continuation
- deployment budget guard
- stale current detector

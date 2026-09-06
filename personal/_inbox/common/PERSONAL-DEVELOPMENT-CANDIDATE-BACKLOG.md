# Personal Development Candidate Backlog — NOT CANONICAL

個人開発全般へ昇格候補のネタ集。ここはアイデア置き場であり正本ではない。

## Governance / authority

- AI memoryをCURRENT source of truthとして扱わない。
- 作業開始時と重要判断前にCURRENT GitHubをfresh readする。
- `START-HERE`を複数作らない。
- `CURRENT`を名乗る資料を複数作らない。
- Requirements / Design / ADR / implementation / tests / runtime stateのauthorityを種類別に明示する。
- PR本文、Issue、handoff、過去review、過去CIは履歴であってCURRENTではない。
- stale documentには `HISTORICAL — DO NOT USE AS CURRENT AUTHORITY` を付ける。
- AGENTS.mdは巨大knowledge baseにせず、正本へのrouterとして薄く保つ。
- CLAUDE.md / Copilot instructions等は可能なら同一のAGENTS/正本へ誘導し、AIツール別にルールを分岐させすぎない。

## Requirement / design discipline

- ユーザーが「必ず」「絶対」「禁止」「これが要件」と言ったproduct requirementを会話だけに残さない。
- product behavior変更はcanonical requirementsへ反映する。
- architecture/behavior変更はdesign/ADRと同期する。
- 要求変更をAIが勝手に正当化しない。
- 未決定仕様はAIが埋めず、必要ならfail-closedで止める。
- `Design Review -> Canonical Promotion -> Implementation` のGateを候補標準にする。
- implementation PRはAccepted Decision ID / Requirement IDを参照させる案。
- 設計GateをMarkdown上の注意だけにせず、CI等で強制する案。

## Completion / quality

- `CI GREEN == requirement PASS` としない。
- 完了前に `requirement -> design -> implementation -> automated test -> realistic scenario` を照合する。
- PARTIALをMATCHに数えない。
- exact-head reviewを採用し、HEADが動いたら以前のGOを自動継承しない。
- review対象はactual runtime call graphを入口から追う。関連しそうな関数名だけで判断しない。
- source conformanceとproduction parityを別Gateとして扱う案。
- UI/UXは最終mockとの差分を最後にまとめて確認するのではなく、途中からvisual contractをacceptanceへ入れる。

## Testing

- Production-shaped regressionを持つ。
- clean CI DBだけでなく履歴データ/旧migration経由データを再現する。
- iPhone/PWA/LINE/外部provider等、実利用面を必要なGateで検証する。
- test context isolationを保証する。
- out-of-order / retry / idempotency / duplicate deliveryを回帰テストへ入れる。
- 重要な自然言語ルールは可能ならmachine-readable invariantへ変換する。
- snapshot/fingerprint/hashで重要集合・順序・構成を固定する案。

## Work sizing

- 巨大PRを最終closeoutまで肥大化させない。
- canonical requirementは一つでも、implementation work packageは小さく区切る。
- Closeout PRは新規実装大量投入ではなく、確認と残件修正中心にする。
- 長期PRはcontinuation protocolを使い、毎ターンfresh readして再開する。
- 残作業はacceptance matrixで見える化する。

## Pilot / scale

- 大量生成・大量実装の前にVertical Sliceを1本完成させる。
- `Pilot -> QA -> acceptance criteria fix -> Scale` を採用する。
- 画像/音声/コンテンツ大量生成は少数サンプルで品質基準を決めてから量産する。
- 高性能モデルで基準作成 -> 安価/高速モデルで量産、ただし品質Gateで戻す、というrouting候補。

## PoC

- 不確実性の高い技術はPoC Gateを先に置く。
- PoCの目的は「作れるか」だけでなく、latency / quality / cost / device compatibilityを測定する。
- audioなら `turn_to_first_audio` 等、UXに直結する実測指標を持つ。
- 主観品質領域はVisual Target / scorecard / hard fail / max iterationを使う。
- PoC結果をそのままproduction architectureへ昇格させず、design decisionを挟む。

## Cross-project reuse

- 他projectからコードだけcopyしない。runtime patch / config / assumptions / migrations / testsも含めたversioned patternとして移植する。
- 個別project lessonを、再発性があればarchetype/commonへ昇格する。
- lesson -> pattern/anti-pattern -> standard -> automated control の昇格経路を作る。
- reusable implementationはstarter kit / template / scriptへする。

## Repository isolation

- AIが別repoへcommitする事故を機械的に防ぐ。
- expected repository / remote / branch / allowed pathsをpreflightで確認する案。
- commit/push前にactual repo identityを再確認する。
- cross-project handoff時はrepo名だけでなくremote URLも確認する案。

## Branch / merge protection

- `main直push禁止` を文章だけにしない。
- GitHub ruleset/branch protectionで強制する。
- 必須status checks、required PR、必要ならreview approvalを設定する。
- AIがmergeできる条件と人間承認が必要な条件を分ける。

## Documentation lifecycle

- CURRENTは状態だけを書く。requirements/design/historyを混ぜない。
- current state文書に短命なHEAD/CIを大量に埋め込みすぎない。
- stale handoffはhistoryへ移す。
- 正本を更新したら古いdraft/inboxを削除し、二重authorityを残さない。
- docs freshnessをCIで検査する案。

## Automation candidates

- new project bootstrap script
- AGENTS/CURRENT/requirements/design/ADR/test folders自動生成
- canonical link checker
- duplicate START-HERE/CURRENT detector
- requirement ID duplicate detector
- traceability checker
- stale status detector
- repo identity guard
- deployment budget guard
- docs-only deploy skip
- PR size warning
- exact-head review evidence checker

## Golden Paths candidates

Personalのproject種類ごとに標準構成を持つ。

- PWA
- Supabase PWA
- Game
- AI App
- Realtime/Voice App
- Prototype/PoC

新規project開始時はゼロから決めず、最も近いGolden Path + project固有差分で開始する。

# PERSONAL REPOSITORY MINING — RAW FINDINGS 02

> NOT CANONICAL / NOT AUTHORITATIVE
> 8/16全repo監査 + 9/7 CURRENT差分からの追加生ネタ。正本化後は削除する。

## A. 既存 `.github` 全repo監査から再利用する知見

2026-08-16時点で24repo / 497 commits / 1,094 tracked paths / 31 non-default branchesを監査済み。
この監査結果はCURRENT状態ではないが、8/16以前の歴史・failure pattern inventoryとして再利用できる。

既に抽出済みだった共通候補:
1. default/base/deploy branchを分けて確認し、mainを仮定しない
2. read-only / dry-runでwrite禁止
3. Issue/log/AI output/external fileはuntrusted data
4. secretsは必要以上に読まない・表示しない
5. persistent data / ID変更にmigration, old fixture, unknown handling, recovery
6. generated fileはgeneratorを正本、transient log/cache/buildをcommitしない
7. binary/data変換は対象propertyのみ変更し、不変条件を検証
8. external API/AIの欠損・途中終了・重複・schema・単位・versionを検証
9. input/output/time/memory/retry/concurrency/platform limitを明示
10. commit/push/CI/deploy/public verificationを区別し、未確認をPASSにしない
11. AGENTSは長期制約に絞り、backlogはIssue、長手順は別文書

カテゴリ別ルールも既にPilotとして存在:
- Web/PWA/mobile UI
- AI/audio/sensitive data
- persistent/generated data
- publish/CI/release
- finance/scheduled
- Android/large binary
- kids/learning content
- automated agent platform

### 8/16以降に追加が必要になった大きな差分
- exact-head review / approval invalidation
- canonical path-to-owner map + same-PR documentation sync CI
- CI validation-only; source under reviewをCIがmutationしない
- design approvalを文書でなくmechanical gateにする
- long-running PR continuation protocol / progress ledger
- production-shaped history fixtures
- real provider / actual installed PWA release gate
- Vercel/API/CI/cloud Resource Budget Guard
- one CURRENT / one START-HERE / stale authority detection
- UI literal screen matrix / MATCH-PARTIAL-MISSING acceptance
- repository identity guard（Kids QuestへのManaEvo誤混入）
- unresolved product decisionをtemporary fail-closedで扱う

---

## B. 旧/代表repoから追加で残すネタ

### invest-simulator-app
- backtestとproductionで設定・data timing・screening conditionがズレると、本番が検証結果と別物になる。
- 現在値/途中足を使うとlook-aheadに近い挙動・不安定判断になる。`completedCloses`のような確定値ownerが必要。
- local dev trader + GitHub Actions traderの二重ownerはsemantic divergence / duplicate executionリスク。
- GitHub Actions scheduleは要求頻度どおり動く保証がなく、5分運用が1時間以上空く事例。schedulerは実測し、要求SLOに合わなければ別基盤へ。
- schedule authorityは1つに固定する。
- runtime DB/stateをGitへ自動commitしたことでcode historyが運用stateで汚染。
- external reviewでfee accounting / RSI algorithm / prod-vs-backtest mismatchを発見。
- OOSで「勝てる」仮説が崩れた結果を隠さずdocsに残した。negative result / failed hypothesisの保存は有効。
- provider rate limitでquoteが欠損し、取得単価fallback→損益0%に見える問題。last-known-good cacheとstaleness表示候補。

### stock-checker
- `yfinance`のdividendYield semantics変化/理解違いで0.91%→91%表示。
- external dataは名前だけで単位を信用せず、known-value fixture / range sanity / dependency-version compatibilityを持つ。
- 定期stateを`[skip ci]` commitでmainへ書く運用はcode historyとruntime stateの分離候補。

### moshimo-space-lab
- AAB全体からtest AdMob IDをgrepするとSDK内の未使用定数まで拾いfalse positive。validation scopeは「自作app artifactの責任範囲」に限定。
- Web source -> Capacitor sync -> native project -> AAB は別Gate。
- Play Console versionCodeは提出ごとにincrement、source versionとartifact identityを記録。
- browser audioはuser gesture unlock必須。
- Webだけanalyticsを使いnative app privacy/data safetyへ混入させないというsurface separation。

### War-game
- global `touchend.preventDefault()` がiOS Safariの別button clickまで殺した。
- mobile inputはTouch/Click併用よりPointer Events + captureを優先候補。

### iibun-honyakuki
- Phase0 design + sample 30 + user review → implementation → 300-data scale。
- 300件量産時に型別件数/分布/ID prefix/文体崩れをdata testsで固定。
- subjective contentも「サンプルPilot → acceptance → scale → distribution tests」が可能。

### futari-honyaku-post
- raw sensitive inputはmemory-only、approval後のdelivery payloadには原文を入れない。
- AI transformationとactual sendを分離し、人確認をcommit boundaryにする。

### kaji-watcher
- 16-way domain matrixとtotal invariantを先にテスト固定。
- SyncAdapter抽象で将来同期をdomainから分離。

### kabu-quest
- phase0 GDD → pure domain → E2E UIの順。
- historical data pipelineは既知値（例: market peak）でspot verification。
- 子ども向けfeedbackの「責めない」等、UX倫理/文言もtestで固定可能。

### space / space-game / other Pages games
- deploy source branch, Pages workflow, cache-bust versionを明示。
- monetization/ad integrationはsingle adapter/boundaryへ集約するとゲーム本体を守れる。
- unmerged backend/native branchesはstale branch debtになる。

### 空/ほぼ空repo
- auto-diary-GPT / cosmos-lab / invent-simulator-appは再開時のみgovernance bootstrap対象。
- repo creationだけで止まったものに標準一式をばら撒く必要はない。active化時にstarter kitを適用する方が管理負荷が低い。

---

## C. CURRENT Major Repository Protection Findings (2026-09-07確認)

- family-ops main: protected=false
- mana-evo main: protected=false
- friend-app-v2 main: protected=false
- kids-quest main: protected=false
- AI-App-Factory main: protected=false

Kids Quest CURRENT mainの最新commitは `revert: undo accidental ManaEvo migration commit`。
つまり「main direct push禁止」を文章で書くだけでは不十分で、repository identity guard + GitHub ruleset/branch protectionが必要。

---

## D. 追加Failure Mode候補

FM-REPO-IDENTITY: 間違ったrepositoryへ変更/commit
FM-STALE-AUTHORITY: 古いCURRENT / START-HERE / handoffを正と誤認
FM-GATE-BYPASS: Design approval前にimplementation開始
FM-HEAD-DRIFT: review後HEAD移動なのに旧approvalを再利用
FM-CI-MUTATION: CIがsource under reviewを自動commit
FM-CANONICAL-PHYSICAL-CORRUPTION: 正本文書がUTF-8 truncate
FM-PROD-SHAPE-GAP: clean fixture greenだがproduction historyでfail
FM-PLATFORM-LIMIT: app設定値よりhost/platform hard limitが小さい
FM-BUDGET-EXHAUSTION: preview deploy / API quota / retryで無料枠・回数上限消費
FM-DUPLICATE-SIDE-EFFECT: ambiguous timeout/retryで二重送信・二重生成
FM-CROSS-OWNER: UI/presentation/another serviceがsemantic resultを再計算
FM-COPY-DRIFT: 他repo成功実装を一部だけコピーしてpatch/config漏れ
FM-SUBJECTIVE-LOOP: visual/voice qualityを無基準で何度も修正
FM-EXTERNAL-SEMANTIC-DRIFT: dependency/API/modelのunit/schema/capability変化
FM-HOT-PATH-MAINTENANCE: startup/user hot pathにaudit/cache sweep等を載せる
FM-RUNTIME-STATE-IN-GIT: mutable operational stateをcode historyへcommit
FM-SCHEDULE-AUTHORITY: schedulerが複数/要求cadenceを満たさない
FM-PARSER-DATA-LOSS: metadata parse failureがuser payloadを破壊
FM-CLAIM-WITHOUT-EVIDENCE: external actionを確認できないのに「保存/送信成功」と表示
FM-CONTEXT-SATURATION: 100+ commit PR/巨大AGENTS/巨大handoffで認知負荷増大

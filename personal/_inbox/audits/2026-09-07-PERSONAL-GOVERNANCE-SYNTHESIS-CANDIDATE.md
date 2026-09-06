# PERSONAL GOVERNANCE SYNTHESIS CANDIDATE — 2026-09-07

> NOT CANONICAL / NOT AUTHORITATIVE
> 全repo履歴から抽出した候補を整理したドラフト。ユーザーと決定後に正本へ分解・昇格し、このファイルは削除する。

## 0. まず決める構造

Personal内は3層にする。

1. `personal/common/` — 個人開発すべてに適用
2. `personal/archetypes/` — PWA / AI音声 / Game / DB連携 / Asset / Finance / Kids / Agent Factory 等
3. `personal/projects/` — Family Ops / ManaEvo / friend-app-v2 / Kids Quest 等の固有知見

旧 `syoudai0514/.github/agent-rules` は有用なPilot/historyとして移行元にするが、新 `ai-development-governance` と二重にcanonical運用しない。
最終的なPersonal正本は `ai-development-governance/personal/` に一本化する候補。

---

## 1. Personal Common — MUST候補

### PC-001 Repository Identity Preflight
作業開始時・commit前・push前に以下をfresh確認する。
- expected repository
- actual remote
- worktree/root
- default branch
- current branch
- base branch
- deploy branch
- actual HEAD
- dirty/uncommitted state

理由: Kids QuestへManaEvo変更を誤commitした実事故。
機械化候補: `scripts/preflight-repo` + repository identity config。

### PC-002 Single Authority Router
各repoに入口を1本だけ持つ。
- AGENTS.md = 短いrouter + 長期制約
- CURRENT = 1個だけ
- START-HERE = 1個だけ
- canonical requirements/design/ADRへの明示リンク
- history/handoffは `HISTORICAL — NOT CURRENT AUTHORITY`

理由: Family Ops / ManaEvoで複数CURRENT/START-HERE/handoffが残存。

### PC-003 Requirement / Acceptance Contract
実装前に確認可能なacceptanceを作る。
- Requirement ID
- screen/interaction matrix（UIなら）
- scenario
- expected evidence
- MATCH / PARTIAL / MISSING

理由: Family Ops最終モックとProduction UIの乖離。

### PC-004 Design Gate
product behavior / architecture / persistence / security等の重要変更は、Accepted Decision/Design IDなしにimplementationしない。
文章だけでなくimplementation PRをCIで拒否する候補。

理由: friend-app-v2 PR #24がapproval待ちなのに#25実装merge。

### PC-005 Canonical Sync in Same Change
product仕様変更に対し、owner requirements/design/ADRとcode/testsを同一変更単位で同期。
path-to-owner mapを持ち、CIで検査。

理由: ManaEvo PR #113が有効。

### PC-006 Exact-head Review
review/GOはcommit SHAに紐づく。HEADが変わったら旧GOをCURRENT approvalとして使わない。

理由: Family Ops #50で実際に必要となった。

### PC-007 CI Validation-only
CIはsource under reviewを自動commit/pushしない。
生成artifactはartifact storeまたはreportとして出し、source更新は別の明示PR/commit。

理由: ManaEvo #132でCIがaudit artifactをbranchへcommitし、レビュー対象を変更。

### PC-008 PR / Change Budget
PRは「1つのreviewable decision/work package」を基本単位にする。
単純なcommit数だけではなく、以下で過大化を検知する候補:
- 独立したproduct decisionが複数
- 多数のsubsystemを跨ぐ
- reviewerが1回でcall graph/acceptanceを追えない
- progressが複数ターンに渡る

暫定ソフト閾値候補:
- 30 commits超 or 50 files超なら分割検討/例外理由を記録
- 100 commits超は原則分割、同一atomic migration等の例外のみ

理由: Family Ops 84/151/203 commits、AI-App-Factory 155/173 commits。

### PC-009 Long-running PR Continuation Protocol
Atomic scopeのため長PRが必要な場合のみ、CURRENT fresh read + acceptance matrix + progress ledger + fixed status formatで毎ターン継続。
PARTIALをMATCHへ丸めない。

### PC-010 Vertical Slice First
大量実装・大量data/asset投入前に、最小E2E 1本を完成させる。

理由: Kids Quest 51-62で学習→battle→capture→growth→evolutionを先に完成。

### PC-011 Pilot → Freeze Acceptance → Scale
大量生成、content、asset、migration、automationは少量Pilotで品質基準を確立し、合格後のみ量産。

理由: Kids Quest monster art / iibun 30→300 pattern。

### PC-012 Production-shaped Acceptance
clean unit/CIだけではなく、リスクに応じてproduction-shaped fixtureを使う。
- historical DB rows
- actual provider semantics
- installed PWA
- real browser/device
- retry/order/duplicate scenarios

理由: Family Ops historical backlog、auto-diary/Vercel hard limit、iOS issues。

### PC-013 Real-world Gate
外部provider/端末/OS差が本質の場合、merge/deploy前後にreal gateを明示。
未実施はPASSにしない。

### PC-014 Side-effect Commit Boundary / Idempotency
send/save/charge/create/jobなどsemantic side effectについて:
- acceptance boundaryを明示
- stable idempotency key
- retry時はsemantic stateを確認
- post-commit UI/refresh failureをsemantic failureと表示しない
- multiple component ownersを作らない

理由: Family Ops duplicate LINE / AI-App-Factory triple submission / auto-diary duplicate Gemini。

### PC-015 Resource Budget Guard
無料枠/従量課金/回数制限を開発工程の資源制約として扱う。
- Vercel deployment count
- Preview deployment
- API RPM/RPD/tokens
- AI model quota
- CI minutes
- storage/network/build quotas
- retry budget

Vercel候補:
- main -> Productionのみ自動
- PR/feature/docs branch Previewはdefault off
- Previewは明示必要時のみ
- docs-only deploy skip
- anomaly / remaining budget monitoring

### PC-016 Platform Limit Registry
host/provider/browser hard limitsを各repoでmachine-readableに持つ候補。
例: request body / timeout / memory / URL / bundle / file / schedule cadence / provider quota。
アプリ自身の設定値よりplatform hard limitを先に確認。

### PC-017 External Semantic Contract
外部API/library/modelにはschemaだけでなくsemantic contractを持つ。
- unit
- range
- version
- model capability
- missing behavior
- staleness
- known-value fixture

理由: yfinance dividend yield 0.91%→91%、model deprecation/capability drift。

### PC-018 Canonical Artifact Integrity
canonical Markdown/YAML/JSON自体の物理破損をCIで防ぐ。
- strict UTF-8
- parse
- required headings/keys
- link integrity
- optional hash/size guard

理由: Family Ops canonical Markdown UTF-8 truncate。

### PC-019 Safe Temporary Restriction for Undecided Product Rule
要求未決定ならAIが永久仕様を発明しない。
必要なら既存機能を壊さない最小のtemporary fail-closedを明示し、Decision待ちとして管理。

理由: ManaEvo evolved-form capture。

### PC-020 Actual Runtime Path Review
似た名前のhelperやdocsから推測せず、entrypoint→semantic mutation→side effectまでactual call graphを追う。

理由: ManaEvo post-win capture review。

### PC-021 One Semantic Owner
同じ意味の結果をUI/presentation/provider層で再計算しない。
stateを確定したsemantic ownerがfacts/eventsを出し、presentationは消費する。

理由: ManaEvo Battle presentation trace。

### PC-022 Minimum-field Mutation / Preserve Unrelated State
asset/config/object変更は対象propertyだけ変更し、関係ないpropertyを保持する。

理由: friend-app material全置換でtexture破壊。

### PC-023 Observability Before Optimization
性能・品質問題はmetricを先に持つ。
- TTFA
- startup duration
- payload size
- provider calls
- retries
- deploy count
- user scenario timing

理由: friend voice `turn_to_first_audio`, ManaEvo playtest telemetry。

### PC-024 Hot-path Budget
startup/first interaction/hot requestにmaintenance/audit/full sweepを載せない。
maintenanceは非同期/明示タスクへ分離。

理由: ManaEvo startup SW update + asset sweep/cache audit。

### PC-025 Evidence-based Completion
以下を別状態として扱う。
IMPLEMENTED / COMMITTED / PUSHED / CI GREEN / REVIEWED exact head / MERGED / DEPLOYED / PRODUCTION VERIFIED。

### PC-026 Knowledge Promotion
各project lessonを以下で昇格:
Project lesson -> Archetype pattern -> Personal Common standard -> Template -> Script/CI。
正本へ昇格したら `_inbox` raw materialを削除する。

---

## 2. Personal Archetype候補

### A. Web / PWA / iOS
- safe area / notch / keyboard / scroll
- Pointer Events / gesture-gated APIs
- PWA installed upgrade/offline/cache
- build-time vs serverless runtime filesystem
- actual device/browser gate
- Vercel deploy budget

### B. AI / Realtime / Voice / Audio
- model/provider capability registry
- structured output + runtime schema validation
- output/token/length/repetition guard
- quota vs transient retry classification
- TTFA and streaming UX SLO
- speculative computation cannot commit/play before semantic ack
- bounded prefetch/backpressure
- audio chunk/memory/resample rules

### C. Game / Stateful app
- pure semantic engine
- presentation consumes semantic trace
- exactly-once settlement/battle result
- save schema/migration/unknown handling
- cloud CAS/conflict/reentrancy suppression
- telemetry-driven balance tuning

### D. Asset / 3D / Binary
- source/provenance/hash/manifest
- immutable original -> new output
- exact allowed changed files
- stale bundle rejection
- structure + checksum + visual QA
- visual target + scorecard + hard fail + max iterations
- patch only intended property

### E. DB / Serverless / External Integration
- production-history fixtures
- migration dependency preflight
- forward-only/transactional migration
- distributed rate limit semantics
- OAuth/LINE/provider manual real gate
- history ordering uses immutable semantic fields

### F. Finance / Scheduled / External Data
- simulation vs real action boundary
- one scheduler authority + measured cadence
- idempotent period lock
- timezone/holiday/market session fixtures
- lookahead prevention / completed data only
- backtest-production parity
- unit/range/known-value fixtures
- runtime state separate from source Git history
- record negative/OOS results

### G. Kids / Learning / Generated Content
- immutable content/identity fingerprint
- answer / explanation / unit / reading independently tested
- option/distribution/bias tests
- registration→dispatch→UI→save→review reachability
- age-appropriate language and UX principles testable
- license/provenance

### H. Automated Agent / Project Factory
- Issue/prompt/output untrusted
- allowlisted structured commands/paths/models
- isolated workspace / non-root / minimum network / no credentials
- durable jobs/leases/heartbeat/idempotency
- evidence hash tied to exact commit
- automated tests do not remove human merge/release gate for high-risk changes

---

## 3. Project-specific retained knowledge candidates

### Family Ops
- LINE production phrase regression corpus
- conversation pending/history ordering/backlog
- Supabase migration/prod schema history
- 34-screen literal UI parity matrix
- provider mutation isolation

### ManaEvo
- battle/capture semantic model
- monster asset FAST LANE
- cloud save conflict/CAS
- iPad / installed PWA continuity
- content/asset canonical mapping

### friend-app-v2
- Live/voice/audio architecture
- persona/voice target
- avatar/VRM/GLB visual pipeline
- realtime commitAck + streaming performance

### Kids Quest
- 1000-monster immutable identity
- learning mastery rules
- monster generation model routing
- learning→battle vertical slice

### AI-App-Factory
- factory trust boundary
- sandbox/lease/job protocol
- human-review generation workflow

### Finance apps
- strategy/backtest specifics and data-provider contracts

---

## 4. 先に機械化するP0候補

1. repository identity preflight
2. branch protection / ruleset + PR-only
3. single CURRENT / START-HERE / authority validator
4. canonical document UTF-8/parse/link integrity
5. accepted design/requirement reference gate
6. canonical path-to-owner sync check
7. CI non-mutation guard
8. Vercel main-only + Preview budget guard
9. Resource Budget config/check
10. release evidence: exact SHA / CI / deployed SHA / production verification

P1:
- production-shaped fixture templates
- platform limit registry
- external semantic contract registry
- long-running PR progress ledger/checker
- stale PR/doc/branch report

P2:
- project bootstrap/starter kit
- archetype selection generator
- knowledge promotion helper

---

## 5. 新規Personal Project Golden Path候補

1. repo作成
2. project archetype選択
3. starter kitでAGENTS / CURRENT / requirements / design / ADR / tests / CI生成
4. repository identity + branch/ruleset + deploy policy設定
5. product requirement + acceptance matrix
6. risky/uncertain部分はDesign/PoC/Pilot Gate
7. Vertical Sliceを最初に完成
8. small implementation PR
9. source tests + production-shaped tests
10. independent exact-head review（risk-based）
11. real device/provider/manual gate（必要時）
12. merge
13. mainからのみproduction deploy
14. deployed exact SHAを実利用確認
15. lessonを`_inbox`へcapture
16. 再発性があればarchetype/commonへ昇格

---

## 6. Long-running PRの扱い候補

### 長PRを許す条件
- 同じatomic product decision / migration / closeoutである
- 分割すると一時的に不整合状態を作る
- acceptance matrixで全体を追跡できる

### 分割すべき条件
- 独立したproduct decisionが複数
- subsystemごとに独立review/merge可能
- UI/DB/provider等のrisk gateが別
- 1 reviewでcall graph/acceptanceを追えない

### 継続方法
CURRENT fresh read -> remaining PARTIAL/MISSING -> 最大安全量 -> test -> docs -> commit/push -> fresh recheck -> fixed progress report。

---

## 7. 最初にユーザーと確定したいDecision候補

D-P01: `ai-development-governance/personal` を今後のPersonal governance唯一の正本にするか。
D-P02: 旧 `.github/agent-rules` はmigration source/historyへ降格するか。
D-P03: major active reposのbranch protection/rulesetを必須化するか。
D-P04: PR-onlyを原則にし、direct mainはexplicit emergency/manual exceptionだけにするか。
D-P05: Design Gateをどのrisk以上で必須にするか（全変更には重すぎる）。
D-P06: PR soft limitを30 commits/50 files、100 commits原則splitで試行するか。
D-P07: new project Golden Pathをstarter kitとして自動生成するか。
D-P08: Vercel main-only / Preview default offをVercel利用Personal repoの標準にするか。
D-P09: project lessonsを毎PR/incident後に自動候補化する運用を入れるか。


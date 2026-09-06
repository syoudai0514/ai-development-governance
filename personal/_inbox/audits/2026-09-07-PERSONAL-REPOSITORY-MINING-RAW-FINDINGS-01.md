# PERSONAL REPOSITORY MINING — RAW FINDINGS 01

> NOT CANONICAL / NOT AUTHORITATIVE
> 調査途中の生ネタ。正本へ昇格した内容はこのファイルから削除し、最終的にこのファイル自体を削除する。

## 調査対象済み
- family-ops
- mana-evo
- friend-app-v2
- kids-quest
- AI-App-Factory
- friend-app (旧版)
- auto-diary
- stock-checker（一部）

---

## 1. 開発方式の時系列的な進化

### 初期: main直接commit + 本番/実機で発見して即修正
friend-app旧版、auto-diary、stock-checkerではPR履歴がほぼ/完全に無く、main上の連続commitが中心。

得られたもの:
- 実際のiPhone/PWA/Vercel/provider挙動を早く学べる
- 小さな問題を高速で修正できる

問題:
- review gateが無い
- 変更前後のexact scopeが弱い
- rollback/release承認の境界が弱い
- CURRENT設計・要件との同期がない
- 同じ種類の問題を次PJでも再発させやすい

### 中期: AGENTS / PR / E2E / independent review
Kids QuestでAGENTS + CLAUDE薄入口 + Issue Form + PR template + model routingを早期導入。
friend-app-v2で小PR・実機修正・transaction設計・CIを導入。

### 後期: canonical design + exact-head review + automated governance
ManaEvoで Design Review -> Canonical Promotion -> Implementation -> exact-head independent review -> real-device release gate が成熟。
Family Opsで Requirements -> Design -> ADR -> implementation -> source review -> real provider/production gate まで拡張。

---

## 2. Family Ops 生ネタ

### 巨大PR
- PR #10: 84 commits
- PR #44: 151 commits
- PR #45: 93 commits
- PR #50: 203 commits

候補改善:
- PR size / change-budget gate
- long-running PR continuation protocol
- Requirement IDごとのwork packageとacceptance ledger
- 最終closeoutを大量実装の場にせず確認中心にする

### Canonical文書そのものの物理破損
PR #39のレビューでcanonical MarkdownがUTF-8途中切断していた。
意味レビュー以前に物理的に文書が壊れていた。

候補改善:
- canonical artifact integrity CI
- UTF-8 strict decode
- expected headings / terminator / minimum structure
- refetch + hash check

### CI GREEN / source conformanceでもProductionでは失敗
PR #50でclean CI DBでは再現しないproduction historical backlog問題。
古いpendingをlazy expiryした更新時刻がrecency順を汚し、新しいdraftをshadow。

候補改善:
- Production-shaped historical fixtures
- immutable ordering key優先
- real provider / real history release gate
- clean DB testだけで完了扱いしない

### 実際のユーザー文言でclassifierが壊れる
help文、予定照会、曖昧依頼などがtask mutationへ誤分類。

候補改善:
- production phrase -> permanent regression corpus
- query/mutation intentはfail-closed
- mutationには明示confirm/state gate

### Duplicate side effects
LINE確認が二重送信された事例。

候補改善:
- one owner per side-effect path
- idempotency key
- exactly-once acceptance

### UI contract後追い
最終モック/承認UXが存在してもproduction実装に十分反映されなかった。

候補改善:
- screen matrixをimplementation前にacceptance contract化
- screenshot / interaction contract
- MATCH / PARTIAL / MISSINGをsource + actual UIで管理

### Prod migration dependency
本番既存FKなどclean環境と異なる状態でmigration失敗。

候補改善:
- production schema/history preflight
- forward-only + transactional migrations
- real dependency inventory

---

## 3. ManaEvo 生ネタ

### 最も成熟したGolden Path候補
Design Review -> Canonical Promotion -> Implementation -> exact-head independent review -> actual device/release gate。

### canonical syncをCI化
PR #113:
- runtime/art path -> owner CURRENT docs mapping
- PR declaration
- same-PR canonical sync validation

Personal共通へ強く昇格候補。

### 要件未決定時に勝手に仕様を発明しない
PR #157: evolved-form captureの長期ルール未決定中、temporary fail-closed。

候補原則:
UNDECIDED PRODUCT RULE -> SAFE TEMPORARY RESTRICTION -> PRODUCT DECISION -> DESIGN -> IMPLEMENTATION

### reviewはactual runtime call graphを追う
PR #153でgeneric capture helperだけを見ると誤判定しうる。実runtimeはdedicated postWinCaptureを使用。
本当のbugはstale same-battle snapshot replayだった。

候補改善:
- entrypoint-to-side-effect call graph review
- similarly named helperをauthorityと推測しない

### semanticsとpresentationの二重計算禁止
Battle presentation側で結果再計算していたものを、実際にstate mutationしたsemantic engineのpresentationEventsへ統一。

候補原則:
One semantic transaction owns truth; presentation consumes committed facts.

### CIがPRを勝手に変更した
Monster Art visual audit workflowがartifactをbranchへauto-commitし、レビュー対象HEADがCI後に変わった。PR #132を閉じてvalidation-only CIに修正。

候補原則:
CI MUST NOT MUTATE SOURCE UNDER REVIEW.

### binary/asset transactionが強い
FAST LANEで:
- baseHeadSha
- expectedCurrentSha
- new SHA
- family references
- transactionId
- stale bundle reject
- exact allowed changed files
- merge-base -> HEAD scope guard
- provenance/history

asset-heavy archetypeへ昇格候補。

### iPad / responsive
fail-first acceptance -> structural fixes -> continuity tests -> manifest orientation unlock -> real iPad / installed PWA gate。

候補原則:
Risky product capability flag/config unlocks only after behavioral acceptance proves readiness.

### hot pathにmaintenanceを置いた
Dex artでstartupごとにSW update/cache sweep、download中O(n^2) audit。Production performance問題になった。

候補改善:
- startup work budget
- operation complexity guard
- maintenance separated from hot path

### external CI dependency outage
npm audit service outage時、dependency filesが変わっていなければwarning、dependency change時やreal findingはfail-closed。

候補:
External Control Dependency Outage Policy

### stale/open PR debt
過去目的のopen PRが残存。

候補:
- stale PR review/cleanup cadence
- superseded marker + close rule

---

## 4. friend-app-v2 生ネタ

### Gate bypass
PR #24: implementation begins only after APPROVE FOR IMPLEMENTATION と明示。
PR #25: #24 openのままimplementationをmerge。

重要Failure Mode:
Written gate != enforced gate.

候補改善:
- implementation PR requires accepted Decision/Design ID
- unresolved design-review dependency => CI fail

### 別PJからのコピー漏れ
PR #31でManaEvoのTsukuyomi modelを利用したがruntime patchが漏れ、声が一致しなかった。PR #32でparity修正。

候補改善:
- copy filesではなくversioned reusable package/pattern
- source version/SHA + parity tests
- dependency manifest

### quota surprise
- Gemini TTS 10 RPDで方式変更
- Vercel Hobby build-rate-limitでrelease retry PR

候補改善:
Resource Budget Guard:
- deploy count
- provider RPD/RPM/token
- CI minutes
- network/build quota
- retries also budget-aware

### duplicate computation/provider cost
Liveがすでにaudio生成済みなのに/api/ttsで再生成、8-20秒追加。

候補改善:
- generation identity / cache key
- same artifact reuse
- cost + latency tracing

### iPhone固有audio
user gestureでHTMLAudioElementをprimeしないと音が鳴らない。

PWA/iOS archetypeへ保存。

### realtime latencyを実測
turn_to_first_audioを導入。first stable chunkをspeculative synthするがcommitAck前に再生しない。

候補:
- UX SLOを測る
- semantic commitとpresentation precomputationを分離

### bounded prefetch
chunk N再生中にN+1だけsynthesize。無制限並列を避ける。

候補:
bounded concurrency / backpressure pattern

### visual/3D主観品質の反復
Rena face -> neck seam -> head offset -> jaw -> neck/material v5 と連続修正。

後のv3 Gate Aで改善:
- Visual Target
- 100pt scorecard
- hard fail
- PASS >=90
- max two iterations
- runtime work禁止

visual/3D archetypeへ昇格候補。

### material全置換で別propertyを破壊
skin sheen修正でbaseColorTextureまで置換し衣装が壊れた。

候補原則:
Patch minimum fields; preserve unrelated state.

### runtime補正がasset調整を上書き
ファイル側チューニングをruntime general patchが上書き。

候補:
Single transformation owner / hidden layered transforms禁止。

### push-main helper
旧PR #15でmain push helperを追加。現在のPR-first思想と矛盾するanti-pattern候補。

---

## 5. Kids Quest 生ネタ

### AIルールを早期導入
AGENTS + CLAUDE薄入口 + Issue Forms + PR template + model routing。

### 1000 identityをimmutable snapshot化
1000 IDs/names/typesなどをsnapshot + SHAで固定。
件数だけでなくtarget集合、順序、boss tier等をfingerprint。

候補原則:
Critical natural-language invariant -> machine-readable immutable fingerprint.

### Pilot -> Scale
1000体画像を一気に作らず、51-62の12体 + temporary formsで基準を確立。
不採用理由をpromptへ戻し、合格後のみ次batchへ。

候補原則:
Pilot first, freeze QA, then scale.

### 高価/高知能モデルを判断へ集中
Solで基準/採否、Terraで量産。2回不採用などでSolへescalation。

候補:
Model routing by risk and judgment intensity.

### Vertical Slice First
大量画像より先に学習 -> 戦う -> 捕獲 -> 育成 -> 進化/形態解放を51-62で実装。

候補原則:
Before mass production, ship one end-to-end vertical slice.

### Cross-repo contamination
後のmainにManaEvo migration commit誤混入をrevertした履歴あり。

候補改善:
- expected repo/remote/branch preflight
- commit hook checks repository identity
- allowed path / worktree isolation

---

## 6. AI-App-Factory 生ネタ

### 目的自体がPersonal Project Factoryに直結
スマホ依頼 -> worker -> tests/security -> Draft PR -> human review/merge の自動工場思想。

### 巨大Draft
PR #1: 173 commits
PR #2: 155 commits
長期作業が1PRへ集中。

候補改善:
- factory core / security foundation / generated app jobを分割
- exact work package + progress ledger
- architectural gate単位でPR分割

### human review required
生成job PRはautomated tests / Playwright mobile smoke / gitleaksを通してもHuman review and merge required。
良い原則。

### security fail-closed
- fixed ACL
- hash binding
- lease-bound execution
- clock rollback denial
- reparse rejection
- fixed PATH
- no network / read-only / non-root containers
- request/component labels

Personal factoryの高リスク自動実行へ参考。

### post-accept UI failureでduplicate submission
Queue 202 acceptance後のrefresh/render failureをsend failureと表示し、retryで3重request。
修正:
- 202 boundaryをacceptanceとする
- refresh failureを別statusにする
- payload SHA + pending idempotency key
- server exact-active dedupe
- transaction内duplicate recovery

候補原則:
Acceptance boundary must be explicit; post-commit UI failure must not cause semantic retry.

---

## 7. friend-app 旧版 生ネタ

### PRなし/main commit中心
現在のfriend-app-v2との比較でガバナンス成熟度が明確。

### provider/model drift
Gemini model名、thinkingConfig、fallback、無料枠、preview model等で連続修正。

候補改善:
- provider capability discovery
- model compatibility contract
- allowed model registry
- budget-aware fallback
- model change canary/eval

### production asset manifest差
runtimeでpublic/を読める前提がVercel serverlessでは成立せず、本番で立ち絵が消えた。

候補:
Build-time vs runtime filesystem contract test.

### PWA safe-area
home screen PWAでノッチとbutton重なり。
PWA/iOS standardへ。

### response parser corruption
memory tag incomplete時、本文末尾まで消していた。

候補:
- parser fail-safe: metadata parse failure must not destroy user-visible payload
- structured output preferable to hidden inline tags

### baseline snapshot before v2
v1の主要画面/保存schemaを記録しschemaVersion導入してからv2へ。
良いmigration pattern。

### Vercel deploy only main
旧friend-appでも後からdeploy only mainへ修正。
Resource Budget Guardの実例。

---

## 8. auto-diary 生ネタ

### PRなし/main commit中心だがテスト文化が強い
56 testsの初期実装から、E2Eを常設化し200+ testsへ。

### 実環境上限をコード設定と混同
アプリ側15MBでもVercel request body約4.5MBでコード到達前にreject。

候補:
Platform Limit Registry:
- request body
- function timeout
- bundle
- storage
- API quotas
- deploy limits

### browser audio transform
container bytesを単純分割すると壊れるためdecode -> PCM -> resample -> self-contained WAV chunks。
実ブラウザでdecodeAudioDataのupsamplingも発見。

PWA/media archetypeへ。

### timeout mismatchでduplicate expensive requests
client timeout < server max durationにより正常処理中でもclient失敗→retry→Gemini重複呼び出し。

候補原則:
Timeout hierarchy must be coordinated end-to-end; retry only if semantic state known.

### save successを偽っていた
外部アプリ起動後に実際に保存されたか確認できないのに「保存しました」。下書きも削除していた。
修正: 実施した操作のみ表示し、draftを24h保持。

候補原則:
Never claim external side effect success without evidence; retain recoverable state until confirmed/TTL.

### API response schema runtime validation
`as Diary` castからruntime schema validationへ。
Personal共通へ。

### giant page refactor
1537-line page.tsx -> components + pure logic; E2E before/after。
候補: module/file complexity threshold + behavior-preserving refactor gate.

### distributed rate limit
serverless instance-local counterが実質無効→Upstash shared counter。

候補:
Distributed infrastructure semantics must be tested in topology-shaped environment.

### AI hallucinated transcription repetition
無音で同一発言を大量反復。maxOutputTokens + repeated-line collapse。

AI media archetype:
- output size invariant
- repetition detector
- bounded output

### roadmap/handoff reusing lessons
既知の落とし穴13項目、モデル切替に耐えるplan-first/handoffをdocsへ保存。
良いknowledge captureの初期例。

---

## 9. Stock Checker 生ネタ（途中）

### PRなし/main automation中心
Scheduled state commits `[skip ci]` が定期的にmainへ入る。

検討候補:
- operational mutable stateをGit historyに書くべきか
- code historyとruntime stateを分離

### upstream library unit semantics changed
`yfinance` dividendYieldがpercent valueなのにsub-1を100倍して90%超表示。

候補:
External Data Semantic Contract:
- library/version fixture
- known-value sanity bounds
- unit/property tests
- dependency update canary

---

## 10. 現時点の最重要Common候補

1. Fresh CURRENT + repository identity preflight
2. Single canonical authority / one START-HERE / one CURRENT
3. Design approval must be mechanically enforced
4. Canonical doc sync CI
5. Exact-head approval invalidation
6. PR/change-budget + long-running PR continuation
7. Vertical Slice First
8. Pilot -> Scale
9. Production-shaped tests + real device/provider gates
10. Resource Budget Guard (Vercel/API/CI/cloud)
11. CI must be validation-only; no source mutation
12. Branch protection / PR-only; no direct main push
13. Runtime call graph review
14. Side-effect idempotency / explicit commit boundary
15. Retry only with known semantic state
16. Platform Limit Registry
17. External dependency/model semantic compatibility tests
18. Visual/subjective work uses target + scorecard + hard fail + max iterations
19. Knowledge copy uses versioned reusable pattern + parity tests
20. Lessons -> Pattern -> Standard -> Template -> Automated Control

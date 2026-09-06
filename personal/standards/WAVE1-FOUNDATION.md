# Personal Standard — Wave 1 Foundation

Status: CANONICAL CANDIDATE
Version: 1.0.0
Scope: active personal software repositories

## 1. Purpose

AIの記憶・会話・handoffに依存せず、間違ったrepo、古いHEAD、競合する正本、無断main反映、古いreview、設計未承認実装、CI自己書換えを仕組みで防ぐ。

## 2. Repository Identity Gate

作業開始時に必ず以下を確認する。

- owner/repository
- default branch
- working/base branch
- deployment branch
- actual HEAD
- remote
- current PR number/status/head when applicable

### Mechanical target

CIまたはbootstrap scriptでrepository identityを明示し、別repo由来のproject ID / package identity / canonical marker混入を検知する。

## 3. Fresh CURRENT Gate

重要判断はCURRENT GitHubからfresh readした事実に基づく。

禁止:
- 過去チャットのHEADをCURRENT扱い
- handoffのCIを最新扱い
- PR本文をsource of truth扱い
- closed reviewの評価を新HEADへ自動継承

完了報告では必要に応じてactual HEAD / branch / PR / CI / changed files / production stateを再確認する。

## 4. Authority Gate

各projectはauthority orderを明示する。

最低構成:
1. START-HERE
2. CURRENT
3. canonical requirements
4. canonical design
5. Accepted ADR
6. CURRENT source/schema
7. Issue/PR/handoff history

`CURRENT`を名乗る正本を複数作らない。同一authorityで競合する場合は実装で勝手に補完しない。

## 5. PR-Only Default

Active personal repoは原則としてdefault/release branchへ直接pushしない。

例外:
- 人間が明示したemergency operation
- 例外理由・scope・検証・rollbackを記録

### Mechanical target

GitHub ruleset / branch protectionで:
- pull request required
- force push blocked
- deletion blocked
- required checks configured
- bypassは最小化

## 6. Exact-Head Review Gate

レビューはPR番号ではなくexact commit SHAへ結び付ける。

レビュー後にHEADが変わった場合、旧GOは新HEADへ自動継承しない。

完了条件:
- reviewed exact SHA
- review verdict
- required CI for same SHA
- changed files reviewed

### Mechanical target

review evidenceとcurrent PR headを比較し、SHA不一致ならmerge/release gateをREDにする。

## 7. Risk-Based Design Gate

高リスク変更は、Design Review → Accepted → Implementation の順を守る。

高リスク例:
- auth / security / permissions
- DB schema / migration / persistence
- external provider mutation
- billing / quota / deployment architecture
- state-machine / concurrency / idempotency
- major UX interaction contract
- large generated asset pipeline

低リスクの文言/CSS調整まで別Design PRを強制しない。

### Mechanical target

高リスクpathまたはIssue labelに対してAccepted design/ADR IDを要求し、未承認ならimplementation gateをREDにする。

## 8. CI Validation-Only

PR/feature CIは原則としてsource branchを自動commitしない。

禁止方向:
- formatter/testがreview branchへ自動push
- generated docsの差分を勝手にcommit
- runtime stateをsource historyへ定期commit

必要な生成物は差分検証し、失敗させて人間/実装agentが明示commitする。

例外は専用bot branch等へ隔離し、source review branchと分離する。

## 9. Canonical Sync Gate

仕様変更を伴うコード変更では、canonical requirements/design/ADRを同じ変更単位で更新する。

逆も同様に、設計だけ更新して実装が古い状態を「完了」としない。

### Mechanical target

source path ↔ owning requirement/design mappingを持ち、対象source変更時にcanonical documentまたは明示的な`no-spec-change`判定を要求する。

## 10. Completion Gate

「完了」は以下を照合して判断する。

Requirement → CURRENT Design → CURRENT Implementation → Test → Real Usage Scenario

禁止:
- 「だいたい合っている」でPASS
- CI GREENだけでproduction ready
- deploy成功だけで公開確認済み
- 外部operationの成功を観測できないのに成功表示

## 11. Rollout Order

P0:
1. ai-development-governance自身
2. family-ops
3. mana-evo
4. friend-app-v2
5. kids-quest
6. AI-App-Factory

P1:
- auto-diary
- stock-checker
- invest-simulator-app
- その他active repo

Paused/empty repoは再開時にstarter kitで導入する。

## 12. Machine-Enforcement Backlog

Wave 1完了条件は文章作成ではなく、少なくとも次を実装すること。

- branch/ruleset protection
- repository identity check
- single START-HERE/CURRENT validator
- exact-head review validator
- CI source-mutation prohibition/check
- canonical sync validator
- accepted-design gate for high-risk work

## 13. Evidence Origin

この標準は、Family Ops / ManaEvo / friend-app-v2 / Kids Quest / AI-App-Factoryおよび2026-08-16全repo監査の再発事例・成功パターンから抽出した。

個別事例は`personal/_inbox/`に保持するが、実装判断はこの正本を使う。

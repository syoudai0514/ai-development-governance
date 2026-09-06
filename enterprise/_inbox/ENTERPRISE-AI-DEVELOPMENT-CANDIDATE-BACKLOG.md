# Enterprise AI Development Candidate Backlog — NOT CANONICAL

会社・大規模開発向けの検討ネタ。Personalとは独立して正本化する。

## Governance themes

- AI生成物のsource of truthを明確化する。
- AI memory / chat / prompt / handoffを正式な仕様・承認記録として扱わない。
- requirements / design / ADR / code / test / release evidenceのauthorityを定義する。
- AI利用工程ごとにHuman accountabilityを明示する。
- AIが仕様変更を提案することと、仕様を承認することを分離する。
- AI実装、AIレビュー、人間レビューの責任分界を定義する。

## Lifecycle candidates

- Requirement definition
- Design
- Implementation
- Test
- Review
- Release
- Operation

各工程で以下を明確化する案:

- AIに許可する作業
- AIに禁止する作業
- 人間承認が必要な作業
- 必要evidence
- quality gate
- rollback/escalation条件

## Failure-mode based governance

ルール集ではなく、Failure Mode -> Risk -> Standard -> Control -> Evidenceで体系化する。

候補Failure Mode:

- stale state anchoring
- authority collision
- requirement drift
- design drift
- false completion / hallucinated GREEN
- context saturation
- cross-project contamination
- review scope error
- environment gap
- tool/provider quota exhaustion
- unauthorized external mutation
- sensitive data leakage
- non-reproducible AI output

## Controls candidates

### Preventive

- canonical artifact requirement
- branch protection / required PR
- design approval gate
- repository/workspace isolation
- tool permission boundary
- model/tool allowlist
- confidential data handling rules
- deployment/resource budget rules

### Detective

- traceability check
- design/code divergence detection
- stale document detection
- AI-generated change tagging/provenance
- quality metrics
- model output evaluation
- deployment anomaly detection

### Corrective

- revert/rollback procedure
- requirement/design resynchronization
- AI incident review
- lesson-to-standard promotion

## Large-scale development themes

- 数百〜数千ファイルをAIで変更/レビューする場合のchunking、coverage、samplingではない完全性保証。
- UI rule/YAML等のmachine-readable standardとAI reviewの組み合わせ。
- model変更/provider変更による検知率・速度劣化を継続計測する。
- benchmark dataset / golden setを固定し、モデル更新前後で比較する。
- precision / recall / false positive / false negativeを品質指標化する。
- prompt/model/version/toolchain/procedureをexperiment evidenceとして記録する。
- 同じ入力でも生成結果が変わる前提で、deterministic controlsを外側に置く。

## Change management

- AI利用標準自体をversion管理する。
- 標準変更時の影響分析を行う。
- projectごとのtailoring ruleを定義する。
- MUSTとSHOULDを区別する。
- 例外申請/期限/責任者/代替controlを管理する。

## Traceability candidates

`Business Requirement -> System Requirement -> Design -> Code -> Test -> Release Evidence`

AIが作った成果物も同じtraceabilityへ乗せる。

- requirement ID
- design ID
- change/PR
- test evidence
- reviewer
- release version

を追跡可能にする。

## AI quality evaluation candidates

- AIによるレビューの検知率
- false positive rate
- correction success rate
- escaped defect rate
- human rework time
- elapsed time
- token/API cost
- model/provider/version差
- task complexity別 performance

「AIを導入して速くなった」ではなく、品質・速度・コストを継続計測する。

## Environment / toolchain drift

会社PoC等で、同じプロンプトでも時期・モデル・Copilot/Claude/GPT更新で性能が変わる可能性を前提にする。

候補対策:

- golden benchmark
- scheduled regression
- model/version記録
- threshold below baselineで利用停止/切替
- model routing policy

## Security / compliance candidates

- confidential data classification
- external modelへの送信可否
- source code送信可否
- customer data / personal information handling
- prompt injection / untrusted repository content
- MCP/tool permission
- audit logging
- generated code license/security scan

## Resource governance candidates

AIは高速反復するため、従来より外部リソースを急速消費し得る。

- CI minutes
- cloud build/deploy count
- LLM/API cost
- test environment creation
- browser/device farm
- storage/log volume

resource budget / threshold / approval gateを標準化する案。

## Documentation candidates

Enterprise側では以下を将来分離する案:

- Policy
- Standard
- Procedure/Playbook
- Control Catalogue
- Template
- Evidence Guide
- Metrics
- Risk Register

## Important

上記は検討候補であり、現時点で会社標準ではない。

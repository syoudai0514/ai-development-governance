# PERSONAL REPOSITORY MINING COVERAGE — 2026-09-07

> NOT CANONICAL / NOT AUTHORITATIVE
> 今回の調査カバレッジ確認用。正本化後は削除する。

## 判定
- DEEP-CURRENT: CURRENT metadata/branch + recent PR/commit historyを重点確認
- DELTA-CURRENT: 2026-08-16全repo監査を土台に、CURRENT/最近の履歴を追加確認
- HISTORICAL-COVERED: 2026-08-16全repo監査で履歴を確認済み。今回、後続大変更が主要検索で見つからない/活動が古い
- EMPTY/BOOTSTRAP: 空またはほぼ空。再開時にstarter kit対象

| repo | coverage | 主な抽出対象 |
|---|---|---|
| family-ops | DEEP-CURRENT | 203-commit PR, exact-head, prod-history gap, UI parity, LINE/Supabase |
| mana-evo | DEEP-CURRENT | design promotion, canonical sync, CI mutation, asset FAST LANE, PWA/iPad |
| friend-app-v2 | DEEP-CURRENT | design-gate bypass, realtime audio, quota, 3D subjective QA, cross-project copy drift |
| kids-quest | DEEP-CURRENT | immutable 1000 identity, pilot→scale, vertical slice, repo contamination |
| AI-App-Factory | DEEP-CURRENT | 155/173-commit long PR, sandbox, lease/idempotency, human gate, project factory |
| ai-development-governance | DEEP-CURRENT | new target repo, raw inbox strategy, current branch protection state |
| .github | DEEP/HISTORICAL | prior 24repo audit, common/category Pilot rules, unresolved distribution/branch/model-routing issues |
| friend-app | DELTA-CURRENT | direct-main era, model/provider drift, serverless asset manifest, PWA safe-area, Vercel main-only |
| auto-diary | DELTA-CURRENT | PWA/audio/serverless hard limits, E2E, timeout/retry, runtime schema, rate limit |
| stock-checker | DELTA-CURRENT | external data unit semantics, scheduled state commits, finance automation |
| invest-simulator-app | DELTA-CURRENT | prod/backtest parity, OOS/negative results, scheduler authority, state-in-git |
| moshimo-space-lab | HISTORICAL-COVERED | Capacitor/AAB, AdMob validation scope, versionCode, gesture audio, privacy |
| War-game | HISTORICAL-COVERED | iOS touch/click conflict, Pointer Events, Pages/native branch |
| space-game | HISTORICAL-COVERED | monetization boundary, Capacitor pipeline, stale backend/native branches |
| space | HISTORICAL-COVERED | Pages deploy-source changes, cache busting, simulation correctness |
| space-lab | HISTORICAL-COVERED | kids/science content, generated visuals, cache/versioning |
| kabu-quest | HISTORICAL-COVERED | GDD→pure domain→E2E, known-value market data, child UX invariants |
| kaji-watcher | HISTORICAL-COVERED | phase0 design, 16-way matrix/invariants, SyncAdapter abstraction |
| iibun-honyakuki | HISTORICAL-COVERED | design/sample Pilot→300 pattern scale, distribution/content tests |
| futari-honyaku-post | HISTORICAL-COVERED | memory-only raw sensitive input, approval boundary, safe delivery payload |
| genai-study | HISTORICAL-COVERED | prompt version/change via PR, customer feedback, routing pattern |
| handson | HISTORICAL-COVERED | learning repo, portable STUDYLOG, API contract/error handling |
| minecraft-game | HISTORICAL-COVERED | minimal/static repo; governance overhead should be minimal until active |
| invest-simulater-app2 | EMPTY/BOOTSTRAP | initial-only/holding repo; bootstrap only if reactivated |
| auto-diary-GPT | EMPTY/BOOTSTRAP | empty; bootstrap on activation |
| cosmos-lab | EMPTY/BOOTSTRAP | empty; bootstrap on activation |
| invent-simulator-app | EMPTY/BOOTSTRAP | empty; bootstrap on activation |

## カバレッジ判断

- 2026-08-16以前: `.github/docs/repository-audit-2026-08-16.md` が24repoのdefault-branch history / non-default branch / Issue / Actions / docs / scriptsを横断確認済み。
- 2026-08-16以降: 新規/活発な主要repoをCURRENTで重点再確認。
- 古い小規模repoは今回も最近のcommit検索を行い、主な最新活動が2026-07以前であることを確認したものを旧監査でカバー扱い。
- 空repoにフルガバナンスを常設するのは管理コストが高いため、active化時にstarter kitを適用する候補。

## 未確認として残すもの

この調査はGitHub source/history/process miningであり、以下を「確認済み」とはしない。
- 各サービスのCURRENT production console settings全件
- Vercel/Supabase/Google/LINE等の全project設定
- 実端末での全アプリ再試験
- closed/private external systems outside GitHub

これらはcanonical rule作成時、該当ruleの実装/適用対象repoごとにfresh verificationする。

# PERSONAL AI DEVELOPMENT — EXECUTIVE SUMMARY CANDIDATE

> NOT CANONICAL / NOT AUTHORITATIVE
> ユーザー判断用の要約。決定後、正式なPersonal標準へ反映し本ファイルは削除する。

## 一言でいうと

これまでの個人開発は、

`AIに作らせる` → `実機で事故を見つける` → `そのproject内で改善する`

から、最近は

`要件/設計を固定する` → `小さく実装する` → `CI/独立review` → `実機/Production` → `知見を次へ持ち越す`

へ進化している。

次の段階は、これを各repoで毎回再発明せず、`ai-development-governance/personal` から新projectへ自動配布すること。

---

## 最重要10ポイント

### 1. AIの記憶よりGitHubのCURRENTを強くする
失敗例: stale handoff / old CURRENT / old HEAD。
対策: repo identity + actual HEAD + one CURRENT + one START-HEREを毎作業でfresh確認。

### 2. 「絶対」は文章だけでなくGitHub/CIに守らせる
失敗例: friend-app-v2でDesign approval待ちなのにimplementationが先行、main protectionなし。
対策: ruleset / branch protection / required checks / accepted design ID gate。

### 3. PRを大きくしすぎない
実例: Family Ops 203 commits、AI-App-Factory 173 commits。
対策: one reviewable work package。長くなる場合だけLong-running PR protocol。

### 4. 設計と実装の正を同じ変更で更新する
ManaEvoのcanonical-sync CIが成功例。
対策: source path -> owning requirement/design mapping + same-PR sync check。

### 5. CI GREENと「本当に使える」を分ける
Family Ops clean CIでは見えないproduction history bug、iPhone/PWA/provider差が多数。
対策: production-shaped fixture + real device/provider gate。

### 6. 最初に一本通してから量産する
Kids QuestのVertical Slice、12体Pilot→Scaleが成功例。
対策: E2E 1本→acceptance freeze→bulk generation/implementation。

### 7. 外部サービスの上限を開発要件として扱う
Vercel build/deploy limit、Gemini RPD、Vercel request body、GitHub schedule遅延など。
対策: Resource Budget Guard + Platform Limit Registry。

### 8. retry/timeout/画面失敗で同じ操作を二重実行しない
Family Ops duplicate LINE、AI-App-Factory triple request、auto-diary duplicate AI request。
対策: explicit acceptance boundary + idempotency + semantic state check。

### 9. UI/声/3Dの「良い感じ」を基準化する
friend-appで主観修正が連続。
対策: Visual/Voice Target + scorecard + hard fail + iteration cap。

### 10. 失敗を次projectの初期装備へ変える
Lessonを残すだけでは弱い。
対策: Project lesson -> Archetype -> Personal Common -> Template -> Script/CI。

---

## 今回の調査で特に価値が高かった成功パターン

### ManaEvo型
Design Review -> Canonical Promotion -> Implementation -> exact-head independent review -> real-device/release gate

高リスクな通常開発のGolden Path候補。

### Kids Quest型
Pilot -> acceptance freeze -> Scale + Vertical Slice First

大量content/asset/AI生成向けGolden Path候補。

### friend-app型
PoC -> measure -> bounded iteration -> product gate

音声/3D/未知技術向け。

### Family Ops型
Production phrase / production history / real providerからregressionを永久化

会話/DB/provider integration向け。

### AI-App-Factory型
untrusted input -> isolated job -> tests/security evidence -> Draft PR -> human merge

将来のProject Factory向け。

---

## 逆に廃止方向がよい過去パターン

- direct main pushを通常運用にする
- push-main helper
- CIがreview branchへsourceをauto-commitする
- runtime mutable stateをGit source historyへ定期commitする
- CURRENT/START-HERE/handoffを複数生かす
- giant AGENTSに全履歴/backlogを入れる
- provider/model/library semanticsを名前だけで信用する
- CI greenのみでProduction readyと言う
- real external operationを確認できないのに「成功」と表示する
- 他repoの成功コードをfile copyだけで移植する

---

## 正本化のおすすめ順

### Wave 1 — 事故を止める
1. Repository Identity + Fresh CURRENT
2. Authority / one CURRENT / one START-HERE
3. Branch protection / PR-only
4. Exact-head review
5. Design Gate
6. CI validation-only
7. Canonical sync

### Wave 2 — 品質とコストを守る
8. Production-shaped acceptance
9. Side-effect/idempotency/retry
10. Resource Budget Guard
11. Platform Limit Registry
12. PR/Long-running protocol

### Wave 3 — 開発を速くする
13. Vertical Slice / Pilot→Scale
14. Archetype standards
15. Starter kits
16. Project bootstrap generator
17. reusable pattern/package catalog

### Wave 4 — 自動で強くなる
18. lesson capture
19. knowledge promotion helper
20. stale-rule/stale-PR/stale-doc scheduled audit
21. project factory integration

---

## 今回おすすめする初期Decision

### DECISION A — 推奨 YES
`ai-development-governance/personal` をPersonal開発ルールの将来唯一のcanonical homeにする。
旧`.github/agent-rules`はmigration source/historyへ降格。

### DECISION B — 推奨 YES
active personal repoはPR-only + branch/ruleset protectionを原則にする。
Emergency direct-mainは人間が明示した時だけ例外。

### DECISION C — 推奨 YES
Vercel利用repoはmainだけProduction auto-deploy。PR/feature Previewはdefault off。

### DECISION D — 推奨 YES（risk-based）
高リスク変更だけ Design Review -> Accepted -> Implementation を必須化。
CSS文字修正等すべてにDesign PRを強制しない。

### DECISION E — 推奨 TRIAL
PR soft limit: 30 commits / 50 filesでsplit検討、100 commits超は原則split。
まず数projectで試し、例外条件を実績で調整。

### DECISION F — 推奨 YES
新規projectは「archetype選択 -> starter kit -> vertical slice」で開始するProject Factory方式へ寄せる。

---

## 最終目標

新しいアプリを作るとき、ユーザーが主に決めるのは「何を作るか」。

AI側は自動で:
- 適切なarchetype選択
- repo/branch/deploy guard
- AGENTS/CURRENT/requirements/design/test/CI scaffold
- 過去failureの防止control
- resource budget
- vertical slice
- review/release gate

を初期装備する。

各project終了後の事故・成功はgovernanceへ戻し、次projectでは最初から改善済みになる。

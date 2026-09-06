# Vercel Deployment Budget Guard — IDEA / NOT CANONICAL

## Trigger

Family Opsで、非main branchへのcommit/pushごとにVercel Preview Deploymentが大量発生し、短時間でdeployment回数を消費した経験からの候補ルール。

## Problem

AI開発ではcommit/push頻度が高く、feature branchやPR branchの自動Previewを許すと、人間開発よりはるかに速くVercel等の回数上限・無料枠・従量課金を消費する。

「テストのために細かくcommitする」ことと「毎commit外部deployする」ことを結びつけてはいけない。

## Candidate default rule for personal projects

原則:

- `main` → Production deploymentのみ自動許可
- PR branch Preview deployment → 原則禁止
- feature branch Preview deployment → 原則禁止
- design/docs branch Preview deployment → 禁止
- Previewが必要な場合 → 明示的なmanual/approved deploymentのみ
- docs-only変更 → deployしない

## Candidate controls

1. Vercel側のGit/deployment設定でmain以外の自動deployを止める。
2. GitHub Actions等からdeployする場合もbranch条件を機械的に限定する。
3. 「main-only deployment」をAGENTS/運用文書だけに書かず設定で強制する。
4. deployment historyを定期的に確認し、想定外branchのdeployを検知する。
5. deploy前に既存CIを通し、失敗が明らかなcommitを外部deployしない。
6. E2E/Previewが必要なproject typeだけ例外profileを持つ。

## Generalize beyond Vercel

この問題はVercel固有ではない。

対象候補:

- Vercel deployment count / build minutes
- Supabase Edge Function deploy / resource usage
- GitHub Actions minutes
- AI API token/cost
- image/video generation quota
- external CI/CD build quota
- test device/cloud browser quota

## Candidate governance concept

`Resource Budget Guard`

AIが高速に反復できるからこそ、外部有限資源を使うアクションはコード変更とは別Gateにする。

```text
local/source change
  -> local/unit validation
  -> repository CI
  -> deploy eligibility check
  -> external deployment
```

## Future automation ideas

- branch-based deploy guard
- docs-only deploy skip
- daily/weekly deployment counter
- quota threshold warning
- deployment anomaly detection
- cost budget file (project-specific)
- CIでdeployment policy自体を検証

## Open question

一律にPreview禁止ではなく、UI確認が重要なprojectでは「1 PRにつき1 Preview」「label付きのみPreview」などのprofileも検討する。

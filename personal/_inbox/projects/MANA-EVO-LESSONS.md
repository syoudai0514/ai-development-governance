# ManaEvo Lessons — NOT CANONICAL

## Observed development style

- 後半は `Design Review -> Canonical Promotion -> Implementation -> fail-first acceptance -> WebKit/実機gate` が明確。
- D-ID単位でdesign decisionを管理。
- 画像/asset量産ではPilotとQAを挟んだ。

## What worked well

- 設計PRと実装PRを分けた。
- product decisionが未決定な箇所で勝手に永久仕様を発明せず、fail-closedにした例がある。
- actual runtime call graphを追うことで、本当に直すべきexactly-once問題等を特定できた。
- CIがreview対象を勝手に変える問題から、validation-only CIの重要性を学んだ。
- iPhone/iPad/WebKitなど実利用端末をacceptanceへ含めた。
- 238体等の大量asset作業でFAST LANE/Pilot/QAの考え方が発達した。

## Problems observed

- ルートに複数のSTART-HERE/HANDOFF/旧status資料が残り、repository information architectureが複雑化。
- Bundle単位のPRが大きくなる傾向。
- reviewerが関数名や近いdomainだけを見て誤ったscope判断をしそうになった。
- asset量産は後戻りコストが大きく、品質基準確定前に規模を広げると危険。

## Candidate improvements

- `AGENTS -> CURRENT -> canonical START-HERE` の入口を一本化する。
- Accepted Design IDをimplementation PRの必須metadataにする。
- review checklistにactual runtime call graph traversalを入れる。
- large asset/content generationは必ずPilot acceptanceを通す。
- CIは検証結果を勝手にsourceへcommitしない。source mutationは明示workflowに分離する。
- Bundle内でもwork package単位にintermediate acceptanceを設ける。

## Promotion candidates

Personal common候補:

- Design Review -> Canonical Promotion -> Implementation
- fail-closed for undecided product behavior
- actual runtime call graph review
- Pilot -> QA -> Scale
- validation-only CI

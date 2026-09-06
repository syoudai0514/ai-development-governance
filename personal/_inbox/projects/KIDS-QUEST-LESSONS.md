# Kids Quest Lessons — NOT CANONICAL

## Observed development style

- 早い段階からAGENTS / CLAUDE / PR Template / Issue Form等を整備。
- 大量生成前にPilot、最小Vertical Slice、snapshot/fingerprint等の機械検証を導入。

## What worked well

- 重要な対象集合・順序・構成をsnapshot/hashで固定し、件数だけ合う誤実装を防いだ。
- 1000体等の大量生成で、少数Pilot -> QA -> Scaleの流れを使った。
- 学習 -> Battle -> Capture -> Growth -> Evolutionの最小縦切りを先に完成させた。
- 高性能モデルで基準作成、別モデルで量産するmodel routingを試した。

## Problems observed

- 別project用の変更が誤って入るcross-repository contaminationが発生し、revertが必要になった。
- AIルールがあっても、actual repo/remote/branchを機械的に確認しなければ防げない。
- 大量生成系は品質基準が曖昧なままscaleすると手戻りが極端に増える。

## Candidate improvements

- expected repo/remote/branch guardをcommit/push前に機械実行する。
- project identity fileを用意し、AI preflightで照合する案。
- Vertical Slice FirstをPersonal共通標準候補にする。
- Pilot acceptanceを通過しない限りlarge-scale generationを許可しない。
- machine-readable invariant/snapshotの適用対象をrequirementsから選定する。

## Promotion candidates

Personal common候補:

- repository isolation guard
- Vertical Slice First
- Pilot -> Scale
- immutable snapshot/fingerprint
- model routing with quality gate

# friend-app-v2 Lessons — NOT CANONICAL

## Observed development style

- 不確実性の高い技術をPoC -> 実機 -> 小PRで高速反復。
- 音声、iPhone audio、realtime、3D/GLBなど、机上設計だけでは判断しにくい領域を実測で進めた。

## What worked well

- 実機でfirst audio latencyや再生可否を確認し、改善を積み上げた。
- `turn_to_first_audio` のようなUX直結指標を使った。
- PoCでprovider/方式を比較し、より良いruntime経路へ切り替えた。
- 3D/見た目の品質でVisual TargetやGate方式へ進み始めた。
- `CLAUDE.md -> AGENTS.md` の共通入口構造を採用した。

## Problems observed

- 「APPROVE FOR IMPLEMENTATION後に実装」と書いたdesign gateが、工程上は強制されず実装が先行した例。
- 他projectの成功方式を移植した際、runtime patch/config/前提の一部が抜け、同等結果にならなかった。
- 主観品質の3D/音声はコードテストだけではacceptanceできず、反復回数が増えた。
- AGENTS.mdがknowledge base化し肥大化すると、入口としての役割が弱くなる。

## Candidate improvements

- implementation PRにAccepted Design IDを必須化し、design gateをCIで強制する。
- 他projectからの再利用はcopyではなくversioned pattern/package/checklistとして移植する。
- 主観品質はVisual/Audio Target、scorecard、hard fail、max iterationを先に決める。
- PoCの出口条件を「動いた」ではなく latency / quality / cost / device compatibilityで定義する。
- AGENTSは薄いrouterにし、詳細知識を専用docsへ分離する。

## Promotion candidates

Personal common/archetype候補:

- PoC Gate
- measurable UX metrics
- design gate enforcement
- reusable pattern packaging
- subjective-quality scorecard
- thin AGENTS router

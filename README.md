# AI Development Governance

このリポジトリは、AIを利用したソフトウェア開発の知見・標準・ガバナンスを蓄積するためのリポジトリです。

## Two independent tracks

- `personal/` — 個人開発。複数アプリで再利用する共通ノウハウ、アプリ種類別ノウハウ、個別プロジェクト固有ノウハウを蓄積する。
- `enterprise/` — 会社・大規模開発向け。個人開発とは独立したガバナンス、標準工程、統制、品質保証、監査・証跡を検討する。

Personal と Enterprise の共通正本は作らない。必要な知見が似ていても、それぞれの文脈で独立して正本化する。

## Important: `_inbox` is NOT canonical

`personal/_inbox/` と `enterprise/_inbox/` はアイデア、観測事実、失敗例、改善案を失わず保存するための一時置き場です。

- `_inbox` の内容は **NOT CANONICAL / NOT AUTHORITATIVE**。
- `_inbox` を仕様・標準・CURRENTとして扱わない。
- 内容が整理され、正式な標準・playbook・pattern・control等へ昇格したら、対応する `_inbox` の記述は削除する。
- `_inbox` 内で矛盾する案が併存していてもよい。正本化時に解決する。
- AIは `_inbox` を実装判断の根拠にしてはならない。

現時点では、完成した体系を急いで作るより、実プロジェクトから得たネタを大量に保存し、後から分類・正本化することを優先する。

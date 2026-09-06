# PERSONAL REPOSITORY MINING PLAN — 2026-09-07

> NOT CANONICAL / NOT AUTHORITATIVE
> この文書は正本化前の調査台帳。正本へ昇格後は削除する。

## 目的
既存の個人GitHubプロジェクト履歴から、実際の進め方、問題、改善、未改善、成功パターン、再利用候補、自動化候補を網羅的に抽出し、次の個人開発を毎回楽にする知識資産へ変換する。

## 調査工程（進捗母数=8）
1. 全repo棚卸し・深掘り対象選定
2. 現状構造・入口・branch/PR/CIガバナンス確認
3. 主要PR/Issue/commit履歴確認
4. 事故・失敗・手戻りパターン抽出
5. 既に効いた改善・成功パターン抽出
6. personal/common / archetype / project 固有へ分類
7. template / playbook / script / CI 自動化候補へ変換
8. 重複整理・優先度付け・ユーザー向けサマリー作成

## 深掘り対象
- family-ops
- mana-evo
- friend-app-v2
- kids-quest
- AI-App-Factory
- friend-app（旧版）
- auto-diary
- stock-checker
- 初期/小規模群から代表例（投資系、Claude生成branch型、space/game系）

## 全件確認対象
所有repo全件。空repoも含め、少なくとも default branch / size / PR履歴の有無 / 開発パターンを確認する。

## 分類先
- personal/common: 個人開発全体に効くもの
- personal/archetypes: PWA / game / AI/realtime / data-heavy / prototype 等の種類別
- personal/projects: 特定repoだけの知見
- personal/_inbox: 正本化前の候補

## 原則
- 過去会話ではなくCURRENT GitHubを優先する。
- PR本文や古いhandoffは履歴として読み、CURRENT状態と混同しない。
- 「良かった話」だけでなく、終わったが改善すべき点も残す。
- lessonは可能なら standard -> template -> script/CI まで昇格候補を付ける。

## 現在進捗
12.5% (1/8)

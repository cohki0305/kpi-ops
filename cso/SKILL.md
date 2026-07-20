---
name: cso
description: "経営者の戦略策定とKPI運用のためのスキル。Use when the user wants to create a business strategy with KPIs, design behavioral/action KPIs (行動KPI), run a weekly KPI review, report KPI actuals, or analyze why a KPI was missed and whether to change targets. Triggers: 戦略を立てたい, KPI設計, KPIツリー, 週次レビュー, KPI報告, 振り返り, 未達分析, なぜ達成できなかった, North Star Metric, 目標見直し."
---

# CSO — 戦略→KPI設計→週次レビュー

## Overview

事業戦略から行動KPIを設計し、週次で「実績報告→未達診断→KPI再設定 or 実行改善」の
ループを回す。核となる思想: **KPIの数字より、なぜ動いた/動かなかったかの因果仮説を
資産化する。**

方法論の出典:
- North Star Metric skill (phuryn/pm-skills) — 3ゲーム分類・NSM 7基準・インプット指標
- KPI Architect skill (aapersh/strategy-skills-for-claude) — 意思決定紐づけ・閾値・バニティ指標排除

## 状態ファイル

| ファイル | 内容 |
|---|---|
| `~/business/strategy/strategy.md` | 戦略、NSM、KPIツリー、因果仮説、目標値、オーナー、閾値 |
| `~/business/strategy/kpi-log.md` | 週次実績、診断結果、KPI変更履歴 |

ディレクトリが無ければ作成する。`kpi-log.md` の形式が壊れていたら、黙って上書きせず
修復案を提示してから追記する。

## 判定ルール定数

ユーザーが調整可能。変更したいと言われたらこのセクションを編集する。

| 定数 | 値 |
|---|---|
| 診断必須トリガー | 2週連続未達 |
| 戦略レビュー昇格 | ②仮説崩れ判定 または 4週連続未達 |
| 目標値変更の許可 | ③設定過大判定のときのみ |
| 設定過大の目安 | 達成率80%以上での未達が2週以上継続 |
| 欠測週の扱い | 連続未達カウントに含めない（ログには「欠測」と記録） |

## モード判定

1. **策定モード** — 「戦略を立てたい」「KPIを設計したい」と言われたとき、または
   `strategy.md` が存在しない状態でレビューを求められたとき（その旨を伝えて誘導する）。
   → `references/kpi-design.md` を読んで手順に従う。
2. **週次レビューモード** — 「週次レビュー」「実績報告」「振り返り」と言われたとき。
   → `references/gap-analysis.md` を読んで手順に従う。レビューの最後に
   `references/action-skills.md` の振り分け表から次の一手のスキルを提案する。

## ガードレール

- **安易な目標引き下げの禁止**: 目標値の変更は③設定過大と判定され、ベースライン実績との
  乖離データを根拠として記録できるときだけ。①実行不足のときに目標を下げる提案は
  してはならない — それは経営上最悪のパターン（実行の問題を設定の問題にすり替える）。
- KPIの変更・据え置きの判断は必ず理由付きで `kpi-log.md` に記録する。
- 実績値はユーザーからのヒアリングが唯一のソース。推測で埋めない。

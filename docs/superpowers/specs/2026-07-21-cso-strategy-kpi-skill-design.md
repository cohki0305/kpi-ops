# cso スキル設計書 — 戦略→KPI設計→週次レビューのループ

日付: 2026-07-21
ステータス: ユーザーレビュー待ち

## 目的

経営者（koki さん自身の事業）向けに、事業戦略から行動KPIを設計し、週次で
「実績報告 → 未達の原因分析 → KPI再設定 or 実行改善」のループを回す Claude スキルを作る。

最重要要件: **KPIが未達だったとき、なぜ未達なのかを分析し、根拠を持ってKPI設定を
変えられる（あるいは変えない判断ができる）こと。**

## 決定済み要件

| 項目 | 決定 |
|---|---|
| 運用形態 | 継続運用型。戦略・KPI・実績をファイルに永続化 |
| 実績入力 | 週次レビュー時に口頭報告、Claude がヒアリングして追記 |
| レビュー周期 | 週次 |
| スキル名 | `cso`（~/.claude/skills/cso/ の空ディレクトリを使用） |
| 状態ファイル置き場 | `~/business/strategy/` |
| 参考スキル | phuryn/pm-skills の north-star-metric、aapersh/strategy-skills-for-claude の kpi-architect |

## ファイル構成

```
~/.claude/skills/cso/
├── SKILL.md              # トリガー定義、モード判定、全体フロー
└── references/
    ├── kpi-design.md     # 策定モードの方法論（NSM + KPI Architect の日本語合成、出典リンク付き）
    ├── gap-analysis.md   # 未達診断の4分類と判定ルール
    └── action-skills.md  # 対策 → インストール済みスキルへの振り分け表

~/business/strategy/       # 状態ファイル（スキル本体とは分離したデータ資産）
├── strategy.md           # 戦略、NSM、KPIツリー、因果仮説、目標値、オーナー、閾値
└── kpi-log.md            # 週次実績ログ、診断結果、KPI変更履歴
```

## モード1: 策定モード

トリガー例: 「戦略を立てたい」「KPIを設計したい」、または `strategy.md` が存在しない状態でレビューを依頼されたとき。

フロー（north-star-metric + kpi-architect の合成）:

1. **事業ゲーム分類**: Attention / Transaction / Productivity のどれか（north-star-metric 由来）
2. **North Star Metric の特定**: 7基準（理解しやすい・顧客中心・持続的価値・ビジョン整合・
   定量的・行動可能・先行指標）で検証（north-star-metric 由来）
3. **行動KPIへの分解**: NSM を動かす3〜5個のインプット指標をドライバーツリーで定義。
   各行動KPI→成果KPIの**因果仮説を必ず文章で記録**する（未達診断②の判定に必須）
4. **意思決定への紐づけ**: 各KPIに「どの意思決定を支えるか」「オーナー」「閾値」「週次目標値」を
   設定。バニティ指標は排除（kpi-architect 由来）
5. `strategy.md` に保存

## モード2: 週次レビューモード

トリガー例: 「週次レビュー」「KPIの報告」「先週の振り返り」。

1. `strategy.md` と `kpi-log.md` を読み、今週の対象KPIを列挙
2. 実績をヒアリング（KPIごとに実績値を確認）、`kpi-log.md` に追記
3. 未達KPIごとに**4分類診断**:

| 分類 | 症状 | 処方 |
|---|---|---|
| ① 実行不足 | 行動量そのものが未達 | KPIは変えない。実行を阻む要因（時間・優先度・手段）を特定して除去 |
| ② 仮説崩れ | 行動KPIは達成したのに成果KPIが動かない | 因果仮説の誤り。KPIツリーと戦略を見直す |
| ③ 設定過大 | 継続的に僅差未達（目安: 達成率80%以上での未達が2週以上継続）、ベースライン実績と乖離 | 目標値を実績ベースで再設定 |
| ④ 外部要因 | 季節・競合・市場変化 | 一時的なら据え置き、構造変化なら戦略レビュー |

4. **判定ルール**（数値は SKILL.md 冒頭に定数として書き、ユーザーが調整可能にする）:
   - 2週連続未達 → 4分類診断を必須実行
   - ③判定のときのみ目標値の変更を許可。変更時はベースライン実績との乖離データを根拠として記録
   - ②判定 または 4週連続未達 → 戦略レビュー（策定モードの再実行）に昇格
5. 診断結果・決定事項・KPI変更とその理由を `kpi-log.md` に記録
6. **次の一手の提案**: `action-skills.md` の振り分け表に従い、対策に使えるインストール済み
   スキルを提案（例: CVR起因→`page-cro`、価格起因→`pricing-strategy`、
   行動量不足→`marketing-ideas`/`social-content`、仮説検証→`ab-test-setup`、
   計測不備→`analytics-tracking`）

## ガードレール

- **安易な目標引き下げの禁止**: 目標値変更は③判定の根拠データがあるときだけ。
  ①実行不足での目標引き下げ提案は明示的に禁止する
- 報告が飛んだ週は「欠測」として記録し、連続未達カウントに含めない
- `kpi-log.md` の形式が壊れていた場合は、修復案を提示してから追記する（黙って上書きしない）

## スコープ外

- 外部ツール（GA4等）からの実績自動取得（将来拡張。まず口頭報告で運用）
- 月次・四半期レビューの別モード（週次レビューの中で必要時に戦略レビューへ昇格する設計で代替）
- 判定ルールの決定論的スクリプト化（KPI数が少ないうちは不要）

## 出典

- North Star Metric skill: https://github.com/phuryn/pm-skills （pm-marketing-growth/skills/north-star-metric）
- KPI Architect skill: https://github.com/aapersh/strategy-skills-for-claude （skills/05-risk-performance-and-value-governance/kpi-architect.md）

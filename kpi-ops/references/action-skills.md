# 次の一手 → スキル振り分け表

週次レビューの診断結果から、深掘りに使えるスキルを提案する。
表に無い症状は提案を省略してよい。

| 未達の中身・症状 | 提案するスキル |
|---|---|
| LP・サイトのCVRが低い | page-cro, btob-site-checklist |
| フォーム・登録の離脱 | form-cro, signup-flow-cro |
| 価格・収益構造に課題 | pricing-strategy, paywall-upgrade-cro |
| リード獲得の行動量・打ち手不足 | marketing-ideas, social-content, paid-ads |
| 検索流入が弱い | seo-audit, programmatic-seo |
| メール・ナーチャリングが弱い | email-sequence |
| 因果仮説を検証したい | ab-test-setup |
| そもそも計測できていない | analytics-tracking |
| オンボーディング・継続率が低い | onboarding-cro |
| 講座・商品のローンチ | launch-strategy, pocket-launch |
| 行動量の上限が工数・人手（自動化余地あり） | （表外）AI効率化の提案 — 下記セクション参照 |

## 提案の仕方

「この未達は [症状] なので、`[スキル名]` で深掘りできます。やりますか？」の1文で提案。
押し付けない。レビューの主目的は診断と記録であり、スキル起動は次のアクション。

## スキルが未インストールの場合

提案前に、そのスキル（または下の配布元表にある同等スキル）が利用可能かを確認する
（`~/.claude/skills/` の実在確認、または利用可能スキル一覧）。

- **インストール済み** → そのまま提案・起動する。
- **未インストールで配布元表にある** → 提案文に「未インストールなのでインストールして
  実行します」と添える。ユーザーが提案を受けたら、先にインストールしてから起動する:

  ```bash
  npx skills add <配布元> --skill <配布元でのスキル名> -g -y
  ```

  配布元でスキル名が改名されていることがある。exact名で失敗したら
  `npx skills add <配布元> --list` で現行名を確認して読み替える。
- **未インストールで配布元表にも無い**（ローカル専用・環境固有のスキル）→ 提案しない。

### 配布元表

配布元: [coreyhaines31/marketingskills](https://github.com/coreyhaines31/marketingskills)

| このスキルでの呼び名 | 配布元でのスキル名 |
|---|---|
| page-cro | cro |
| form-cro | cro |
| signup-flow-cro | signup |
| pricing-strategy | pricing |
| paywall-upgrade-cro | paywalls |
| marketing-ideas | marketing-ideas |
| social-content | social |
| paid-ads | ads |
| seo-audit | seo-audit |
| programmatic-seo | programmatic-seo |
| email-sequence | emails |
| ab-test-setup | ab-testing |
| analytics-tracking | analytics |
| onboarding-cro | onboarding |
| launch-strategy | launch |

## AI効率化の提案

未達診断が①実行不足で原因が工数・人手の上限にある場合、
または③設定過大でベースラインが工数上限に起因する場合は、
「その行動をAIで自動化・半自動化して行動量の上限を引き上げられないか」を必ず検討する。
①では目標引き下げは引き続き禁止 — 自動化は目標を守るためのレバーであり、
引き下げの前段階ではない。
対象業務をヒアリングし、スキル化・定期実行（schedule / loop）・
パイプライン化（pipeline / handoff-instructions）など実装可能な効率化案を提案する。
これらは環境固有の機能・スキルなので、利用可能なものだけ提案する。
効率化で行動KPIの上限が変わったら、策定モードで strategy.md の目標値と因果仮説を
更新する — AIによる効率化は戦略の前提を変える。

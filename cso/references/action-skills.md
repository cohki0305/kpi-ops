# 次の一手 → スキル振り分け表

週次レビューの診断結果から、深掘りに使えるインストール済みスキルを提案する。
表に無い症状は提案を省略してよい。存在しないスキルは提案しない
（提案前に ~/.claude/skills/ に実在するか確認する）。

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

## 提案の仕方

「この未達は [症状] なので、`[スキル名]` で深掘りできます。やりますか？」の1文で提案。
押し付けない。レビューの主目的は診断と記録であり、スキル起動は次のアクション。

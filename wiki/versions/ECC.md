# SAP ECC 6.0（移行元）— 原価センタ関連の留意点

[← バージョン比較](README.md) | [← Home](../Home.md)

S/4HANA への移行時に影響する ECC 側の仕様をまとめています。

## データモデル

| 項目 | ECC 6.0 | S/4HANA での扱い |
|---|---|---|
| 実績明細 | COEP | ACDOCA（COEP は互換ビュー） |
| 合計 | COSP / COSS | 互換ビュー（旧データ COSP_BAK / COSS_BAK） |
| 原価要素 | CSKA / CSKB（KA01 等） | G/L 勘定に統合 |
| 原価要素のデフォルト勘定設定 | 原価要素マスタ | OKB9（TKA3A） |
| FI-CO 照合 | 照合元帳 / リアルタイム統合（新 G/L） | 不要（Universal Journal） |

→ 詳細: [1. ECC と S/4HANA の違い](../01_ECC_vs_S4HANA.md)

## ECC 側で事前に確認すべきこと

1. **COEP/COSP/COSS を直接 SELECT しているアドオン**の一覧化（Note 2185026）
2. **統計内部指図＋置換による原価センタ予算管理**（Note 68366 / 101030）の有無 → S/4HANA 1909 以降の標準 AVC へ置換可否
3. **FI-GL 計画／従来型 PCA 計画**の利用有無 → 再有効化 Note（2253067 / 2345118 / 2313341）
4. **原価センタ階層（Set）**の運用（標準階層・代替階層）→ S/4HANA でのグローバル階層化方針
5. FI/CO 整合性: **FIN_CORR_RECONCILE**（Note 2755360）、Finance 事前チェック（Note 2245333）
6. 管理領域通貨と会社コード通貨の関係、過去の通貨換算・SLO プロジェクト履歴

→ 詳細: [5. ECC からの移行](../05_Migration_from_ECC.md)

## ECC と S/4HANA で共通の運用ルール

- 転記後のマスタ変更条件（Note 62716、KS042 / KS134） → [2.2](../02_Master_Data_and_Hierarchy.md)
- 従来型配賦（KSU5 / KSV5）は S/4HANA On-Premise でも利用可

## 参考

- [SAP Controlling (CO) Sub modules comparison from ECC to S/4 HANA](https://community.sap.com/t5/enterprise-resource-planning-blog-posts-by-members/sap-controlling-co-sub-modules-comparison-from-ecc-to-s-4-hana/ba-p/13380564) `[Blog]`
- [Cost Budget and Availability Control on SAP ECC, and S/4HANA On premise](https://community.sap.com/t5/enterprise-resource-planning-blog-posts-by-sap/cost-budget-and-availability-control-on-sap-ecc-and-s-4hana-on-premise/ba-p/13440115) `[Blog]`
- [Migrating data from SAP ECC to SAP S/4HANA with the migration cockpit](https://community.sap.com/t5/enterprise-resource-planning-blog-posts-by-members/migrating-data-from-sap-ecc-to-sap-s4-hana-with-the-migration-cockpit/ba-p/13676908) `[Blog]`

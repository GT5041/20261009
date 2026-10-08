# 3. 計画と予算管理

[← Home](Home.md)

---

## 3.1 原価センタ計画の選択肢

| 方式 | 対象バージョン | 格納先 | 備考 |
|---|---|---|---|
| 従来計画（KP06 / KP26 等） | ECC / S/4HANA 全バージョン（On-Premise） | COEJ / COSP_BAK / COSS_BAK 系 | S/4HANA ではメニューから削除されたがトランザクションは利用可能 |
| SAP BPC for S/4HANA（Embedded BPC / Integrated Business Planning） | S/4HANA 1511〜 | ACDOCP / BW 内 | Note **2081400**（マスタ Note） |
| 新計画（ACDOCP）＋ Fiori「Import Financial Plan Data」 | S/4HANA 1610 以降（Cloud 含む） | **ACDOCP** | Public Cloud では KP06 は利用不可、本アプリが代替 |
| SAP Analytics Cloud（SAC）連携 | S/4HANA 新しめのリリース | ACDOCP | 推奨方向性 |

- KP06 はメニューから削除されたが利用可能
  - 出典: [Cost Center planning on S4 HANA 1610](https://community.sap.com/t5/enterprise-resource-planning-q-a/cost-center-planning-on-s4-hana-1610/qaq-p/413340) `[Q&A]`
  - 出典: [Cost Center Planning in S/4HANA](https://community.sap.com/t5/enterprise-resource-planning-q-a/cost-center-planning-in-s-4hana/qaq-p/11801819) `[Q&A]`
- Public Cloud では KP06 → 「Import Financial Plan Data」
  - 出典: [Tcode KP06 is not available in S/4HANA Public Cloud](https://community.sap.com/t5/enterprise-resource-planning-q-a/tcode-kp06-is-not-available-in-s-4hana-public-cloud/qaq-p/13713797) `[Q&A]`
- 計画オプション全体像
  - 出典: [Financial Planning Options in S/4HANA (Updated for the S/4HANA 2022 release)](https://blogs.sap.com/2020/12/01/financial-planning-options-in-s-4hana-2020-release) `[Blog]`
  - 出典: [How to do Activity-Independent and Activity-Dependent Cost Planning in S/4HANA](https://community.sap.com/t5/enterprise-resource-planning-q-a/how-to-do-activity-independent-and-activity-dependent-cost-planning-in-s/qaq-p/12235794) `[Q&A]`
  - 出典: [Cost Center Planning (SAP Help)](https://help.sap.com/docs/SAP_S4HANA_ON-PREMISE/5e23dc8fe9be4fd496f8ab556667ea05/fc21a4514f672f09e10000000a441470.html) `[Help]`

### ECC からの移行時の注意（計画）

| ポイント | ソース |
|---|---|
| S/4HANA で使えなくなるのは **FI-GL 計画と利益センタ計画（従来型）**。原価センタ計画（KP06）は対象外 | Note 2081400、[What you should know about controlling in SAP S/4HANA (Part 2)](https://community.sap.com/t5/enterprise-resource-planning-blog-posts-by-sap/what-you-should-know-about-controlling-in-sap-s-4hana-part-2/ba-p/13473973) `[Blog]` |
| FI-GL 計画の再有効化 | Note **2253067** |
| 従来型利益センタ計画の再有効化 | Note **2345118**, **2313341** |
| 従来型 PCA を再有効化した場合、**KP06 による統合計画で EC-PCA に計画データが作成されない** | KBA [2996269](https://userapps.support.sap.com/sap/support/knowledge/en/2996269) |
| BPC の計画を KP06/KP26 で見るには従来合計テーブルへのリトラクトが必要。KP06 入力値は ACDOCP ではなく COEP 系に書かれる | [Cost Center Planning in S/4HANA](https://answers.sap.com/questions/12584778/cost-center-planning-in-s4hana.html) `[Q&A]` |

## 3.2 Budget Availability Control（予算有効性管理）

### 経緯

| バージョン | 状況 |
|---|---|
| ECC 6.0 / S/4HANA 〜1809 | 原価センタ向け標準 AVC なし。**統計内部指図＋置換**で代替（Note **68366** Active Availability Control for Cost Centers、Note **101030** FAQ: Availability Control for Cost Centers） |
| **S/4HANA 1909** | **原価センタの Budget Availability Control が標準機能として導入**（Cloud 先行機能の On-Premise 化） |
| **S/4HANA 2020** | 原価センタ間の**予算移管用 Fiori アプリ**が追加 |

- 出典: [Cost Budget and Availability Control on SAP ECC, and S/4HANA On premise](https://community.sap.com/t5/enterprise-resource-planning-blog-posts-by-sap/cost-budget-and-availability-control-on-sap-ecc-and-s-4hana-on-premise/ba-p/13440115) `[Blog]`
- 出典: [Budget Availability Control for Cost Centers in SAP S/4HANA 1909](https://community.sap.com/t5/enterprise-resource-planning-blog-posts-by-sap/budget-availability-control-for-cost-centers-in-sap-s-4hana-1909/ba-p/13424386) `[Blog]`
- 出典: [S/4HANA 1909 Cost Centres Budget Availability Control](https://blogs.sap.com/2019/11/06/s4hana-1909-cost-centres-budget-availability-control/) `[Blog]`
- 出典: [Budget Availability Control (SAP Help, On-Premise)](https://help.sap.com/docs/SAP_S4HANA_ON-PREMISE/5e23dc8fe9be4fd496f8ab556667ea05/30cb8deab95e4d8c9420aeabb2ae496f.html) `[Help]`

### 設定・運用のポイント

1. **Budget Availability Control Profile** を作成し、対象原価センタに割当てる（**Fiori「Manage Cost Centers」でのみ割当可能**、KS01/KS02 では不可）。
2. 予算は計画カテゴリに格納（例: **BUDGET02** = Cost Center Budgeting）。
3. 購買依頼・購買発注を予算消費として扱うには、原価センタマスタの **Lock Commitment Updates** を外す。
4. **貸借対照表勘定には AVC は効かない**。
5. コミットメントはレポート用の情報であり、AVC を設定しない限り発注をブロックしない。

- 出典: [S/4HANA 1909 Cost Centres Budget Availability Control – Configuration](https://blogs.sap.com/2020/01/02/s-4hana-1909-cost-centres-budget-availability-control-configuration/) `[Blog]`
- 出典: [Cost Center – Budget Availability Control](https://community.sap.com/t5/enterprise-resource-planning-blog-posts-by-members/cost-center-budget-availability-control/ba-p/13752625) `[Blog]`
- 出典: [Cost Center Commitments in SAP S/4HANA Cloud](https://community.sap.com/t5/enterprise-resource-planning-blog-posts-by-sap/cost-center-commitments-in-sap-s-4hana-cloud/ba-p/13495753) `[Blog]`
- 出典: [Cost Center Budgeting control per GL account](https://community.sap.com/t5/enterprise-resource-planning-q-a/cost-center-budgeting-control-per-gl-account/qaq-p/12303342) `[Q&A]`
- 出典: [Budget Availability Control for Cost Centers (SAP Help, Cloud)](https://help.sap.com/docs/SAP_S4HANA_CLOUD/bd39d476d1e34b48afed98759286efd6/787e0e992eb04c688730b1032f49a3f6.html) `[Help]`

> **ECC からの移行時**: ECC で統計内部指図＋置換による AVC 代替を実装している場合、S/4HANA 1909 以降では標準 AVC への置き換えを検討できる。ただし統計指図の既存データ・置換ルールの廃止計画が必要。

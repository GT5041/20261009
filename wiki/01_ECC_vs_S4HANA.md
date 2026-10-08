# 1. ECC と S/4HANA の違い（データモデル）

[← Home](Home.md)

適用バージョン: **S/4HANA 1511 以降の全バージョン**（ECC からの移行時に必須の理解事項）

---

## 1.1 Universal Journal（ACDOCA）への統合

S/4HANA では FI と CO の実績明細が **ACDOCA（Universal Journal）** に統合されました。原価センタへの実績転記（一次原価・二次原価とも）は ACDOCA に 1 レコードとして記録されます。

- Universal Journal により二次原価要素も G/L 勘定となり、すべての CO 転記が Universal Journal に記録されるため、**照合元帳（Reconciliation Ledger）や FI-CO リアルタイム統合の概念は不要**になった。
  - 出典: [What you should know about controlling in SAP S/4HANA (Part 1)](https://community.sap.com/t5/enterprise-resource-planning-blog-posts-by-sap/what-you-should-know-about-controlling-in-sap-s-4hana-part-1/ba-p/13443867) `[Blog]`
  - 出典: [What you should know about controlling in SAP S/4HANA (Part 2)](https://community.sap.com/t5/enterprise-resource-planning-blog-posts-by-sap/what-you-should-know-about-controlling-in-sap-s-4hana-part-2/ba-p/13473973) `[Blog]`

| 値タイプ (WRTTP) | 意味 | ECC 格納先 | S/4HANA 格納先 |
|---|---|---|---|
| 04 | 実績 | COEP / COSP / COSS | **ACDOCA** |
| 11 | 統計実績 | COEP / COSP / COSS | **ACDOCA**（互換アクセス用に COEP も保持） |
| その他（01 計画 等） | 計画・コミットメント等 | COEP / COSP / COSS | COEP / **COSP_BAK** / **COSS_BAK**（＋新計画は ACDOCP） |

## 1.2 互換ビュー（Compatibility Views）

ECC の COEP / COSP / COSS / COVP は S/4HANA では**同名の互換ビュー**として提供され、旧テーブル構造を実行時に ACDOCA 等から算出します。

| ソース | 種別 | ポイント |
|---|---|---|
| SAP Note **2185026** – Compatibility views COSP, COSS, COEP, COVP: How do you optimize their use? | Note | カスタムプログラムは値タイプ 04 / 11 について互換ビューではなく **ACDOCA を直接参照**するよう改修を推奨 |
| SAP Note **3127377** | Note | SE16 で COSP/COSS/COEP/COVP を参照した際のメモリ割当エラー |
| KBA **[3730708](https://userapps.support.sap.com/sap/support/knowledge/en/3730708)** | KBA | COSS の WRTTP 04 は ACDOCA から読まれる。調査は **V_COSS_ORI** と ACDOCA で行う |
| [replacing coep coss cosp with acdoca](https://answers.sap.com/questions/455932/replacing-coep-coss-cosp-with-acdoca.html) | Q&A | アドオンの置換方針に関する議論 |
| [Some interesting facts of compatibility views in SAP BW/4HANA and SAP S/4HANA](https://groups.community.sap.com:443/t5/technology-blogs-by-sap/some-interesting-facts-of-compatibility-views-in-sap-bw-4hana-and-sap-s/bc-p/13542678) | Blog | 互換ビューは単純な 1:1 参照ではなく、複雑なロジックでリダイレクトしている |

> **移行時の注意**: 互換ビューはマッピングが複雑で、単純なテーブルアクセスに比べ**性能劣化のリスクが大きい**。ECC で COEP/COSP/COSS を直接 SELECT しているアドオン・レポートは、移行プロジェクトで洗い出し・改修対象にする。

## 1.3 原価要素の G/L 勘定への統合

| ECC 6.0 | S/4HANA |
|---|---|
| 原価要素は独立マスタ（KA01/KA02/KA03/KA06、テーブル CSKA/CSKB） | 原価要素は **G/L 勘定マスタ（FS00）** の一部。G/L マスタに **原価要素タイプ** 項目が追加 |
| 原価要素は期間依存で管理 | **期間依存の管理は不可** |
| 原価要素マスタにデフォルト勘定設定（原価センタ／指図） | デフォルト勘定設定は **TKA3A（OKB9）** に移行 |
| 原価要素グループ（KAH1） | 原価要素グループは引き続き利用可能 |

- KA01/KA02/KA03/KA06 は 1511 で利用不可。
  - 出典: [SAP Controlling (CO) Sub modules comparison from ECC to S/4 HANA](https://community.sap.com/t5/enterprise-resource-planning-blog-posts-by-members/sap-controlling-co-sub-modules-comparison-from-ecc-to-s-4-hana/ba-p/13380564) `[Blog]`
  - 出典: [Simplification List for SAP S/4HANA, on-premise edition 1511 (PDF)](https://help.sap.com/doc/pdfa4322f56824ae221e10000000a4450e5/1511%20002/en-US/SIMPL_OP1511_FPS02.pdf) `[Help]`
  - 出典: [can cost elements have validity date in s4](https://answers.sap.com/questions/346494/can-cost-elements-have-validity-date-in-s4.html) `[Q&A]`
  - 出典: [Cost element for balance sheet GL in S4 HANA](https://answers.sap.com/questions/487306/cost-element-for-balance-sheet-gl-in-s4-hana.html) `[Q&A]`

## 1.4 原価センタマスタ自体の変更

- 原価センタマスタのテーブル **CSKS / CSKT は ECC から構造的に継続**しており、KS01/KS02/KS03 も引き続き利用可能。
  - 出典: [How to create a Cost Center? (SAP Help – Support Content)](https://help.sap.com/docs/SUPPORT_CONTENT/ficontrolling/3361880651.html) `[Help]`
- 一方で、**Budget Availability Control プロファイルの割当**など、S/4HANA で追加された一部の属性は **Fiori アプリ「Manage Cost Centers」でのみ保守可能**（KS01/KS02 では不可）。→ [3. 計画と予算管理](03_Planning_and_Budget.md)
- 原価センタの **管理領域通貨と会社コード通貨が異なる場合**、新規原価センタのオブジェクト通貨には会社コード通貨が設定される。
  - 出典: [Specifying the Controlling Area Currency (SAP Help)](https://help.sap.com/docs/SAP_S4HANA_ON-PREMISE/5e23dc8fe9be4fd496f8ab556667ea05/f641de531ed3424de10000000a174cb4.html) `[Help]`

## 1.5 計画機能の変化（概要）

- CO-OM 計画・P/L 計画・利益センタ計画は **SAP BPC for S/4HANA Finance（Integrated Business Planning）** でカバー（Note **2081400**）。
- ただし **KP06（原価要素別原価センタ計画）は SAP Easy Access メニューから削除されたが実行は可能**。
- 詳細: [3. 計画と予算管理](03_Planning_and_Budget.md)

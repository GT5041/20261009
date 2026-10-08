# S/4HANA Cloud Public Edition（参考）

[← バージョン比較](README.md) | [← Home](../Home.md)

On-Premise / Private Edition の将来機能の先行指標として、Public Edition の原価センタ関連情報を記載します。

| トピック | 内容 | ソース |
|---|---|---|
| 標準階層（CE2502〜） | 2502 以前は新規原価センタがデフォルトで Set ベースの標準階層 0001 に割当。**2502 以降は Manage Global Hierarchies の階層 ID 1（予約）が標準階層**となり、作成時に割当 | [Cost Center Standard Hierarchy Change from CE2502](https://community.sap.com/t5/technology-blog-posts-by-sap/cost-center-standard-hierarchy-change-from-ce2502-in-sap-s-4hana-cloud/ba-p/14091199) `[Blog]`, [Cost Center Standard Hierarchies (Help)](https://help.sap.com/docs/SAP_S4HANA_CLOUD/c56f622a2edf491b9f1b596b55587009/e98a67e25fd246408db8c39d8d0b6175.html) |
| Set → グローバル階層移行 | 「Migrate Set-Based Groups to Global Hierarchies」。0001 は ID 1 固定、移行後は Manage Global Hierarchies のみで編集 | [How to Migrate Cost Center Groups to Global Hierarchies (Help)](https://help.sap.com/docs/SAP_S4HANA_CLOUD/c56f622a2edf491b9f1b596b55587009/3d930485bdf747a3a8a4019243498ecf.html) |
| フレキシブル階層で標準階層を代替 | 属性から階層を生成。リアルタイムではない | [Blog](https://community.sap.com/t5/enterprise-resource-planning-blog-posts-by-sap/using-flexible-hierarchies-instead-of-standard-hierarchies-for-cost-and/ba-p/13565975) |
| 計画 | KP06 は利用不可 →「Import Financial Plan Data」 | [Q&A](https://community.sap.com/t5/enterprise-resource-planning-q-a/tcode-kp06-is-not-available-in-s-4hana-public-cloud/qaq-p/13713797) |
| コミットメント | 原価センタ・プロジェクトで対応。レポート用であり、超過防止は AVC で行う | [Cost Center Commitments in SAP S/4HANA Cloud](https://community.sap.com/t5/enterprise-resource-planning-blog-posts-by-sap/cost-center-commitments-in-sap-s-4hana-cloud/ba-p/13495753) |
| 原価センタ作成 | Fiori で作成。原価センタグループは Q・P 両システムで作成が必要 | KBA [2720597](https://apps.support.sap.com/sap/support/knowledge/preview/en/2720597) |
| 構造設計 | 原価センタ・利益センタ・セグメント構造 | [Mastering Cost Center, Profit Center & Segment Structures](https://community.sap.com/t5/enterprise-resource-planning-blog-posts-by-members/mastering-cost-center-profit-center-amp-segment-structures-in-sap-s-4hana/ba-p/13708698) |
| 2602 | 標準利益センタ階層の作成（参考） | [Creating Standard Profit Center Hierarchy (2602)](https://community.sap.com/t5/technology-blog-posts-by-sap/creating-standard-profit-center-hierarchy-in-sap-s-4hana-cloud-public/ba-p/14415549) |

# バージョン別ビュー：機能比較マトリクス

[← Home](../Home.md)

凡例: ✅ 利用可能 / ➖ 従来方式のみ・代替手段 / ❌ 利用不可 / 🆕 そのバージョンで導入・拡張

| 機能 / 論点 | ECC 6.0 | 1511 | 1610 | 1709 | 1809 | 1909 | 2020 | 2021 | 2022 | 2023 | 2025 |
|---|---|---|---|---|---|---|---|---|---|---|---|
| 実績明細を ACDOCA に格納 | ❌（COEP） | 🆕 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| COEP/COSP/COSS 互換ビュー | ― | 🆕 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| 原価要素の独立マスタ（KA01 等） | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| 原価要素を G/L 勘定（FS00）で保守 | ❌ | 🆕 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| 照合元帳（Reconciliation Ledger） | ✅ | ❌（不要） | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| KS01/KS02/KS03 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| KP06（従来型原価センタ計画） | ✅ | ✅※1 | ✅※1 | ✅※1 | ✅※1 | ✅※1 | ✅※1 | ✅※1 | ✅※1 | ✅※1 | ✅※1 |
| 新計画テーブル ACDOCP | ❌ | ※2 | ※2 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| Budget Availability Control（原価センタ） | ➖ Note 68366 | ➖ | ➖ | ➖ | ➖ | 🆕 | ✅ | ✅ | ✅ | ✅ | ✅ |
| 原価センタ間予算移管 Fiori アプリ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | 🆕 | ✅ | ✅ | ✅ | ✅ |
| Universal Allocation | ❌ | ❌ | ❌ | ❌ | ❌ | 🆕 | 🆕 | 🆕 会社間・計画 | 🆕 累積処理 | ✅ | ✅ |
| Universal Parallel Accounting（間接費会計） | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | 🆕 | ✅ | ✅ |
| Manage Cost Centers（F1443A） | ❌ | ※3 | ※3 | ※3 | ※3 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| グローバル階層（Manage Global Hierarchies） | ❌ | ❌ | ❌ | ※3 | ※3 | ※3 | ※3 | ✅ | ✅ | 🆕 参照ノード（GR） | ✅ |
| Migration Cockpit | ― | ※3 | LTMC | LTMC | LTMC | LTMC | 🆕 Migrate Your Data | ✅ | ✅（M01 不具合は 2022 まで） | ✅ | 🆕 新機能 |

**注記**
- ※1 SAP Easy Access メニューからは削除されているがトランザクションは実行可能（[Q&A](https://community.sap.com/t5/enterprise-resource-planning-q-a/cost-center-planning-on-s4-hana-1610/qaq-p/413340)）。Public Cloud では利用不可。
- ※2 ACDOCA ベースの新計画（ACDOCP）の導入時期は、本 Wiki 作成時に参照した公開ソースでは明示的に確認できなかったため、**What's New Viewer で要確認**。
- ※3 該当バージョンでの提供有無・提供時期を公開ソースで特定できなかった項目。導入バージョンは **SAP Fiori Apps Reference Library / What's New Viewer で要確認**。
- 「🆕」は SAP Community Blog / Help Portal で該当バージョンでの導入・拡張が確認できたもの。

## バージョン別ページ

- [SAP ECC 6.0（移行元）](ECC.md)
- [S/4HANA 1511 / 1610](S4HANA_1511_1610.md)
- [S/4HANA 1709 / 1809](S4HANA_1709_1809.md)
- [S/4HANA 1909](S4HANA_1909.md)
- [S/4HANA 2020](S4HANA_2020.md)
- [S/4HANA 2021](S4HANA_2021.md)
- [S/4HANA 2022](S4HANA_2022.md)
- [S/4HANA 2023](S4HANA_2023.md)
- [S/4HANA 2025](S4HANA_2025.md)
- [S/4HANA Cloud Public Edition（参考）](S4HANA_Cloud_Public.md)

## リリースごとの詳細確認方法

- **What's New Viewer for SAP S/4HANA**: カテゴリ（Finance / Controlling）でフィルタして差分を確認
  - 出典: [2022 Release Highlights in Seconds: SAP S/4HANA & SAP S/4HANA Cloud, private edition](https://community.sap.com/t5/enterprise-resource-planning-blog-posts-by-sap/2022-release-highlights-in-seconds-sap-s-4hana-sap-s-4hana-cloud-private/ba-p/13540351) `[Blog]`
- **Simplification Item Catalog**: 移行先バージョンを指定して Controlling 関連 Item を確認

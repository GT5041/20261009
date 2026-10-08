# SAP S/4HANA Cost Center Wiki

SAP S/4HANA の **Cost Center（原価センタ／CO-OM-CCA）** に関する技術情報を、SAP 公式ソース（SAP Note / KBA / SAP Community Q&A / SAP Community Blog / SAP Help Portal）から集約した Wiki です。
S/4HANA を中心に、**SAP ECC からの移行（System Conversion / New Implementation）に関連する場合は ECC 側の情報も併記**しています。

> 最終更新: 2026-10-08
> SAP Note / KBA の本文閲覧には S-User（SAP for Me）ログインが必要です。本 Wiki の記載は公開プレビュー・Community 投稿・Help Portal の情報に基づく要約であり、**適用前に必ず最新版の Note 本文を確認してください**。

---

## ナビゲーション

### トピック別

| # | ページ | 内容 |
|---|--------|------|
| 1 | [ECC と S/4HANA の違い（データモデル）](01_ECC_vs_S4HANA.md) | Universal Journal (ACDOCA)、原価要素の G/L 勘定統合、互換ビュー |
| 2 | [マスタデータと階層](02_Master_Data_and_Hierarchy.md) | KS01/KS02、Fiori「Manage Cost Centers」、標準階層・グローバル階層・フレキシブル階層 |
| 3 | [計画と予算管理](03_Planning_and_Budget.md) | KP06、ACDOCP、Embedded BPC、Budget Availability Control |
| 4 | [配賦（Allocation）](04_Allocation.md) | 従来の配賦／按分サイクル、Universal Allocation、Universal Parallel Accounting |
| 5 | [ECC からの移行](05_Migration_from_ECC.md) | Simplification Item、FINS_RECON、Migration Cockpit「CO - Cost Center」 |
| 6 | [トラブルシューティング（KBA 集）](06_Troubleshooting_KBA.md) | 症状別 KBA / Q&A |
| 7 | [ソース一覧（Notes / KBA / Q&A / Blog）](07_Sources.md) | 参照した全ソースの索引 |

### バージョン別ビュー

| バージョン | 主なトピック |
|-----------|-------------|
| [バージョン比較マトリクス](versions/README.md) | 全バージョンの機能差分を一覧 |
| [SAP ECC 6.0（移行元）](versions/ECC.md) | 移行前に知っておくべき ECC 側の仕様 |
| [S/4HANA 1511 / 1610](versions/S4HANA_1511_1610.md) | Universal Journal、原価要素の G/L 統合、ACDOCP |
| [S/4HANA 1709 / 1809](versions/S4HANA_1709_1809.md) | Fiori マスタ管理アプリ、グローバル階層 |
| [S/4HANA 1909](versions/S4HANA_1909.md) | Cost Center の Budget Availability Control、Universal Allocation |
| [S/4HANA 2020](versions/S4HANA_2020.md) | 予算移管アプリ、「Migrate Your Data」アプリ |
| [S/4HANA 2021](versions/S4HANA_2021.md) | Universal Allocation（会社間・計画配賦）、Cycle Run Groups |
| [S/4HANA 2022](versions/S4HANA_2022.md) | Universal Parallel Accounting、累積サイクル処理 |
| [S/4HANA 2023](versions/S4HANA_2023.md) | 階層の参照ノード、Fiori 改善 |
| [S/4HANA 2025](versions/S4HANA_2025.md) | 最新リリースの UX 改善、Migration Cockpit 新機能 |
| [S/4HANA Cloud Public Edition](versions/S4HANA_Cloud_Public.md) | 参考：CE2502 以降の標準階層変更など |

---

## クイックリファレンス

| 項目 | ECC 6.0 | S/4HANA |
|------|---------|---------|
| マスタテーブル | CSKS / CSKT（テキスト） | CSKS / CSKT（変更なし） |
| 実績明細 | COEP | **ACDOCA**（COEP は互換ビュー経由で参照） |
| 実績合計 | COSP / COSS | **ACDOCA** から算出（COSP/COSS は互換ビュー、旧データは COSP_BAK / COSS_BAK） |
| 計画データ | COEJ / COSP / COSS | **ACDOCP**（新計画）＋ 従来テーブル（KP06 等の従来計画） |
| 原価要素 | KA01/KA02/KA06 による独立マスタ（CSKA/CSKB） | **G/L 勘定（FS00）に統合**、原価要素タイプを G/L マスタに保持 |
| 原価要素のデフォルト勘定設定 | 原価要素マスタ | **OKB9（TKA3A）** |
| 原価センタ作成 | KS01 | KS01 / Fiori **Manage Cost Centers（F1443A）** |
| 原価センタグループ | KSH1/KSH2（Set） | KSH1/KSH2 / Fiori **Manage Cost Center Groups（F1024）** / **Manage Global Hierarchies** |
| 予算管理 | 標準なし（統計指図で代替：Note 68366） | **Budget Availability Control（1909〜）** |
| 配賦 | KSU5（按分）/ KSV5（配布） | 従来トランザクション ＋ **Universal Allocation**（Fiori） |
| データ移行 | LSMW / KS01 BAPI 等 | **Migration Cockpit（LTMC → Migrate Your Data）** |

---

## 本 Wiki のソース方針

- 一次ソース: **SAP Note / KBA**（`me.sap.com/notes/…`, `userapps.support.sap.com/sap/support/knowledge/…`）
- 二次ソース: **SAP Community Blog / Q&A**（`community.sap.com`、旧 `blogs.sap.com` / `answers.sap.com`）
- 補足: **SAP Help Portal**（`help.sap.com`）
- 各記述には参照ソースを併記しています。Community の投稿はユーザ／SAP 社員の個人見解を含むため、記述末尾に `[Blog]` `[Q&A]` などの種別を付記しています。

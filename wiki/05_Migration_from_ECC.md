# 5. ECC からの移行（原価センタ観点）

[← Home](Home.md)

---

## 5.1 移行シナリオ別の論点

| シナリオ | 原価センタ観点の主な論点 |
|---|---|
| **System Conversion（Brownfield）** | Simplification Item Check、FI/CO 整合性チェック、互換ビューを参照するアドオンの改修、原価要素→G/L 勘定への統合、Set 階層の扱い |
| **New Implementation（Greenfield）** | Migration Cockpit「CO - Cost center」によるマスタ移行、階層の再設計（グローバル／フレキシブル階層） |
| **Selective Data Transition** | 管理領域・会社コード単位での移行。履歴 CO データの扱い |

## 5.2 System Conversion: 事前チェック

| ツール / Note | 内容 |
|---|---|
| Note **2399707** | Simplification Item Check フレームワーク |
| Note **2502552** | アプリ別チェッククラス（TCI で配信） |
| Note **2245333** | Finance 事前チェック（元帳・会社コード・**管理領域**設定）の原因と対応 |
| Note **2755360** | **FIN_CORR_RECONCILE / FIN_CORR_DISPLAY / FIN_CORR_MONITOR** による ECC 側の不整合検出・修正 |
| Note **2643232** | FINS_RECON エラーを受け入れてよいケース |
| KBA [2781513](https://userapps.support.sap.com/sap/support/knowledge/en/2781513) | 移行時に大量の「重要でない差異」が出る場合（FINS_RECON 203/204、503/504、543 等） |
| KBA [3639057](https://userapps.support.sap.com/sap/support/knowledge/en/3639057) | S/4HANA 移行時の FI 不整合の修正 |

- Simplification Item Check は **クライアント 000** で実行。リターンコード 8 / 12 は変換をブロックするので必ず解消する。
  - 出典: [SAP S/4HANA Simplification Item Check – How to do it right](https://community.sap.com/t5/enterprise-resource-planning-blog-posts-by-sap/sap-s-4hana-simplification-item-check-how-to-do-it-right/ba-p/13386669) `[Blog]`
  - 出典: [Simplification Item Catalog, Simplification Item Check and SAP Readiness Check](https://community.sap.com/t5/enterprise-resource-planning-blog-posts-by-sap/simplification-item-catalog-simplification-item-check-and-sap-readiness/ba-p/13440989) `[Blog]`
  - 出典: [S/4HANA Conversion – t4 – Simplification Item Check step by step](https://community.sap.com/t5/enterprise-resource-planning-blog-posts-by-sap/s-4hana-conversion-t4-simplification-item-check-step-by-step/ba-p/13443148) `[Blog]`
- FI/CO データ移行時のエラー対応
  - 出典: [Conversion to SAP S/4HANA – How to handle errors during finance data migration](https://community.sap.com/t5/enterprise-resource-planning-blog-posts-by-sap/conversion-to-sap-s-4hana-how-to-handle-errors-during-finance-data/ba-p/13407304) `[Blog]`
  - 出典: [Conversion to SAP S/4HANA – Consistency checks in Finance (Part 1)](https://blogs.sap.com/2018/08/13/conversion-to-sap-s4hana-consistency-checks-in-finance-part-1/) `[Blog]`
  - 出典: [S/4HANA System Conversion – Finance Data Consistency Checks](https://community.sap.com/t5/enterprise-resource-planning-blog-posts-by-sap/s-4hana-system-conversion-finance-data-consistency-checks/ba-p/13490321) `[Blog]`
  - 出典: [Finance Consistency Checks: FIN_CORR_MONITOR](https://community.sap.com/t5/enterprise-resource-planning-blog-posts-by-sap/finance-consistency-checks-fin-corr-monitor/ba-p/13469941) `[Blog]`
  - 出典: [SAP S/4HANA 1909 Post Conversion Tips and Suggestions](https://blogs.sap.com/2020/02/12/sap-s-4hana-1909-post-conversion-tips-and-suggestions) `[Blog]`

> **通貨関連**: SLO による通貨換算や並行通貨導入は当会計年度のみ照合するため、過去年度の不整合が変換ツールでエラーとして検出されることがある（上記 Blog）。管理領域通貨と会社コード通貨の関係も確認すること。

## 5.3 System Conversion: 原価センタ関連の変更点チェックリスト

- [ ] 原価要素（CSKA/CSKB）→ G/L 勘定（原価要素タイプ）への統合を確認（→ [1.3](01_ECC_vs_S4HANA.md)）
- [ ] 原価要素マスタのデフォルト勘定設定 → **OKB9（TKA3A）** への移行を確認
- [ ] COEP/COSP/COSS を直接参照するアドオン・レポート・抽出（BW 等）の洗い出し（Note 2185026）
- [ ] 照合元帳（KALC）を使用している場合は廃止（Universal Journal で不要）
- [ ] 統計内部指図による原価センタ AVC 代替 → 1909 以降は標準 AVC を検討（→ [3.2](03_Planning_and_Budget.md)）
- [ ] 従来型 PCA・FI-GL 計画を使用している場合は再有効化 Note の要否を確認（2253067 / 2345118 / 2313341）
- [ ] Set ベース原価センタ階層 → Fiori 分析で利用するならグローバル階層への移行／HRRP_REP による同期を検討（→ [2.3](02_Master_Data_and_Hierarchy.md)）
- [ ] Fiori アプリの置換（F0123 / F1623 → **F1443A**）

## 5.4 New Implementation: Migration Cockpit「CO - Cost center」

| 項目 | 内容 | ソース |
|---|---|---|
| 方式 | ステージングテーブル／ファイル、Direct Transfer（ECC から直接） | [CO - Cost center (SAP Help)](https://help.sap.com/docs/SAP_S4HANA_CLOUD/d5699934e7004d048c4801b552f3b013/05cb43899af24589884e9b0588725a85.html) `[Help]` |
| 前提オブジェクト | **CO - Profit center**、PSM - Functional area / Fund / Grant、JVA（使用時） | 同上 |
| 並列処理 | **不可**（原価センタ追加時に階層がロックされるため）。1 ファイルにまとめるか順次実行 | 同上、Note **3595079** |
| DRF 連携 | 移行では DRF 複製はトリガーされない。後で「Replicate by Object Selection」等で複製（KBA **2858316**） | 同上 |
| カスタム項目 | 追加・変更・削除のたびに移行プロジェクト更新とテンプレート再ダウンロードが必要 | 同上 |
| ツール | **S/4HANA 2020 以降は Fiori「Migrate Your Data」**。LTMC は 2020 でも使えるが既存プロジェクトの照会のみ | [Utilizing the migration cockpit app for cost centers](https://blogs.sap.com/2023/09/07/utilize-migrate-your-data-fiori-app-for-uploading-cost-centers-into-sap/) `[Blog]` |
| 転送 ID | デフォルト **M01** はエラー（Note **3080500**）。他の番号を使用。2022 より後のリリースで修正済 | [SAP S/4HANA Migration Cockpit: CO-Cost Center Object case](https://community.sap.com/t5/technology-blog-posts-by-members/sap-s-4hana-migration-cockpit-migrate-your-data-using-staging-tables-a-co/ba-p/13993232) `[Blog]` |

### 関連 Note

| Note | 内容 |
|---|---|
| **2684818** | Migration Cockpit の用途（定常インタフェースや大量変更用途には非推奨） |
| **2481235** | 提供移行オブジェクトの拡張／独自オブジェクト作成 |
| **2596400** | 移行オブジェクト一覧 |
| **2698032** | 廃止予定移行オブジェクト |
| **3595079** | Direct Transfer の分割ロジック（CO - Cost Center は 1 ジョブ制限） |

### 参考

- [Migrating data from SAP ECC to SAP S/4HANA with the migration cockpit](https://community.sap.com/t5/enterprise-resource-planning-blog-posts-by-members/migrating-data-from-sap-ecc-to-sap-s4-hana-with-the-migration-cockpit/ba-p/13676908) `[Blog]`
- [10 Tips for using S/4HANA Migration Cockpit's Direct Transfer Approach](https://blogs.sap.com/2020/10/23/10-tips-for-using-s-4hana-migration-cockpits-direct-transfer-approach/) `[Blog]`
- [Part 1: SAP S/4HANA migration cockpit – staging tables](https://blogs.sap.com/2019/12/02/sap-s-4-hana-migration-cockpit-migrating-data-using-staging-tables-and-methods-for-populating-the-staging-tables/) `[Blog]`
- [Latest Features in the SAP S/4HANA Migration Cockpit, released with 2025 FPS0](https://community.sap.com/t5/technology-blog-posts-by-sap/latest-features-in-the-sap-s-4hana-migration-cockpit-released-with-2025/ba-p/14247326) `[Blog]`
- [SAP S/4HANA Migration Cockpit FAQ](https://pages.community.sap.com/topics/s4hana-migration-cockpit/faq) `[Community]`
- [Migration Objects for SAP S/4HANA 1909 (PDF)](https://help.sap.com/doc/69dd0f1ef0034261ab6e617e0b2beb21/1909.latest/en-US/MigrationObjects_OP_EN.pdf) `[Help]`

## 5.5 階層の移行

- Set ベースの原価センタグループは「**Migrate Set-Based Groups to Global Hierarchies**」で移行可能。標準グループ 0001 は新階層 ID **1** に固定。移行後は Manage Global Hierarchies でのみ編集可（Cloud）。
  - 出典: [How to Migrate Cost Center Groups to Global Hierarchies (SAP Help)](https://help.sap.com/docs/SAP_S4HANA_CLOUD/c56f622a2edf491b9f1b596b55587009/3d930485bdf747a3a8a4019243498ecf.html) `[Help]`
- On-Premise では HRRP_REP による Set → グローバル階層同期も選択肢（Note 3108730）。
  - 出典: [How to synchronize Set Controlling Hierarchies in SAP Fiori for SAP S/4HANA](https://community.sap.com/t5/enterprise-resource-planning-blog-posts-by-sap/how-to-synchronize-set-controlling-hierarchies-in-sap-fiori-for-sap-s-4hana/ba-p/13563144) `[Blog]`

## 5.6 Central Finance

- Central Finance ではコストオブジェクトマッピングの標準オブジェクトに原価センタが含まれる。2023 で CO 製造指図が送信元コストオブジェクトとして対応。
  - 出典: [SAP Central Finance – What's new in 2023 release](https://community.sap.com/t5/enterprise-resource-planning-blog-posts-by-members/sap-central-finance-what-s-new-in-2023-release/ba-p/13580108) `[Blog]`

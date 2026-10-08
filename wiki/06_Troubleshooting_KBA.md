# 6. トラブルシューティング（症状別 KBA / Q&A）

[← Home](Home.md)

| 症状 | 原因・対応の要点 | ソース | 対象 |
|---|---|---|---|
| Fiori「Manage Cost Centers」で権限（K_CCA）が期待通りに効かない／階層サブノード配下の原価センタを管理できない | Fiori と GUI で権限チェック仕様が異なる。直接割当ノードのみ有効 | KBA [3692682](https://userapps.support.sap.com/sap/support/knowledge/en/3692682) | S/4HANA |
| Fiori「Manage Cost Centers」に原価計算シート項目がない | KS02 では表示される | KBA [3486329](https://userapps.support.sap.com/sap/support/knowledge/en/3486329) | S/4HANA |
| レポートアプリ（Cost Centers – Plan/Actuals 等）で原価センタ階層が選択できない | 階層種別（Set／グローバル）設定を確認 | KBA [3559612](https://userapps.support.sap.com/sap/support/knowledge/en/3559612) | S/4HANA |
| 将来日付の原価センタが「Manage Cost Center Groups」（F1024）に表示されない | キー日付と有効期間の扱い | KBA [3701946](https://userapps.support.sap.com/sap/support/knowledge/en/3701946) | S/4HANA |
| Work center（CSKS-WERKS）を入力できない | — | KBA [3464269](https://userapps.support.sap.com/sap/support/knowledge/en/3464269) | ECC / S/4HANA |
| Fiori Cloud で原価センタを作成したい／原価センタグループを Q・P 両方で作る必要 | — | KBA [2720597](https://apps.support.sap.com/sap/support/knowledge/preview/en/2720597) | Cloud |
| KS02 で KS042「Change range must cover complete existence period」 | 分析期間単位で階層領域を変更しようとした | KBA [2495740](https://userapps.support.sap.com/sap/support/knowledge/en/2495740) | ECC / S/4HANA |
| KS02 で事業領域を変更できない（KS134） | 当年度に転記あり。翌年度からなら変更可 | [Q&A](https://community.sap.com/t5/enterprise-resource-planning-q-a/not-able-change-business-area-in-cost-center-with-actual-posting/qaq-p/14490467), [Q&A](https://community.sap.com/t5/enterprise-resource-planning-q-a/changing-business-area/qaq-p/9477617) | ECC / S/4HANA |
| 転記後に原価センタマスタを変更したい | Note 62716 の条件、RKACSHOW で実績確認、分析期間の追加／削除 | [Q&A](https://community.sap.com/t5/financial-management-q-a/ks02-edit-cost-center/qaq-p/13639843), [Q&A](https://community.sap.com/t5/enterprise-resource-planning-q-a/deactivating-analysis-period-in-ks02-cost-center-change/qaq-p/6409830) | ECC / S/4HANA |
| 「Cost Center is locked against direct postings」が HCM 等で発生 | 原価センタのロック指示子を確認 | [Q&A](https://community.sap.com/t5/enterprise-resource-planning-q-a/cost-center-related-error-quot-cost-center-is-locked-against-direct/qaq-p/12246028), [Q&A](https://community.sap.com/t5/enterprise-resource-planning-q-a/cost-center-locked-for-revenue-posting/qaq-p/5394113) | ECC / S/4HANA |
| WBS の責任原価センタが計画データアップロード後に変更できない | 仕様（転記後は変更不可） | KBA [3740848](https://userapps.support.sap.com/sap/support/knowledge/en/3740848) | Cloud |
| 従来型 PCA 再有効化後、KP06 の統合計画が EC-PCA に反映されない | — | KBA [2996269](https://userapps.support.sap.com/sap/support/knowledge/en/2996269) | S/4HANA |
| COSS の値と ACDOCA が一致しないように見える | WRTTP 04 は ACDOCA から読まれる。V_COSS_ORI と ACDOCA で確認 | KBA [3730708](https://userapps.support.sap.com/sap/support/knowledge/en/3730708) | S/4HANA |
| SE16 で COSP/COSS/COEP/COVP 参照時にメモリ不足 | 互換ビューの性能特性 | Note 3127377 | S/4HANA |
| 標準階層 1 を有効化できない（未割当原価センタ） | 原価センタと階層ノードの有効期間の重なり | [Q&A](https://community.sap.com/t5/enterprise-resource-planning-q-a/unassigned-cost-center-despite-assignment-in-1-standard-hierarchy-in-manage/qaq-p/14340909) | Cloud / S/4HANA |
| MDG 複製で原価センタのフレキシブル階層項目が消える | MDG-F は Delete & Re-create | [Q&A](https://community.sap.com/t5/technology-q-a/cost-center-changes-from-mdg-is-wiping-out-existing-values-of-flexible/qaq-p/12674249) | S/4HANA + MDG |
| 移行時 FINS_RECON 203/204/503/504/543 等が多発 | 重要でない差異の扱い | KBA [2781513](https://userapps.support.sap.com/sap/support/knowledge/en/2781513), Note 2643232 | ECC→S/4 |
| Migration Cockpit で転送 ID M01 使用時にエラー | 他の番号を使用 | Note 3080500 | 〜2022 |

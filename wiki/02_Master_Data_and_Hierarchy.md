# 2. マスタデータと階層

[← Home](Home.md)

---

## 2.1 原価センタマスタの保守手段

| 手段 | 種別 | 備考 |
|---|---|---|
| KS01 / KS02 / KS03 | SAP GUI | ECC と同じ。作成／変更／照会 |
| KS12 / KS13 等 | SAP GUI | 一括変更・一覧（ECC と同等） |
| **Manage Cost Centers（F1443A, Version 2）** | Fiori | 旧ファクトシート F0123（Cost Center (ERP)）/ F1623（Cost Center (sFIN)）の後継 |
| **Manage Cost Center Groups（F1024）** | Fiori | Set ベースのグループ／標準階層の保守 |
| **Manage Global Hierarchies** | Fiori | グローバル階層（ID プレフィックス `G`）の保守。有効化時に自動で複製される |
| Migration Cockpit「CO - Cost center」 | 移行ツール | → [5. ECC からの移行](05_Migration_from_ECC.md) |

- F1443A が F0123 / F1623 の後継アプリ
  - 出典: [SAP Fiori for SAP S/4HANA – Replacing SAP Fiori apps during system conversion](https://community.sap.com/t5/enterprise-resource-planning-blog-posts-by-sap/sap-fiori-for-sap-s-4hana-replacing-sap-fiori-apps-during-system-conversion/ba-p/14260897) `[Blog]`
- Fiori と GUI で保守可能な項目には差異がある（下表）。

### GUI と Fiori の差異（既知事例）

| 事象 | ソース |
|---|---|
| Fiori「Manage Cost Centers」で **原価計算シート（Costing Sheet）項目が表示されない**（KS02 では表示される） | KBA [3486329](https://userapps.support.sap.com/sap/support/knowledge/en/3486329) |
| Fiori アプリでの権限チェック（**K_CCA**）が KS01/KS02 と異なる。標準階層（OKENN）のサブノード配下の原価センタを管理できない場合がある（直接割当ノードのみ有効） | KBA [3692682](https://userapps.support.sap.com/sap/support/knowledge/en/3692682) |
| **Work center（CSKS-WERKS）** を KS01/KS02 で入力できない | KBA [3464269](https://userapps.support.sap.com/sap/support/knowledge/en/3464269) |
| **Budget Availability Control プロファイル**の割当は Fiori でのみ可能 | [S/4HANA 1909 Cost Centres Budget Availability Control – Configuration](https://blogs.sap.com/2020/01/02/s-4hana-1909-cost-centres-budget-availability-control-configuration/) `[Blog]` |
| Fiori で **Lock Commitment Updates** フラグを外さないとコミットメントが更新されない | [Cost Center Commitments in SAP S/4HANA Cloud](https://community.sap.com/t5/enterprise-resource-planning-blog-posts-by-sap/cost-center-commitments-in-sap-s-4hana-cloud/ba-p/13495753) `[Blog]` |

## 2.2 転記後のマスタ変更（ECC / S/4HANA 共通）

| メッセージ / ソース | 内容 |
|---|---|
| **KS042** "Change range must cover complete existence period" – KBA [2495740](https://userapps.support.sap.com/sap/support/knowledge/en/2495740) | KS02 で分析期間を指定して階層領域を変更しようとすると発生。S/4HANA / S/4HANA Finance が対象 |
| **KS134**（事業領域の変更） – [Q&A](https://community.sap.com/t5/enterprise-resource-planning-q-a/not-able-change-business-area-in-cost-center-with-actual-posting/qaq-p/14490467) | 当会計年度に転記がある場合は変更不可。翌会計年度からの変更は可能（メッセージ長文参照） |
| SAP Note **62716** – [KS02 edit Cost Center (Q&A)](https://community.sap.com/t5/financial-management-q-a/ks02-edit-cost-center/qaq-p/13639843) | 転記後のマスタ変更条件（変更区間が期間境界であること、区間以降に依存する実績がないこと 等）。**RKACSHOW** で実績の存在を確認 |
| [Is it possible to change Company Code in Cost center, once postings done?](https://community.sap.com/t5/enterprise-resource-planning-q-a/is-it-possible-to-change-company-code-in-cost-center-once-postings-done/qaq-p/10466379) | 会社コード変更は不可 → 新規原価センタ作成 & 旧原価センタロックが一般的 |
| [KS02 - changing of Profit center](https://community.sap.com/t5/enterprise-resource-planning-q-a/ks02-changing-of-profit-center/qaq-p/11337984) | 利益センタ変更：転記なし→警告、転記あり→エラー |

## 2.3 階層（Hierarchy）

S/4HANA では原価センタ階層に 3 種類の方式があり、**ID プレフィックスで種別を判別**できます。

| 種別 | 例 | 保守アプリ | 特徴 |
|---|---|---|---|
| Set ベース（従来） | `0101` | KSH1/KSH2、Manage Cost Center Groups | ECC と互換。標準階層（OKEON/OKENN）はこの方式 |
| グローバル階層 | `G101` | Manage Global Hierarchies | 有効化時に自動複製。Fiori 分析アプリで利用 |
| フレキシブル階層 | `F101` | Manage Flexible Hierarchies | マスタ属性（国、会社コード、原価センタカテゴリ等）から自動生成 |

- 出典: [Manage your hierarchies in FIORI 1/3](https://blogs.sap.com/2022/03/14/manage-your-hierarchies-in-fiori..-1-3/) `[Blog]`
- 出典: [Manage your hierarchies in FIORI 3/3](https://community.sap.com/t5/enterprise-resource-planning-blog-posts-by-members/manage-your-hierarchies-in-fiori-3-3/ba-p/13532299) `[Blog]`

### 注意点

1. **Manage Global Hierarchies で原価センタ階層を作成し始めると、Fiori レポートは Set ベース階層を読まなくなる**。
   - 出典: [Manage your hierarchies in FIORI 1/3](https://blogs.sap.com/2022/03/14/manage-your-hierarchies-in-fiori..-1-3/) `[Blog]`
2. Set ベース階層をグローバル階層へ同期するには **HRRP_REP**（Replicate Runtime Hierarchy）等を使用。ただし複製された Set は「本当の Fiori 階層ではない」ため、最終的には移行が必要との意見もある。
   - 出典: [How to synchronize Set Controlling Hierarchies in SAP Fiori for SAP S/4HANA](https://community.sap.com/t5/enterprise-resource-planning-blog-posts-by-sap/how-to-synchronize-set-controlling-hierarchies-in-sap-fiori-for-sap-s-4hana/ba-p/13563144) `[Blog]`
   - 関連 Note: **3108730**（Set とグローバル階層の同期）
3. 標準階層 0001 は Manage Cost Center Groups では**仕様上編集不可**（Public Cloud）。
   - 出典: [Cost Center Standard Hierarchy S/4HANA Public Cloud](https://community.sap.com/t5/enterprise-resource-planning-q-a/cost-center-standard-hierarchy-s-4hana-public-cloud/qaq-p/12687709) `[Q&A]`
4. 代替階層を使う場合、全原価センタが含まれる保証はない。
   - 出典: [Cost Center Hierarchy, flexible and Global accounting hierarchies](https://community.sap.com/t5/enterprise-resource-planning-q-a/cost-center-hierarchy-flexible-and-global-accounting-hierarchies/qaq-p/12350920) `[Q&A]`
5. フレキシブル階層は**リアルタイム更新ではない**。属性値変更後は再生成が必要。属性未設定は「Unassigned」ノードに入る。
   - 出典: [Using Flexible Hierarchies Instead of Standard Hierarchies for Cost and Profit Centers in S/4HANA Cloud](https://community.sap.com/t5/enterprise-resource-planning-blog-posts-by-sap/using-flexible-hierarchies-instead-of-standard-hierarchies-for-cost-and/ba-p/13565975) `[Blog]`
6. **MDG（MDG-F）から原価センタを複製すると、S/4 側で手動保守したフレキシブル階層用項目が消える**（Delete & Re-create 方式のため、ペイロードに無い項目はクリアされる）。
   - 出典: [Cost Center changes from MDG is wiping out existing values of flexible hierarchy fields](https://community.sap.com/t5/technology-q-a/cost-center-changes-from-mdg-is-wiping-out-existing-values-of-flexible/qaq-p/12674249) `[Q&A]`
7. 標準階層 1 が「未割当の原価センタがある」ため有効化できない場合、**原価センタと階層ノードの有効期間の重なり**を確認。
   - 出典: [Unassigned Cost Center despite assignment in 1~ Standard Hierarchy](https://community.sap.com/t5/enterprise-resource-planning-q-a/unassigned-cost-center-despite-assignment-in-1-standard-hierarchy-in-manage/qaq-p/14340909) `[Q&A]`
8. 将来日付の有効開始日で作成した原価センタが、キー日付を将来日にしても Manage Cost Center Groups（F1024）に表示されない。
   - 出典: KBA [3701946](https://userapps.support.sap.com/sap/support/knowledge/en/3701946)
9. Plan/Actual 等のレポートアプリで原価センタ階層が選択肢に出てこない。
   - 出典: KBA [3559612](https://userapps.support.sap.com/sap/support/knowledge/en/3559612)

### 階層の属性化

- [How to convert Hierarchy to Attribute in S/4HANA](https://community.sap.com/t5/enterprise-resource-planning-blog-posts-by-sap/how-to-convert-hierarchy-to-attribute-in-s-4hana/ba-p/13571608) `[Blog]`
- [Simple understanding of Hierarchies in SAP S/4HANA Embedded Analytics](https://community.sap.com/t5/technology-blog-posts-by-members/simple-understanding-of-hierarchies-in-sap-s-4hana-embedded-analytics-part/ba-p/14274085) `[Blog]`

## 2.4 参考ガイド

- [The Ultimate Guide to SAP S/4HANA Controlling Master Data](https://community.sap.com/t5/supply-chain-management-blog-posts-by-members/the-ultimate-guide-to-sap-s-4hana-controlling-master-data/ba-p/14226391) `[Blog]`
- [Mastering Cost Center, Profit Center & Segment Structures in SAP S/4HANA Cloud, Public Edition](https://community.sap.com/t5/enterprise-resource-planning-blog-posts-by-members/mastering-cost-center-profit-center-amp-segment-structures-in-sap-s-4hana/ba-p/13708698) `[Blog]`
- [Assignment of Cost Centers (SAP Help)](https://help.sap.com/docs/SAP_S4HANA_ONPREMISE/651d8af3ea974ad1a4d74449122c620e/926cd7531a4d424de10000000a174cb4.html) `[Help]`

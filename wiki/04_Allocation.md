# 4. 配賦（Allocation）

[← Home](Home.md)

---

## 4.1 従来の配賦（ECC 互換）

- 按分（Assessment, KSU1/KSU5）、配布（Distribution, KSV1/KSV5）、活動配分などの従来トランザクションは S/4HANA On-Premise でも利用可能。
- S/4HANA では二次原価要素も G/L 勘定のため、配賦結果は ACDOCA に記録され、FI 側と常に一致する（照合元帳不要）。
  - 出典: [What you should know about controlling in SAP S/4HANA (Part 1)](https://community.sap.com/t5/enterprise-resource-planning-blog-posts-by-sap/what-you-should-know-about-controlling-in-sap-s-4hana-part-1/ba-p/13443867) `[Blog]`
  - 出典: [Allocations & Universal Allocation](https://blogs.sap.com/2021/04/29/allocations-universal-allocation/) `[Blog]`

## 4.2 Universal Allocation（Fiori）

| バージョン | 原価センタ関連の主な内容 | ソース |
|---|---|---|
| 1909 | Universal Allocation の On-Premise 提供開始（初期スコープ） | [Universal Allocation in SAP S/4HANA 1909](https://blogs.sap.com/2020/08/27/universal-allocation-in-sap-s-4hana-1909/) `[Blog]` |
| 2020 | スコープ拡張 | [Universal Allocation in SAP S/4HANA 2020](https://blogs.sap.com/2021/06/11/universal-allocation-in-sap-s-4hana-2020/) `[Blog]` |
| 2021 | 原価センタ配賦が **実績・計画の両方**に対応。**会社間配賦**（実績: FPS00、計画: FPS01）。**Manage Cycle Run Groups** アプリ追加。FPS00 時点で 5 つの Fiori アプリ | [Universal Allocation in SAP S/4HANA 2021](https://community.sap.com/t5/enterprise-resource-planning-blog-posts-by-sap/universal-allocation-in-sap-s-4hana-2021/ba-p/13541317) `[Blog]` |
| 2022 | **累積サイクル処理**、Allocation Tag ビュータイプ | [Universal Allocation in SAP S/4HANA 2022](https://community.sap.com/t5/enterprise-resource-planning-blog-posts-by-sap/universal-allocation-in-sap-s-4hana-2022/ba-p/13553646) `[Blog]` |

### 制約事項

- 会社間配賦は **同一管理領域内のみ**。サイクルセグメントごとに送信側会社コードは 1 つ。同一サイクル内で会社内／会社間の混在不可。
- Universal Allocation は **未決済明細管理勘定へは転記不可**。
  - 出典: [Universal Allocation in SAP S/4HANA 2021](https://community.sap.com/t5/enterprise-resource-planning-blog-posts-by-sap/universal-allocation-in-sap-s-4hana-2021/ba-p/13541317) `[Blog]`
  - 出典: [Intercompany Allocations, using the Universal Allocation](https://community.sap.com/t5/enterprise-resource-planning-blog-posts-by-sap/intercompany-allocations-using-the-universal-allocation/ba-p/13527353) `[Blog]`

### 参考

- [Cost Allocation via Universal Allocation](https://community.sap.com/t5/enterprise-resource-planning-blog-posts-by-sap/cost-allocation-via-universal-allocation/ba-p/13390017) `[Blog]`
- [Introduction to Universal Cost Allocation in SAP S/4HANA (Part I)](https://community.sap.com/t5/financial-management-blog-posts-by-members/introduction-to-universal-cost-allocation-in-sap-s-4hana-part-i/ba-p/13909866) `[Blog]`
- [Introduction to Universal Cost Allocation in SAP S/4HANA (Part II)](https://community.sap.com/t5/financial-management-blog-posts-by-members/introduction-to-universal-cost-allocation-in-sap-s-4hana-part-ii/ba-p/13914371) `[Blog]`

## 4.3 Universal Parallel Accounting（UPA）

- S/4HANA 2022 で、原価センタを含む間接費会計（Overhead Accounting）が Universal Parallel Accounting に対応。
  - 出典: [Overhead Accounting with Universal Parallel Accounting in SAP S/4HANA 2022](https://community.sap.com/t5/enterprise-resource-planning-blog-posts-by-sap/overhead-accounting-with-universal-parallel-accounting-in-sap-s-4hana-2022/ba-p/13540993) `[Blog]`

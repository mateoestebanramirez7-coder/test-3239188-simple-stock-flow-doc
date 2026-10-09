# 01 - Context & System Scope

## General Overview
**Simple Stock Flow** is a backend system designed for inventory management, internal operator authentication, and immutable sales recording (`06-data/data-model.md §1`). It serves as supporting software for managing warehouse stock flow, available inventory, and point-of-sale checkout operations (`06-data/data-model.md §1`).

---

## Detailed Scope

### What IS Built (In Scope)
- **Catalog Management:** Product registration and retrieval constrained strictly to name, price, stock, category, and an optional image key (`06-data/data-model.md §1` DP-03).
- **Fixed Category Seeding:** Mandatory product assignment to one of 5 read-only, pre-seeded categories (`06-data/data-model.md §2.1`, `§9.1`).
- **Inventory & Stock Control:** Strict stock level enforcement preventing negative balances both in application logic and database engine constraints (`06-data/data-model.md §2.2`, `ck_product_stock_non_negative`).
- **Sales Processing & Immutability:** Recording of itemized sales with point-in-time snapshot freezing for unit prices, product names, and category names (`06-data/data-model.md §1`, `§2.3`, `§2.4`).
- **Security & Identity:** Internal operator authentication utilizing irreversible password hashing and closed-set role attribution (`admin`, `seller`) (`06-data/data-model.md §1`, `§2.5`).
- **Sales Reporting:** On-the-fly aggregate sales report generation calculated directly within the database engine over date ranges without persisting redundant tables (`06-data/data-model.md §1`, D-06).

### What IS NOT Built (Out of Scope)
- **Customer Profiles or Accounts:** No customer or end-buyer entities exist within the system (`06-data/data-model.md §1`).
- **Multi-Currency Support:** The system operates under a single-currency architecture by design (`06-data/data-model.md §1`, D-05).
- **Electronic Invoicing or Payments:** Integrations with tax authorities, POS printing, or external payment gateways are excluded `[Assumption]`.
- **Dynamic Category Management:** Categories are static and read-only; no CRUD endpoints exist for category maintenance (`06-data/data-model.md §2.1`).
- **Generic Audit Columns:** Catalog entities do not contain generic `created_at` or `updated_at` columns (`06-data/data-model.md §8`)..
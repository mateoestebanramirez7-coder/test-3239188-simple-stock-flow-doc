# 03 - Product Vision & Problem Statement

## Problem Statement
In retail and inventory operations, organizations frequently face financial discrepancies, reporting inconsistencies, and inventory leakage due to three primary factors:

1. **Retroactive Data Distortion:** When catalog prices or product names change over time, naive systems retroactively alter past sales records, invalidating closed accounting periods (`06-data/data-model.md §1`).
2. **Uncontrolled Inventory Balances:** Lack of strict database-level constraints allows negative stock balances, causing physical stockouts and inventory tracking failures (`06-data/data-model.md §2.2`).
3. **Traceability & Operator Attribution Gaps:** Inability to pin point-of-sale transactions to authenticated internal operators without corrupting audit trails (`06-data/data-model.md §1`, `§2.5`).

---

## Product Vision
**Simple Stock Flow** is a lightweight, high-reliability inventory management and sales recording system. Its primary purpose is to provide **absolute historical integrity** for sales transactions through point-in-time data freezing, strict non-negative stock enforcement at the database level, and zero-redundancy aggregate reporting calculated directly on the database engine (`06-data/data-model.md §1`, `§2.2`, D-06).

---

## Core Product Principles
- **Point-in-Time Immutability:** Sales record immutable snapshot copies of unit prices, product names, and category names at the instant of transaction (`06-data/data-model.md §1`, ADR-004).
- **Engine-Level Integrity:** Critical business constraints (such as `stock >= 0`) are enforced directly by the database engine, ensuring data safety against out-of-band writes (`06-data/data-model.md §2.2`, `ck_product_stock_non_negative`).
- **Zero-Redundancy Architecture:** Eliminates unrequested audit columns, unnecessary intermediary join tables, and pre-calculated report persistence (`06-data/data-model.md §1`, `§8`).

---

## Product Assumptions & Boundaries
- `[Assumption]` The product is designed exclusively for internal store/warehouse operators (`admin`, `seller`) and does not feature a public customer e-commerce interface (`06-data/data-model.md §1`).
- `[Assumption]` Product return workflows, partial order refunds, and order cancellations are excluded from the current product lifecycle (`06-data/data-model.md §2.3`).
- `[Assumption]` Sales receipt generation and physical thermal printing integrations are handled outside the core domain model `[Assumption]`.
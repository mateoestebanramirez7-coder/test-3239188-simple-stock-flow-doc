# 04 - Requirements Specification

## User Stories (US)

### US-01: Product Catalog Management
- **As an** internal system operator (`admin`).
- **I want to** register and update product details strictly comprising name, price, stock, category, and optional image key.
- **So that** I can maintain an active catalog for point-of-sale processing without bloated schema fields.
- **Traceability:** `06-data/data-model.md §1` (DP-03), `§2.2` (`Product`).

### US-02: Point-in-Time Immutable Sales Recording
- **As a** sales operator (`seller` or `admin`).
- **I want to** record sales transactions that capture snapshot copies of product names, unit prices, and category names.
- **So that** historical financial reports remain unaltered when product details or categories change in the catalog.
- **Traceability:** `06-data/data-model.md §1` (Frozen values), `§2.3` (`Sale`), `§2.4` (`SaleItem`), ADR-004.

### US-03: Operator Authentication & Attribution
- **As an** internal system operator.
- **I want to** authenticate securely using a unique username and password.
- **So that** sales transactions are accurately attributed to my user account for auditing purposes.
- **Traceability:** `06-data/data-model.md §1` (`User`), `§2.5` (`User` invariants), `§3` (`sale.sold_by`).

### US-04: Aggregate Sales Reporting
- **As an** administrator.
- **I want to** generate aggregate sales reports filtered by date ranges grouping by product and frozen category label.
- **So that** I can analyze commercial performance over closed periods without modifying past report reads.
- **Traceability:** `06-data/data-model.md §1` (Sales report), `§11.1` (H-1 report grouping decision).

---

## Non-Functional Requirements (NFR)

- **NFR-01 (Engine-Level Stock Integrity):** The system must guarantee that stock levels never drop below zero at the database level via explicit check constraints (`06-data/data-model.md §2.2`, `§4`, `ck_product_stock_non_negative`).
- **NFR-02 (Single-Currency Enforcement):** The system must operate under a strict single-currency architecture without storing or projecting currency codes in monetary fields (`06-data/data-model.md §1`, D-05).
- **NFR-03 (Credential Privacy & Security):** User passwords must never be stored in plain text or indexed; only irreversible password hashes (`password_hash`) are permitted (`06-data/data-model.md §1`, `§7`, D-09).
- **NFR-04 (Timezone Uniformity):** All system timestamps must strictly use `timestamptz` mapped to UTC (`06-data/data-model.md §3`).
- **NFR-05 (Read-Optimized Indexing):** Date range queries and report aggregations must be supported by dedicated single and compound indexes over `sale.sold_at` and `sale_item.product_id` (`06-data/data-model.md §6.2`).
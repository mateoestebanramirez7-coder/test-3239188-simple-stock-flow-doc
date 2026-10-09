# 02 - Domain Model

## Domain Glossary
- **Product:** Catalog item characterized strictly by name, price, stock, category, and an optional image key (`06-data/data-model.md §1`, DP-03).
- **Category:** Fixed classification belonging to a closed set of five pre-seeded elements without maintenance operations (`06-data/data-model.md §1`, D-10).
- **Price:** Monetary value of a product in the catalog. Strictly positive (`06-data/data-model.md §1`, `§2.2`).
- **Stock:** Available physical units of a product. Non-negative (`06-data/data-model.md §1`, `ck_product_stock_non_negative`).
- **Product Image:** Opaque binary storage key in external storage. Never a file path or raw binary (`06-data/data-model.md §1`, D-08).
- **Sale:** Consumated and immutable commercial transaction recording timestamp, operator, and sold items (`06-data/data-model.md §1`, `§2.3`).
- **Sale Line (`SaleItem`):** Itemized record within a sale containing quantity, frozen price, frozen name, and frozen category name (`06-data/data-model.md §1`, `§2.4`).
- **Quantity:** Units sold within a sale line. Strictly positive (`06-data/data-model.md §1`, `§2.4`).
- **User:** Internal operator who authenticates and registers sales (`06-data/data-model.md §1`, `§2.5`).
- **Role:** Operator attribution restricted to a closed set: `admin` or `seller` (`06-data/data-model.md §1`, `§2.5`).
- **Password Hash:** Irreversible credential digest (`06-data/data-model.md §1`, D-09).

---

## Domain Entities & Invariants

### 1. `Product` (Aggregate Root)
- **Invariants:**
  - Name is required, non-empty, and stored trimmed (`06-data/data-model.md §2.2`).
  - `price > 0` enforced by domain logic (`06-data/data-model.md §2.2`).
  - `stock >= 0` enforced at domain level and backed by database check constraint `ck_product_stock_non_negative` (`06-data/data-model.md §2.2`, `§4`).
  - Belongs to exactly one mandatory and existing category (`06-data/data-model.md §2.2`, `FK-1`).
  - Soft deletion via `deleted_at` shadow property; physical deletion is prohibited (`06-data/data-model.md §2.2`, ADR-003).

### 2. `Sale` (Aggregate Root)
- **Invariants:**
  - Must contain at least one sale line to be confirmed (`06-data/data-model.md §2.3`).
  - Product duplication within the same sale is strictly forbidden (`06-data/data-model.md §2.3`, `IX_sale_item_sale_id_product_id`).
  - Withdrawing stock and adding a sale line constitute a single atomic domain operation (`06-data/data-model.md §2.3`).
  - Immutable once recorded: no update or delete operations exist (`06-data/data-model.md §2.3`).

### 3. `SaleItem` (Internal Aggregate Entity)
- **Invariants:**
  - Belongs strictly to its parent sale (`FK_sale_item_sale_sale_id` with `ON DELETE CASCADE`) (`06-data/data-model.md §2.4`, `FK-2`).
  - `quantity > 0` enforced by domain constructor (`06-data/data-model.md §2.4`).
  - Product name, unit price, and category name are frozen copies captured at the instant of sale (`06-data/data-model.md §1`, `§2.4`, ADR-004).

---

## Domain Events
- `StockWithdrawn`: Fired when units are decremented during sale creation (`06-data/data-model.md §2.3`).
- `SaleConfirmed`: Fired upon successful persistence of an immutable sale aggregate (`06-data/data-model.md §2.3`).
- `ProductSoftDeleted`: Fired when a product is marked as inactive (`06-data/data-model.md §2.2`, ADR-003).
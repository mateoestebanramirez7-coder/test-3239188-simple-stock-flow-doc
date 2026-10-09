# 05 - System Architecture

## Architectural Style
- **Hexagonal Architecture (Ports & Adapters) / DDD:** Strict separation of the core domain logic from persistence, infrastructure, and presentation layers (`06-data/data-model.md §3.1`).
- **Relational Persistence:** PostgreSQL 16 managed via Entity Framework Core in C#, enforcing strict schema alignment (`06-data/data-model.md §0`).

---

## Aggregates & System Components

### 1. Catalog Aggregate (`Product`)
- **Aggregate Root:** `Product` (`06-data/data-model.md §2.2`).
- **Bound Entities / References:** Linked to `Category` via mandatory `category_id` foreign key (`06-data/data-model.md §2.2`, `FK-1`).
- **Persistence Boundary:** Mapped to table `sales.product` (`06-data/data-model.md §3`).

### 2. Sales Aggregate (`Sale`)
- **Aggregate Root:** `Sale` (`06-data/data-model.md §2.3`).
- **Internal Entities:** `SaleItem` (`06-data/data-model.md §2.4`), representing an itemized line within a composition relationship (`ON DELETE CASCADE`) (`06-data/data-model.md §2.4`, `FK-2`).
- **Persistence Boundary:** Mapped to tables `sales.sale` and `sales.sale_item` (`06-data/data-model.md §3`).

### 3. Identity Aggregate (`User`)
- **Aggregate Root:** `User` (`06-data/data-model.md §2.5`).
- **Persistence Boundary:** Mapped to table `sales.user` (`06-data/data-model.md §3`).

### 4. Reference Entity (`Category`)
- **Read-Only Entity:** Seeded lookup data without write repositories or maintenance ports (`06-data/data-model.md §2.1`, `§9.1`).
- **Persistence Boundary:** Mapped to table `sales.category` (`06-data/data-model.md §3`).

---

## Rule Placement (Domain vs. Database Engine)

| Rule / Invariant | Enforcement Location | Tracing & Mechanism |
|---|---|---|
| Non-negative stock (`stock >= 0`) | **Database Engine & Domain** | `ck_product_stock_non_negative` constraint (`06-data/data-model.md §2.2`, `§4`) |
| Unique Category Name | **Database Engine** | Unique index `IX_category_name` (`06-data/data-model.md §2.1`, `§4`) |
| Unique Username | **Database Engine** | Unique index `IX_user_username` (`06-data/data-model.md §2.5`, `§4`) |
| Stock Withdrawal Logic | **Domain Only** | Domain method `Product.Withdraw` (`06-data/data-model.md §2.2`) |
| Price & Quantity (`> 0`) | **Domain Only** | Constructor guards in `Money` and `Quantity` value objects (`06-data/data-model.md §2.2`, `§2.4`) |
| String Trimming & Lowercase | **Domain Only** | Domain normalization methods (`06-data/data-model.md §2.2`, `§2.5`) |
| Non-duplication in Sale | **Domain & Database Engine** | Unique index `(sale_id, product_id)` (`06-data/data-model.md §2.3`, `§4`) |

---

## Ports & Adapters Structure
- **Input Ports (Use Cases):** Application interfaces for user authentication, sales recording, and catalog management.
- **Output Ports (Infrastructure):** Repository interfaces for relational persistence and external image storage adapters (`06-data/data-model.md §1`).
- **Read-Only Reporting Port:** Direct database engine projection for sales reporting over date ranges without aggregate materialization (`06-data/data-model.md §1`, D-06).

---

## Architectural Consistency & Model Alignment (Closing Check)
- **Physical Schema Mapping:** The 5 database tables (`sales.category`, `sales.product`, `sales.sale`, `sales.sale_item`, `sales.user`) map 1:1 with the domain aggregates and reference entities (`06-data/data-model.md §0`, `§3`).
- **Audit Column Decision:** Generic `created_at` / `updated_at` triggers and columns are explicitly omitted to prevent dual-truth conflicts with domain-driven time capturing (`06-data/data-model.md §8`).
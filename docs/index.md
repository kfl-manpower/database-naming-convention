# PostgreSQL Database Naming Conventions

This document provides a comprehensive guide to the naming conventions used for database objects in our PostgreSQL environments.

## Table of Contents

1.  [General Principles](general-principles.md)
2.  [Databases](databases.md)
3.  [Schemas](schemas.md)
4.  [Tables](tables.md)
5.  [Columns](columns.md)
6.  [Primary Keys](primary-keys.md)
7.  [Foreign Keys](foreign-keys.md)
8.  [Indexes](indexes.md)
9.  [Views](views.md)
10. [Functions](functions.md)
11. [Stored Procedures](stored-procedures.md)
12. [Triggers](triggers.md)
13. [Sequences](sequences.md)
14. [Abbreviations](abbreviations.md)
15. [Examples](examples.md)

## Key Principles

- **Clarity**: Names should be descriptive and unambiguous.
- **Consistency**: Apply the same naming rules throughout the database.
- **Conciseness**: Strive for brevity without sacrificing clarity.
- **Meaningful**: Names should reflect the meaning and role of the object.
- **Lowercase with Underscores (`snake_case`)**: Use lowercase letters for all database object names, separating words with underscores (e.g., `first_name`, `last_name`).
- **Avoid Reserved Words**: Do not use PostgreSQL reserved keywords as object names.

For a detailed explanation of each section and examples, please refer to the linked documents above.

---

## Prefixes for Standalone Objects

### Views

- **Prefix**: `v_`
- **Format**: `v_<base_name>`
- **Example**: `v_sales_summary`

**Rationale**:

- Prefixing views ensures they are immediately identifiable as derived or virtual tables.
- Views serve as standalone objects and are often grouped separately from related tables.

### Stored Procedures

- **Prefix**: `p_`
- **Format**: `p_<action>_<object>`
- **Example**: `p_calculate_commission`

**Rationale**:

- Prefixing stored procedures (`sp_`) highlights their role as executable logic units.
- This convention separates stored procedures from tables, views, and triggers in schema browsers.

### Triggers

- **Prefix**: `trg_`
- **Format**: `trg_<table>_<event>_<timing>`
- **Example**: `trg_orders_after_insert`

**Rationale**:

- Prefixing triggers (`tr_`) ensures they are distinct and easily recognizable.
- Triggers are standalone objects tied to table events but perform independent actions.

### Sequences

- **Prefix**: `seq_`
- **Format**: `seq_<base_name>`
- **Example**: `seq_customer_id`

**Rationale**:

- Prefixing sequences (`seq_`) distinguishes them from tables and avoids clashes.
- Sequences often generate unique identifiers and serve purposes distinct from tables.

---

## Suffixes for Table-Specific Objects

### Primary Keys

- **Suffix**: `_pk`
- **Format**: `<table>_pk`
- **Example**: `orders_pk`

**Rationale**:

- PostgreSQL automatically names primary keys with the `_pk` suffix when created using constraints.
- Suffixing primary keys associates them with their parent table while maintaining clarity.

### Foreign Keys

- **Suffix**: `_fk`
- **Format**: `<referencing_table>_<referenced_table>_fk`
- **Example**: `orders_customer_fk`

**Rationale**:

- Foreign keys are named with the `_fk` suffix by PostgreSQL when automatically generated.
- This convention helps identify relationships between tables while avoiding ambiguity.

### Indexes

- **Suffix**: `_idx`
- **Format**: `<table>_<column>_idx`
- **Example**: `products_name_idx`

**Rationale**:

- PostgreSQL automatically applies the `_idx` suffix to index names.
- This ensures consistency with default naming practices while clearly indicating indexed columns.

---

## Summary Table

| **Object Type**   | **Prefix** | **Suffix** | **Example**              |
| ----------------- | ---------- | ---------- | ------------------------ |
| Views             | `v_`       | None       | `v_employee_summary`     |
| Stored Procedures | `sp_`      | None       | `sp_calculate_tax`       |
| Triggers          | `tr_`      | None       | `tr_orders_after_insert` |
| Sequences         | `seq_`     | None       | `seq_customer_id`        |
| Primary Keys      | None       | `_pk`      | `orders_pk`              |
| Foreign Keys      | None       | `_fk`      | `orders_customer_fk`     |
| Indexes           | None       | `_idx`     | `products_name_idx`      |

---

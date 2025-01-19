# Indexes

This section outlines the naming conventions for indexes within our PostgreSQL databases.

## Naming Rules

### 1. Case

- Use **lowercase letters** for all index names.
- Separate words with underscores (`_`). This follows the `snake_case` convention.

### 2. Naming Format

- Use the format `<table_name>_<column_name(s)>_idx` for index names. For composite indexes with more than two columns, include all relevant column names in order of their inclusion in the index. If the resulting name becomes too long, consider abbreviating non-critical words while retaining clarity. This clearly identifies the table and column(s) the index applies to.
- For multi-column indexes, list the columns in the order they are indexed.

```sql
-- Good
CREATE INDEX orders_customer_id_idx ON orders (customer_id);
CREATE INDEX orders_customer_date_idx ON orders (customer_id, order_date);

-- Bad
CREATE INDEX idx_orders_customer_id ON orders (customer_id); -- Less clear
```

### 3. Unique Indexes

- For unique indexes, use the format `<table_name>_<column_name(s)>_uidx` to differentiate them from regular indexes. For composite unique indexes, include all relevant column names in order of their inclusion, ensuring the name reflects the uniqueness enforced by the index. If the name becomes too lengthy, consider meaningful abbreviations to maintain clarity.

```sql
-- Good
CREATE UNIQUE INDEX customers_email_uidx ON customers (email);

-- Bad
CREATE UNIQUE INDEX idx_customers_email ON customers (email); -- Less clear, denotes that this is a regular index
```

### 4. Partial Indexes

- For partial indexes, use the format `<table_name>_<column_name(s)>_pidx`. The condition should be concise but descriptive.

```sql
-- Good
CREATE INDEX orders_customer_pending_pidx ON orders (customer_id) WHERE status = 'pending';

-- Bad
CREATE INDEX idx_orders_pending ON orders (customer_id) WHERE status = 'pending'; -- Less clear
```

### 5. Avoid Redundancy

- Avoid including redundant information like `idx_` or `index_` as prefix. The naming convention already implies the object is an index.

```sql
-- Avoid
CREATE INDEX idx_orders_customer_id ON orders (customer_id);
```

### 6. Length

- Keep index names reasonably short, but prioritize clarity over brevity.

## Rationale

- **Clarity**: The `<table_name>_<column_name(s)>_idx` format immediately identifies the table and column(s) the index applies to.
- **Consistency**: Consistent naming conventions improve the overall consistency of the database schema.
- **Maintainability**: Well-named indexes are easier to understand and manage, especially when dealing with complex schemas.
- **Performance**: Index improves query performance, so clearly identifying their purpose is critical for optimization and troubleshooting.
-

## Examples

**Good Index Definitions**:

```sql
-- Single-column index
CREATE INDEX orders_customer_id_idx ON orders (customer_id);

-- Multi-column index
CREATE INDEX orders_customer_date_idx ON orders (customer_id, order_date);

-- Unique index
CREATE UNIQUE INDEX customers_email_uidx ON customers (email);

-- Partial index
CREATE INDEX orders_customer_pending_pidx ON orders (customer_id) WHERE status = 'pending';
```

**Bad Index Definitions**:

```sql
-- Unclear naming
CREATE INDEX idx_orders_customer_id ON orders (customer_id);

-- Redundant prefix
CREATE INDEX idx_orders_customer_id ON orders (customer_id); -- 'idx_' is redundant

-- No indication of uniqueness
CREATE UNIQUE INDEX idx_customers_email ON customers (email);

-- Unclear condition for partial index
CREATE INDEX idx_orders_pending ON orders (customer_id) WHERE status = 'pending';
```

## Exceptions

- **Legacy Systems**: Existing indexes that don't follow these conventions might require a gradual migration plan.
- **External Systems**: Indexes created for external systems or tools may need to follow different naming conventions.

Document any exceptions to these rules in the [Exceptions](exceptions.md) file.

## Best Practices

- **Analyze Index Usage**: Regularly analyze index usage to ensure indexes are being utilized effectively and remove unused indexes. PostgreSQL's `pg_stat_user_indexes` view is a useful tool for analyzing index usage and determining whether an index is beneficial or redundant.
- **Index Selectivity**: Focus on indexing columns with high selectivity to improve query performance.
- **Avoid Over-Indexing**: Too many indexes can degrade write performance and increase storage requirements. Use indexes judiciously.
- **Use Data Modeler**: Consider using a data modeling tool to help design and visualize your indexes as part of the database schema.
- **Document Index Purpose**: Use the `COMMENT` command in SQL to describe the purpose of each index. For example:
  ```sql
  COMMENT ON INDEX orders_customer_id_idx IS 'Index to speed up queries filtering orders by customer_id';
  ```

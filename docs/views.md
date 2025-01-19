# Views

This section outlines the naming conventions for views within our PostgreSQL databases.

## Naming Rules

### 1. Case

- Use **lowercase letters** for all views.
- Separate words with underscores (`_`). This follows the `snake_case` convention.

### 2. Naming Format

- Use the format `v_<descriptive_name>` for view names. This clearly identifies the object as a view while describing its purpose.

```sql
-- Good
CREATE VIEW v_active_customers AS SELECT * FROM customers WHERE is_active = true;
CREATE VIEW v_monthly_sales_summary AS SELECT ...;

-- Bad
CREATE VIEW vv_active_customers AS SELECT * FROM customers WHERE is_active = true; -- Less descriptive
CREATE VIEW activeCustomers AS SELECT * FROM customers WHERE is_active = true; -- PascalCase
```

### 3. Avoid Other Prefixes

- Avoid adding other prefixes like `vw_` or `view_`. The naming format already implies that the object is a view.

```sql
-- Avoid
CREATE VIEW vw_active_customers AS SELECT * FROM customers WHERE is_active = true;
CREATE VIEW view_monthly_sales_summary AS SELECT ...;
```

### 4. Schema-Qualified Names

- Use schema-qualified names when creating views to organize them logically within the database.

```sql
-- Example
CREATE VIEW reporting.v_active_customers AS SELECT * FROM customers WHERE is_active = true;
```

### 5. Special Characters

- Avoid using special characters other than underscores in view names.

### 6. Length

- Keep view names reasonably short, but prioritize clarity over brevity.

## Rationale

- **Clarity**: The `v_<descriptive_name>` format immediately identifies the object as a view and its purpose.
- **Consistency**: Consistent naming conventions improve the overall consistency of the database schema.
- **Maintainability**: Well-named views are easier to manage, modify, and understand over time.
- **Collaboration**: Clear naming conventions facilitate collaboration among team members and reduce the risk of errors.

## Examples

**Good View Names**:

```sql
CREATE VIEW v_active_customers AS SELECT * FROM customers WHERE is_active = true;
CREATE VIEW v_monthly_sales AS SELECT ...;
CREATE VIEW reporting.v_customer_order_history AS SELECT ...;
```

**Bad View Names**:

```sql
CREATE VIEW active_customers_v AS SELECT * FROM customers WHERE is_active = true; -- Less descriptive
CREATE VIEW activeCustomers AS SELECT * FROM customers WHERE is_active = true; -- PascalCase
CREATE VIEW v_monthly_sales_summary AS SELECT ...; -- Redundant prefix
CREATE VIEW view_customer_orders AS SELECT ...; -- Does not follow v_ convention
```

## Exceptions

- **Legacy Systems**: Existing views that don't follow these conventions might require a gradual migration plan.
- **External Systems**: Views that integrate with external systems might need to follow different naming conventions to maintain compatibility.

Document any exception to these rules in the [Exceptions](exceptions.md) file.

## Best Practices

- **Use Views for Readability**: Use views to simplify complex queries and improve readability for common data retrieval patterns.
- **Avoid Using Views for Writes**: Avoid designing views for writing data; instead, use tables, triggers, or stored procedures for this purpose.
- **Materialized Views for Performance**: When a view involves complex calculations or large datasets, consider using materialized view to improve performance.
- **Document View Purpose**: Use the `COMMENT` command in SQL to describe the purpose of each view. For example:
  ```sql
  COMMENT ON VIEW v_active_customers IS 'View to list all active customers'.
  ```
- **Refreshed Materialized Views**: If using materialized views, ensure they are refreshed regularly to keep the data up-to-date.

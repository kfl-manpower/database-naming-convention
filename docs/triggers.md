# Triggers

This section outlines the naming conventions for triggers within our PostgreSQL database.

## Naming Rules

### 1. Case

- Use **lowercase letters** for all trigger names.
- Separate words with underscores (`_`). This follows the `snake_case` convention.

### 2. Naming Format

- Use the format `trg_<table_name>_<action>_<timing>` for trigger names. This identifies the table, action, and the timing of the trigger.
  - `<action>`: The type of operation that activates the trigger (e.g., `insert`, `update`, `delete`).
  - `<timing>`: The timing of the trigger (e.g., `before`, `after`, `instead_of`).

```sql
-- Good
CREATE TRIGGER trg_orders_before_insert BEFORE INSERT ON orders FOR EACH ROW EXECUTE FUNCTION validate_order();
CREATE TRIGGER trg_customers_after_update AFTER UPDATE ON customers FOR EACH ROW EXECUTE FUNCTION log_customer_changes();

-- Bad
CREATE TRIGGER trg_orders_insert BEFORE INSERT ON orders FOR EACH ROW EXECUTE FUNCTION validate_order(); -- Vague
CREATE TRIGGER beforeInsertOrders BEFORE INSERT ON orders FOR EACH ROW EXECUTE FUNCTION validate_order(); -- Mixed case
```

### 3. Avoid Other Prefixes

- Avoid adding redundant prefixes like `trigger_` to the trigger name. The `trg_` prefix already indicates that it's a trigger.

```sql
-- Avoid
trigger_orders_before_insert -- Redundant prefix
```

### 4. Schema-Qualified Names

- Use schema-qualified names to logically organize triggers, especially when multiple schemas are involved.

```sql
-- Example
CREATE TRIGGER sales.trg_orders_before_insert BEFORE INSERT ON sales.orders FOR EACH ROW EXECUTE FUNCTION validate_order();
```

### 5. Special Characters

- Avoid using special characters other than underscores in trigger names.

### 6. Length

- Keep trigger names reasonably short, but prioritize clarity over brevity.

## Rationale

- **Clarity**: The `trg_<table_name>_<action>_<timing>` format clearly indicates the table, action, and timing associated with the trigger.
- **Consistency**: Consistent naming conventions improve the overall readability and maintainability of the database schema.
- **Maintainability**: Well-named triggers are easier to manage, modify, and debug over time.
- **Scalability**: Clear naming conventions are especially important in large schemas with numerous triggers.

## Examples

**Good Trigger Definitions**:

```sql
-- Before insert trigger
CREATE TRIGGER trg_orders_before_insert BEFORE INSERT ON orders FOR EACH ROW EXECUTE FUNCTION validate_order();

-- After update trigger
CREATE TRIGGER trg_customers_after_update AFTER UPDATE ON customers FOR EACH ROW EXECUTE FUNCTION log_customer_changes();

-- Using schema-qualified name
CREATE TRIGGER sales.trg_orders_before_insert BEFORE INSERT ON sales.orders FOR EACH ROW EXECUTE FUNCTION validate_order();
```

**Bad Trigger Definitions**:

```sql
-- Vague naming
CREATE TRIGGER trg_orders_insert BEFORE INSERT ON orders FOR EACH ROW EXECUTE FUNCTION validate_order();

-- Mixed case and unclear purpose
CREATE TRIGGER beforeInsertOrders BEFORE INSERT ON orders FOR EACH ROW EXECUTE FUNCTION validate_order();

-- Redundant suffix
CREATE TRIGGER trg_orders_insert_trg BEFORE INSERT ON orders FOR EACH ROW EXECUTE FUNCTION validate_order();
```

## Exceptions

- **Legacy Systems**: Existing triggers that don't follow these conventions might require a gradual migration plan.
- **External Systems**: Triggers created for external systems may need to follow different naming conventions to maintain compatibility.

Document any exceptions to these rules in the [Exceptions](exceptions.md) file.

## Best Practices

- **Use Triggers for Automation**: Use triggers to enforce business rules or automate repetitive tasks at the database level.
- **Versioning and Documentation**: When modifying triggers, consider versioning them (e.g., `trg_orders_before_insert_v2`) to ensure backward compatibility while making improvements. Maintain clear documentation of changes to facilitate understanding and collaboration among team members.
- **Comment Your Triggers**: Use the `COMMENT` command in SQL to describe the purpose of each trigger. For example:
  ```sql
  COMMENT ON TRIGGER trg_orders_before_insert ON orders IS 'Validates orders before inserting them into the orders table. Ensures all mandatory fields are populated and checks for duplicate entries.';
  ```
- **Avoid Overusing Triggers**: Use triggers judiciously, as excessive use can make the database harder to debug and maintain.
- **Secure Trigger Functions**: Ensure that the functions executed by triggers are optimized and secure to prevent performance bottlenecks or vulnerabilities.
- **Test Thoroughly**: Validate all scenarios to ensure the trigger behaves as expected under various conditions.

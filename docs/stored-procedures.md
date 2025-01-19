# Stored Procedures

This section outlines the naming conventions for stored procedures within our PostgreSQL databases.

## Naming Rules

### 1. Case

- Use **lowercase letters** for all stored procedure names.
- Separate words with underscores (`_`). This follows the `snake_case` convention.

### 2. Naming Format

- Use the format `p_<action>_<object>`.
- Use descriptive names that clearly convey the procedure's purpose.
- Begin with a verb to indicate the action performed by the procedure (e.g., `p_insert`, `p_update`, `p_process`).
- For helper or utility procedures, consider using a prefix like `util_` or `helper_` to distinguish them from domain-specific procedures.
- Optionally, prefix the procedure with a module or category if relevant (e.g., `billing_process_payment).

```sql
-- Good
CREATE PROCEDURE p_insert_customer(name VARCHAR, email VARCHAR) LANGUAGE plpgsql AS $$ ... $$;
CREATE PROCEDURE p_update_order_status(order_id INT, status VARCHAR) LANGUAGE plpgsql AS $$ ... $$;

-- Bad
CREATE PROCEDURE proc_insert_customer(name VARCHAR, email VARCHAR) LANGUAGE plpgsql AS $$ ... $$; -- Doesn't follow the convention p_
CREATE PROCEDURE InsertCustomer(name VARCHAR, email VARCHAR) LANGUAGE plpgsql AS $$ ... $$; -- PascalCase
```

### 3. Avoid Other Prefixes

- Avoid adding other prefixes like `proc_` or `procedure_`. The naming format already implies it is a stored procedure.

```sql
-- Avoid
CREATE PROCEDURE proc_update_inventory(item_id INT, quantity INT) LANGUAGE plpgsql AS $$ ... $$;
```

### 4. Schema-Qualified Names

- Use schema-qualified names to logically organize procedures, especially when multiple schemas are involved.

```sql
-- Example
CREATE PROCEDURE hr.p_insert_employee(name VARCHAR, department VARCHAR) LANGUAGE plpgsql AS $$ ... $$;
```

### 5. Special Characters

- Avoid using special characters other than underscores in procedure names.

### 6. Length

- Keep procedure names reasonably short, but prioritize clarity over brevity.

## Parameters

### 1. Naming Parameters

- Use **lowercase letters** for all parameter names.
- Separate words with underscores (`_`).
- Use descriptive names that indicate the purpose of the parameter.

```sql
-- Good
CREATE PROCEDURE p_process_refund(order_id INT, refund_amount NUMERIC) LANGUAGE plpgsql AS $$ ... $$;

-- Bad
CREATE PROCEDURE process_ref(order INT, amt NUMERIC) LANGUAGE plpgsql AS $$ ... $$; -- Vague parameter names
```

### 2. Default Values

- PostgreSQL does not currently support default parameter values in stored procedures. Consider using wrapper functions if default values are required.

```sql
-- Wrapper function providing a default value for a parameter
CREATE FUNCTION p_insert_customer_with_defaults(name VARCHAR, email VARCHAR DEFAULT 'unknown@example.com') RETURNS VOID AS $$
BEGIN
    CALL insert_customer(name, email);
END;
$$ LANGUAGE plpgsql;
```

## Rationale

- **Clarity**: Descriptive names make it immediately clear what the procedure does.
- **Consistency**: Consistent naming conventions improve the readability and maintainability of the database schema.
- **Reusability**: Clear and logical procedure names encourage reuse across different parts of the application.

## Examples

**Good Stored Procedure Definitions**:

```sql
-- Insert operation
CREATE PROCEDURE p_insert_customer(name VARCHAR, email VARCHAR) LANGUAGE plpgsql AS $$
BEGIN
    INSERT INTO customers (name, email) VALUES (name, email);
END;
$$;

-- Update operation
CREATE PROCEDURE p_update_order_status(order_id INT, status VARCHAR) LANGUAGE plpgsql AS $$
BEGIN
    UPDATE orders SET status = status WHERE id = order_id;
END;
$$;

-- Using schema-qualified name
CREATE PROCEDURE billing.p_process_payment(order_id INT, amount NUMERIC) LANGUAGE plpgsql AS $$
BEGIN
    -- Payment logic here
END;
$$;
```

**Bad Stored Procedure Definitions**:

```sql
-- Unclear purpose
CREATE PROCEDURE proc_order_status_update(order_id INT, status VARCHAR) LANGUAGE plpgsql AS $$ ... $$;

-- Mixed case
CREATE PROCEDURE UpdateOrderStatus(order_id INT, status VARCHAR) LANGUAGE plpgsql AS $$ ... $$;

-- Vague parameter names
CREATE PROCEDURE process_ref(order INT, amt NUMERIC) LANGUAGE plpgsql AS $$ ... $$;
```

## Exceptions

- **Legacy Systems**: Existing procedures that don't follow these conventions might require a gradual migration plan.
- **External Systems**: Procedures designed to integrate with external systems may need to follow different naming conventions.

Document any exceptions to these rules in the [Exceptions](exceptions.md) file.

## Best Practices

- **Use Procedures for Encapsulation**: Encapsulate complex or repetitive database operations within stored procedures to simplify application logic.
- **Versioning and Deprecation**: When updating stored procedures, consider versioning them (e.g., `process_payment_v2`) to allow existing code to continue using the old version. Clearly document deprecation timelines and notify stakeholders about planned changes to ensure smooth transitions.
- **Comment Your Procedure**: Use the `COMMENT` command in SQL to describe the purpose of each procedure. For example:
  ```sql
  COMMENT ON PROCEDURE p_insert_customer IS 'Inserts a new customer into the customers table.';
  ```
- **Optimize for Performance**: Analyze and tune procedures to ensure optimal performance, especially for operations involving large datasets.
- **Secure Procedure**: Use `SECURITY DEFINER` carefully and ensure procedures are designed to prevent privilege escalation.
- **Test Thoroughly**: Validate all edge cases and scenarios to ensure the procedure behaves as expected.

# Functions

This section outlines the naming conventions for functions within our PostgreSQL databases.

## Naming Rules

### 1. Case

- Use **lowercase letters** for all function names.
- Separate words with underscores (`_`). This follows the `snake_case` convention.

### 2. Naming Format

- Use descriptive names that clearly convey the function's purpose. For helper or utility functions, consider using a prefix like `util_` or `helper_` to distinguish their role.
- Begin with a verb to indicate the action performed by the function (e.g., `get`, `calculate`, `generate`).
- Optionally, prefix the function name with a module or category if relevant (e.g., `math_calculate_average`).

```sql
-- Good
CREATE FUNCTION calculate_total_price(price NUMERIC, quantity INT) RETURNS NUMERIC AS $$ ... $$;
CREATE FUNCTION get_customer_by_id(customer_id INT) RETURNS RECORD AS $$ ... $$;

-- Bad
CREATE FUNCTION calc_price(price NUMERIC, quantity INT) RETURNS NUMERIC AS $$ ... $$; -- Vague abbreviation
CREATE FUNCTION CustomerDetails(customer_id INT) RETURNS RECORD AS $$ ... $$; -- PascalCase
```

### 3. Avoid Other Prefixes

- Avoid adding other prefixes like `fn_` or `function_`. The naming format already indicates it is a function.

```sql
-- Avoid
CREATE FUNCTION fn_get_customer_by_id(customer_id INT) RETURNS RECORD AS $$ ... $$;
```

### 4. Schema-Qualified Names

- Use schema-qualified names to logically organize functions, especially when multiple schemas are involved. This is particularly useful in scenarios such as multi-tenant applications, where schemas separate data for different tenants, or modular databases, where schemas group related functions logically.

```sql
-- Example
CREATE FUNCTION analytics.calculate_growth_rate(current_sales NUMERIC, previous_sales NUMERIC) RETURNS NUMERIC AS $$ ... $$;
```

### 5. Special Characters

- Avoid using special characters other than underscores in function names.

### 6. Length

- Keep function names reasonably short, but prioritize clarity over brevity.

## Parameters

### 1. Naming Parameters

- Use **lowercase letters** for all parameter names.
- Separate words with underscores (`_`).
- Use descriptive names that indicate the purpose of the parameter.

```sql
-- Good
CREATE FUNCTION calculate_discounted_price(price NUMERIC, discount_rate NUMERIC) RETURNS NUMERIC AS $$ ... $$;

-- Bad
CREATE FUNCTION calculate_discount(price NUMERIC, dr NUMERIC) RETURNS NUMERIC AS $$ ... $$; -- Vague parameter name
```

### 2. Default Values

- When appropriate, provide default values for parameters to simplify function calls and reduce repetitive code. This approach enhances usability by allowing functions to handle optional parameters seamlessly.

```sql
-- Example
CREATE FUNCTION calculate_tax(amount NUMERIC, tax_rate NUMERIC DEFAULT 0.05) RETURNS NUMERIC AS $$ ... $$;
```

## Rationale

- **Clarity**: Descriptive names make it immediately clear what the function does.
- **Consistency**: Consistent naming conventions improve the readability and maintainability of the database schema.
- **Maintainability**: Well-named functions are easier to understand and modify over time.
- **Reusability**: Clear and logical function names encourage reuse across different parts of the application.

## Examples

**Good Function Definitions**:

```sql
-- Simple calculation
CREATE FUNCTION calculate_total_price(price NUMERIC, quantity INT) RETURNS NUMERIC AS $$
BEGIN
    RETURN price * quantity;
END;
$$ LANGUAGE plpgsql;

-- Fetching data
CREATE FUNCTION get_customer_by_id(customer_id INT) RETURNS RECORD AS $$
BEGIN
    RETURN QUERY SELECT * FROM customers WHERE id = customer_id;
END;
$$ LANGUAGE plpgsql;

-- Using default parameter values
CREATE FUNCTION calculate_tax(amount NUMERIC, tax_rate NUMERIC DEFAULT 0.05) RETURNS NUMERIC AS $$
BEGIN
    RETURN amount * tax_rate;
END;
$$ LANGUAGE plpgsql;
```

**Bad Function Definitions**:

```sql
-- Vague naming
CREATE FUNCTION calc_price(price NUMERIC, quantity INT) RETURNS NUMERIC AS $$ ... $$;

-- Mixed case and unclear purpose
CREATE FUNCTION CustomerDetails(customer_id INT) RETURNS RECORD AS $$ ... $$;

-- Redundant prefix
CREATE FUNCTION fn_calculate_price(price NUMERIC, quantity INT) RETURNS NUMERIC AS $$ ... $$;
```

## Exceptions

- **Legacy Systems**: Existing functions that don't follow these conventions might require a gradual migration plan.
- **External Systems**: Functions designed to integrate with external systems may need to follow different naming conventions.

Document any exceptions to these rules in the [Exceptions](exceptions.md) file.

## Best Practices

- **Use Functions for Modular Logic**: Encapsulate reusable logic in functions to simplify queries and improve maintainability.
- **Versioning and Deprecation**: When updating a function, consider creating a new version (e.g., `calculate_tax_v2`) while retaining the old one to maintain backward compatibility. Clearly document deprecation timelines and communicate changes to stakeholders.
- **Comment Your Functions**: Use the `COMMENT` command in SQL to describe the purpose of each function. For example:
  ```sql
  COMMENT ON FUNCTION calculate_total_price IS 'Calculates the total price based on price and quantity.';
  ```
- **Optimize for Performance**: Analyze and tune functions to ensure optimal performance, especially for complex calculations or queries.
- **Secure Functions**: When using functions with `SECURITY DEFINER`, ensure the functions is designed to prevent privilege escalation.
- **Test Functions Thoroughly**: Validate all edge cases and scenarios to ensure the function behaves as expected.

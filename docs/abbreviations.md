# Abbreviations

This section outlines the approved abbreviations for use in PostgreSQL database objects. Consistent use of abbreviations ensure clarity and maintainability while keeping object names concise.

## General Guidelines

### 1. Consistency

- Use abbreviations consistently and across all database objects (e.g., tables, columns, sequences).
- Avoid mixing full words and abbreviations for the same term within the same schema.

### 2. Clarity

- Only use abbreviations when they are widely recognized and understood within the context of your database.
- Avoid overly ambiguous or uncommon abbreviations.

### 3. Simplicity

- Limit the number of abbreviations used in a single object name to maintain readability.
- Prioritize clarity over brevity when an abbreviation might cause confusion.

## Approved Abbreviations

| Full Word  | Abbreviation | Example                    |
| ---------- | ------------ | -------------------------- |
| Number     | no           | `invoice_no`, `order_no`   |
| Reference  | ref          | `ref_code`, `customer_ref` |
| Temporary  | tmp          | `tmp_users`, `tmp_data`    |
| Aggregate  | agg          | `agg_sales`, `agg_data`    |
| Quantity   | qty          | `product_qty`, `order_qty` |
| Amount     | amt          | `total_amt`, `refund_amt`  |
| Identifier | id           | `customer_id`, `order_id`  |

## Rationale

- **Readability**: Consistent and approved abbreviations make object names concise while maintaining clarity.
- **Maintainability**: Documenting and adhering to a standard list of abbreviations reduces confusion among team members.
- **Scalability**: Clear abbreviations help in maintaining large schemas with many objects.

## Examples

**Good Usage**:

```sql
-- Using approved abbreviations
CREATE TABLE orders (
    order_id SERIAL PRIMARY KEY,
    customer_ref VARCHAR(255),
    product_qty INT,
    total_amt NUMERIC(10, 2)
);
```

**Bad Usage**:

```sql
-- Mixing abbreviations and full words inconsistently
CREATE TABLE orders (
    orderID SERIAL PRIMARY KEY, -- Mixed case and unclear abbreviation
    customer_reference VARCHAR(255), -- Full word used inconsistently
    product_quantity INT, -- Full word instead of approved abbreviation
    totalAmount NUMERIC(10, 2) -- Mixed case and unclear abbreviation
);
```

## Exceptions

- **Legacy Systems**: Existing objects that don't follow these conventions might require a gradual migration plan.
- **External Systems**: When integrating with external systems, follow the naming conventions required for compatibility.

Document any exceptions to these rules in the [Exceptions](exceptions.md) file.

## Best Practices

- **Document Project-Specific Abbreviations**: Maintain a team-accessible document listing all approved abbreviations for your project.
- **Review Abbreviation Usage**: Regularly review database objects for adherence to the approved abbreviation list.
- **Prioritize Clarity**: When in doubt, use the full word rather than an ambiguous abbreviation.
- **Avoid Over-Abbreviation**: Overusing abbreviations can make object names cryptic and hard to interpret.

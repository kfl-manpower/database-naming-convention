# Sequences

This section outlines the naming conventions for sequences within our PostgreSQL databases.

## Naming Rules

### 1. Case

- Use **lowercase letters** for all sequence names.
- Separate words with underscores (`_`). This follows the `snake_case` convention.

### 2. Naming Format

- Use the format `seq_<table_name>_<column_name>` for sequence names. If multiple sequences are associated with the same table, include additional context in the name to distinguish their purpose (e.g., `seq_orders_order_id`, `seq_orders_invoice_id`). This identifies the table and column associated with the sequence.

```sql
-- Good
CREATE SEQUENCE seq_customers_id;
CREATE SEQUENCE seq_orders_order_id;

-- Bad
CREATE SEQUENCE customers_id_seq; -- Suffix instead of prefix
CREATE SEQUENCE orders_seq_id; -- Incorrect word order
```

### 3. Avoid Other Prefixes

- Avoid adding other prefixes like `sequence_`. The `seq_` prefix is sufficient to identify the object as a sequence.

```sql
-- Avoid
CREATE SEQUENCE sequence_customers_id;
```

### 4. Schema-Qualified Names

- Use schema-qualified names to logically organize sequences, especially when multiple schemas are involved.

```sql
-- Example
CREATE SEQUENCE hr.seq_employees_id;
```

### 5. Special Characters

- Avoid using special characters other than underscores in sequence names.

### 6. Length

- Keep sequence names reasonably short, but prioritize clarity over brevity.

## Rationale

- **Clarity**: The `seq_<table_name>_<column_name>` format clearly indicates the table and column associated with the sequence.
- **Consistency**: Consistent naming conventions improve the overall readability and maintainability of the database schema.
- **Maintainability**: Well-named sequences are easier to manage and debug.
- **Scalability**: Clear naming conventions are especially important in large schemas with numerous sequences.

## Examples

**Good Sequence Definitions**:

```sql
-- Sequence for a single table column
CREATE SEQUENCE seq_customers_id;

-- Sequence for a schema-qualified table
CREATE SEQUENCE hr.seq_employees_id;
```

**Bad Sequence Definitions**:

```sql
-- Suffix instead of prefix
CREATE SEQUENCE customers_id_seq;

-- Redundant prefix
CREATE SEQUENCE sequence_customers_id;

-- Mixed case and unclear purpose
CREATE SEQUENCE CustomersIDSeq;
```

## Exceptions

- **Legacy Systems**: Existing sequences that don't follow these conventions might require a gradual migration plan.
- **External Systems**: Sequences created for external systems may need to follow different naming conventions.

Document any exceptions to these rules in the [Exceptions](exceptions.md) file.

## Best Practices

- **Use Sequences for Unique Identifiers**: Sequences are ideal for scenarios where sequential IDs are required, such as maintaining order, simplifying debugging, or ensuring predictable ID generation. For example, sequences work well in logging tables, invoice numbering, or cases where database-level performance is critical. Consider using UUIDs instead for distributed systems where globally unique identifiers are needed. Use sequences to generate unique identifiers for primary key columns or other unique fields.
- **Comment Your Sequences**: Use the `COMMENT` command in SQL to describe the purpose of each sequence. For example:
  ```sql
  COMMENT ON SEQUENCE seq_customers_id IS 'Sequence to generate unique IDs for the customers table. This sequence is tied to the id column of the customers table and is used to ensure unique identification. It can also be extended to composite key scenarios or shared across tables in advanced schemas, as needed.';
  ```
- **Audit Usage**: Regularly audit sequences to ensure they are actively used and associated with the intended tables.
- **Avoid Unnecessary Sequences**: Avoid creating sequences for columns where unique values can be derived using other methods, such as UUIDs.
- **Monitor Sequence Values**: Monitor sequence values to ensure they do not approach their limits, especially for `SMALLINT` or `INTEGER` sequences.

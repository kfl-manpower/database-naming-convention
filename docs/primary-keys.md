# Primary Keys

This section outlines the naming conventions for primary keys within our PostgreSQL databases.

## Naming Rules

### 1. Case

- Use **lowercase letters** for all primary key constraint names.
- Separate words with underscores (`_`). This follows the `snake_case` convention.

```sql
-- Good
CREATE TABLE customers (
    id SERIAL,
    first_name VARCHAR(255),
    CONSTRAINT customers_pk PRIMARY KEY (id)
);

-- Bad
CREATE TABLE products (
    product_id INT,
    name VARCHAR(255),
    CONSTRAINT PK_Products PRIMARY KEY (product_id)
);
```

### 2. Constraint Name Format

- Use the format `<table_name>_pk` for primary key constraint names. This aligns with PostgreSQL standards and ensures consistency across constraint naming. This clearly identifies the constraint as a primary key and associate it with the correct table.

```sql
-- Good
CREATE TABLE orders (
    order_id INT,
    customer_id INT,
    CONSTRAINT pk_orders PRIMARY KEY (order_id)
);

-- Less explicit
CREATE TABLE order_items (
    order_int INT,
    product_id INT,
    CONSTRAINT order_items_pk PRIMARY KEY (order_id, product_id) -- Not immediately clear it's a primary key
);
```

### 3. Column Name

- **Surrogate Keys**: If you are using a surrogate key (e.g., an auto-incrementing integer or UUID) as the primary key, prefer using `id` when the table name is clear and unambiguous in the schema context. Use `<table_name>_id` when clarity is needed, especially in schemas with many interrelated tables or where table names may be ambiguous (e.g., `customer_id`, `order_id`).

  - `id` is concise and commonly used for surrogate keys.
  - `<table_name>_id` (e.g., `customer_id`, `order_id`) can provide more context, especially when dealing with many tables. Choose one style and be consistent.

  ```sql
  -- Good (using 'id')
  CREATE TABLE customers (
      id SERIAL PRIMARY KEY,
      ...
  );

  -- Also good (using 'table_name_id')
  CREATE TABLE products (
      product_id UUID PRIMARY KEY,
      ...
  );
  ```

- **Natural Keys**: If you are using a natural key (a column or a set of columns that already uniquely identifies an entity), you can use the existing column name(s) as the primary key. However, be sure the natural key truly guarantees uniqueness and is unlikely to change in the future.
  ```sql
  -- Example of a natural key (assucming 'product_code' is unique and stable)
  CREATE TABLE products (
      product_code VARCHAR(20) PRIMARY KEY,
      ...
  )
  ```

### 4. Avoid Prefixes for Column Names

- Avoid adding prefixes like `pk_` to the primary key column name itself. Prefixes can make column names unnecessarily verbose and redundant, as the constraint name already indicates that it's a primary key.
- Relying on descriptive constraint naming (e.g., `<table_name>_pk`) ensures clarity without adding redundancy to the column name.

```sql
-- Avoid
CREATE TABLE customers (
    pk_customer_id SERIAL PRIMARY KEY, -- Redundant prefix
    ...
);
```

### 5. Special Characters

- Avoid using special characters other than underscores in primary key constraint names or column names.

### 6. Length

- Keep primary key constraint names reasonably short, but prioritize clarity over brevity.

## Rationale

- **Clarity**: The `pk_<table_name>` format clearly identifies the constraint as a primary key and the table it belongs to.
- **Consistency**: Consistent naming conventions improve the overall consistency of the database schema and make it easier to understand.
- **Maintainability**: Well-named primary keys are easier to manage and modify over time.
- **Uniqueness**: Primary keys enforce uniqueness, ensuring data integrity.

## Examples

**Good Primary Key Definitions**:

```sql
-- Using a surrogate key ('id')
CREATE TABLE customers (
    id SERIAL PRIMARY KEY,
    first_name VARCHAR(255) NOT NULL,
    last_name VARCHAR(255) NOT NULL
);

-- Using a surrogate key ('table_name_id')
CREATE TABLE products (
    product_id UUID PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    price NUMERIC(10, 2) NOT NULL
);

-- Using a natural key
CREATE TABLE countries (
    country_code CHAR(2) PRIMARY KEY,
    country_name VARCHAR(255) NOT NULL
);

-- Composite primary key (using existing columns)
CREATE TABLE order_items (
    order_id INT,
    product_id INT,
    quantity INT,
    PRIMARY KEY (order_id, product_id)
);
```

**Bad Primary Key Definitions**:

```sql
-- Constraint name not informative, mixed case
CREATE TABLE employees (
    employeeId INT PRIMARY KEY,
    ...
);

-- Redundant prefix on column name
CREATE TABLE suppliers (
    pk_supplier_id INT PRIMARY KEY,
    ...
);
```

## Exceptions

- **Legacy Systems**: Existing primary keys that don't follow these conventions might require a gradual migration plan.

Document any exception to these rules in the [Exceptions](exceptions.md) file.

## Best Practices

- **Choose Surrogate Keys Wisely**: Surrogate keys are often preferred for their simplicity and performance benefits. Consider using them unless you have a strong reason to use a natural key.
  - **When to Use Natural Keys**: Natural keys are better suited for lookup tables (e.g., `countries`, `currencies`) or cases where a column is guaranteed to remain unique and stable over time (e.g., `email` for users, `product_code` for products). These scenarios reduce redundancy and provide meaningful identifiers directly tied to the entity.
- **Keep Primary Keys Short**: Shorter primary key values (e.g., integers) generally lead to better performance for indexing and joins.
- **Define Primary Keys Explicitly**: Always explicitly define primary keys when creating tables, even if you are using a column named `id`.
- **Use Data Modeler**: Consider using a data modeling tool to help design and visualize your database schema, including your primary key definitions.

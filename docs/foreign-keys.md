# Foreign Keys

This section outlines the naming conventions for foreign keys within our PostgreSQL databases.

## Naming Rules

### 1. Case

- Use **lowercase letters** for all foreign key constraint names.
- Separate words with underscores (`_`). This follows the `snake_case` convention.

### 2. Constraint Name Format

- Use the format `<table_name>_<referenced_table_name>_fk` for foreign key constraint names. This format helps maintain clarity in complex schemas with many relationships by explicitly identifying both the referencing table and referenced table, reducing ambiguity. This clearly identifies the constraint as a foreign key, the table it belongs to, and the table it references.

```sql
-- Good
CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    customer_id INTEGER,
    CONSTRAINT orders_customers_fk FOREIGN KEY (customer_id) REFERENCES customers(id)
);

-- Less explicit
CREATE TABLE order_items (
    order_id INT,
    product_id INT,
    CONSTRAINT order_items_product_id_fkey FOREIGN KEY (product_id) REFERENCES products(id) -- Not immediately clear which tables are invovled
)
```

### 3. Column Name

- Name the foreign key column(s) in the referencing table using the format `<referenced_table_name>_fk`. This makes the relationship between the tables clear and easy to understand.
- For composite foreign keys, include all relevant referenced columns in the naming format. For example, if referencing both `order_id` and `product_id`, use `order_product_id` or `order_id_product_id` for clarity. This makes the relationship between the tables clear and easy to understand.

```sql
-- Good
CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    customer_id INTEGER REFERENCES customer(id), -- Clearly indicates the relationship
    order_date DATE
);

-- Less clear
CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    cust_id INTEGER REFERENCES customer(id), -- Relationship is less obvious
    order_date DATE
);
```

### 4. Avoid Prefixes for Column Names

- Avoid adding prefixes like `fk_` to the foreign key column name itself. The constraint name already indicates that it's a foreign key.

```sql
-- Avoid
CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    fk_customer_id, INTEGER REFERENCES customers(id), -- Redundant prefix
    order_date DATE
);
```

### 5. Special Characters

- Avoid using special characters other than underscore in foreign key constraint names or column names.

### 6. Length

- Keep foreign key constraint names reasonably short, but prioritize clarity over brevity. The `<table_name>_<referenced_table_name>_fk` format can lead to longer names, but the clarity it provides is generally worth it.

## Rationale

- **Clarity**: The `<table_name>_<referenced_table_name>_fk` format immediately identifies the constraint as a foreign key, the referencing table, and the referenced table. The `<referenced_table_name>_id` format for column names makes the relationship very clear.
- **Consistency**: Consistent naming conventions improve the overall consistency of the database schema.
- **Maintainability**: Well-named foreign keys are easier to understand and manage, especially when dealing with complex relationships.
- **Referential Integrity**: Foreign keys enforce referential integrity, ensuring that relationships between tables remain valid.

## Examples

**Good Foreign Key Definitions**:

```sql
-- Clear constraint name and column name
CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    customer_id INTEGER NOT NULL,
    order_date DATE NOT NULL,
    CONSTRAINT orders_customers_fk FOREIGN KEY (customer_id) REFERENCES customers(id)
);

-- Another example with multiple foreign keys
CREATE TABLE order_items (
    order_id INTEGER NOT NULL,
    product_id INTEGER NOT NULL,
    quantity INTEGER NOT NULL,
    CONSTRAINT order_items_orders_fk FOREIGN KEY (order_id) REFERENCES orders(id),
    CONSTRAINT order_items_products_fk FOREIGN KEY (product_id) REFERENCES products(id),
    PRIMARY KEY (order_id, product_id)
)
```

**Bad Foreign Key Definitions**:

```sql
-- Unclear constraint name, mixed case
CREATE TABLE Orders (
    Id SERIAL PRIMARY KEY,
    CustomerID INTEGER,
    OrderDate DATE,
    CONSTRAINT OrdersCustomersFK FOREIGN KEY (CustomerID) REFERENCES Customers(Id)
);

-- Redundant prefix on column name, abbreviation
CREATE TABLE Products (
    ProdID SERIAL PRIMARY KEY,
    fk_CatID INTEGER,
    ProductName VARCHAR(255),
    CONSTRAINT Products_Categories_fk FOREIGN KEY (fk_CatID) REFERENCES Categories(CatID)
);
```

## Exceptions

- **Legacy Systems**: Existing foreign keys that don't follow these conventions might need a gradual migration plan.
- **External Systems**: Foreign keys that references tables in external systems might need to follow different naming conventions to maintain compatibility.

Document any exceptions to these rules in the [Exceptions](exceptions.md) file.

## Best Practices

- **Define Foreign Keys Explicitly**: Always explicitly define foreign keys when creating tables to enforce referential integrity.
- **Document Foreign Key Relationships**: Use comments in SQL to describe the purpose of each foreign key, making it easier for developers and stakeholders to understand the relationships between tables. For example:
  ```sql
  COMMENT ON CONSTRAINT orders_customers_fk IS 'Links orders to the customers table by customer id'
  ```
- **Choose Appropriate ON DELETE and ON UPDATE Actions**: Carefully consider the `ON DELETE` and `ON UPDATE` actions for each foreign key (e.g., `CASCADE`, `SET NULL`, `RESTRICT`, `NO ACTION`). Choose the actions that best suit your application's requirement for data consistency.
- **Index Foreign Key Columns**: Create indexes on foreign key columns to improve the performance of queries that join related tables.
- **Use a Data Modeler**: Consider using a data modeling tool to help design and visualize your database schema, including foreign key relationships.

# Tables

This section outlines the naming conventions for tables within our PostgreSQL databases.

## Naming Rules

### 1. Case

- Use **lowercase letters** for all table names.
- Separate words with underscores (`_`). This follows the `snake_case` convention.

```sql
-- Good
customers
order_items
product_categories

-- Bad
OrderItems -- (PascalCase)
customer-orders -- (Hyphen instead of underscore)
CustomerOrders -- (No separation)
```

### 2. Plural Nouns

- Use **plural nouns** for table names. This reflects that tables typically store collections of entities, aligning with the concept that each row represents a single instance of the entity, and the table as a whole represents a collection.

```sql
-- Good
employees
departments
products
invoices

-- Bad
employee
department
product
invoice
```

### 3. Descriptive Names

- Choose names that clearly and concisely describe the entities stored in the table.
- The name should be meaningful to developers, database administrators, and anyone else who might interact with the database.

```sql
-- Good - Clearly indicates the entities stored
users
transactions
purchase_orders

-- Bad - Vague or unclear
data
table1
stuff
```

### 4. Avoid Prefixes

- Avoid using redundant prefixes like `tbl_` for table names. The context already implies that it's a table.

```sql
-- Good
orders

-- Bad
tbl_orders
```

### 5. Schema-Qualified Names

- If you are using schemas to organize tables (which is recommended), you can prefix the table name with schema name when creating or referencing the table.
- Use this only when necessary to avoid ambiguity or if you need to reference a table outside of the current `search_path`.
- Managing the `search path` effectively is crucial to avoid ambiguity when working with schema-qualified table names. For example explicitly setting the `search_path` can ensure queries target the correct schema:
  ```sql
  SET search_path TO sales, public;
  ```

```sql
-- Example: Creating a table in the 'sales' schema
CREATE TABLE sales.order_items (
    ...
);
```

### 6. Avoid Reserved Words

- Do not use PostgreSQL reserved keywords as table names.

```sql
-- Bad - 'user' and 'order' are reserved words
CREATE TABLE user ( -- Incorrect
    ...
);

CREATE TABLE order ( -- Incorrect
    ...
);
```

### 7. Special Characters

- Avoid using special characters other than underscores in table names.

### 8. Length

- Keep table names reasonably short, but prioritize clarity over brevity.
- Avoid overly long names that are difficult to type or remember.

## Rationale

- **Readability**: Using plural nouns and `snake_case` makes table names easier to read and understand, especially when used in SQL queries.
- **Consistency**: Consistent naming conventions improve the overall consistency of the database schema.
- **Maintainability**: Well-named tables are easier to maintain and modify over time.
- **Collaboration**: Clear naming conventions facilitate collaboration among team members and reduce the risk of errors.

## Examples

**Good Table Names**:

- `customers`
- `products`
- `orders`
- `order_items`
- `employees`
- `departments`
- `sales.invoices` (if using schemas)

**Bad Table Names**:

- `tblCustomers` (Unnecessary prefix, PascalCase)
- `customer` (Singular noun)
- `Cust_Orders` (Abbreviation, mixed case)
- `data` (Vague and not descriptive)
- `order` (Reserved word)

## Exceptions

- **Legacy Systems**: Existing tables that don't follow these conventions might require a gradual migration plan rather than an immediate rename.
- **External System**: Tables that need to integrate with external systems might need to follow different naming conventions to maintain compatibility.

Document any exceptions to these rules in the [Exceptions](exceptions.md) file.

## Best Practices

- **Use a Data Modeler**: Consider using a data modeling tool to help you design and visualize your database schema. This can help you choose meaningful and consistent table names.
- **Standardize Join Table Naming**: For many-to-many relationships, use standardized naming conventions such as alphabetical order of the entities involved (e.g., `products_categories` or `students_courses`).
- **Define Constraints**: When creating tables, always define appropriate primary keys, foreign keys, and other constraints to ensure data integrity. Refer to the sections on [Primary Keys](primary-keys.md) and [Foreign Keys](foreign-keys.md) for naming conventions for those objects.
- **Document Your Schema**: Create a dictionary or other documentation that describes the purpose of each table and its columns.

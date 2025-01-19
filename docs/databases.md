# Databases

This section outlines the naming conventions for databases within our PostgreSQL environment.

## Naming Rules

### 1. Case

- Use **lowercase letters** for all database names.
- Separate words with underscores (`_`). This follows the `snake_case` convention.

```sql
-- Good
client_data
inventory_management
reporting_system

-- Bad
ClientData -- (PascalCase)
client-data -- (Hyphen instead of underscore)
clientdata -- (No separation)
```

### 2. Descriptive Names

- Choose names that clearly and concisely describe the purpose or content of the database.
- The name should be meaningful to developers, database administrators, and anyone else who might interact with the database.

```sql
-- Good - Clearly indicates the purpose
sales_transactions
user_authentication
product_catalog

-- Bad - Vague or unclear
db1
data_store
my_database
```

### 3. Avoid Prefixes

- In most cases, avoid using prefixes like `db_` or `database_` as they are redundant. The context already implies that it's a database.

```sql
-- Good
client_data

-- Bad
db_client_data
database_client_data
```

### 4. Project or Application Specific Suffix (Optional)

- If you have multiple databases related to different projects or applications, consider adding a project-specific suffix to the database name for clarity. Only use if there is a real risk of name collision or confusion otherwise.
- For instance, in multi-tenant systems where each tenant has its own database, project-specific suffixes can help distinguish between tenants.

```sql
-- Example: Multiple projects or tenants
client_data_tenant1
client_data_tenant2
projextx_sales
projecty_marketing
```

### 5. Environment-Specific Suffix (Optional)

- If you need to distinguish between databases for different environments (e.g., development, testing, production), use a clear and consistent suffix.
- Consider the following standards for these suffixes to ensure uniformity:
  - `_dev` for development
  - `_test` for testing
  - `_prod` for production

```sql
-- Example: Different environments
client_data_dev
client_data_test
client_data_prod
```

### 6. Avoid Reserved Words

- Do not use PostgreSQL reserved keywords as database names.

```sql
-- Bad - `user` and `order` are reserved words
user
order
```

### 7. Special Characters

- Avoid using special characters other than underscores in database names.

### 8. Length

- Keep database names reasonably short, but prioritize clarity or brevity.
- Avoid overly long names that are difficult to type or remember.
- Very long database names can be cumbersome to work with in scripts and command-line tools.

## Rationale

- **Consistency**: Using `snake_case` for database names ensures consistency with other database objects (tables, columns, etc.), making the overall schema easier to read and understand.
- **Clarity**: Descriptive names improve the clarity of the database's purpose and make it easier for developers to work with the correct database.
- **Maintainability**: Well-named databases are easier to manage, back up, restore, and migrate.
- **Collaboration**: Consistent naming conventions facilitate collaboration among team members and reduce the risk of errors.

## Examples

**Good Database Names**:

- `sales_data`
- `employee_records`
- `project_management`
- `inventory_control-dev`
- `analytics_reporting_prod`

**Bad Database Names**:

- `db_SalesData` (Mixed case, unnecessary prefix)
- `my_database` (Vague and not descriptive)
- `DataStore` (Mixed case, not specific enough)
- `user` (Reserved word)

## Exceptions

- **Legacy Systems**: Existing databases that don't follow these conventions might require a gradual migration plan rather than an immediate rename.
- **External Systems**: Databases that need to integrate with external systems might need to follow different naming conventions to maintain compatibility.

Any exceptions to these rules should be documented in the [Exceptions](exceptions.md) file.

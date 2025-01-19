# Schemas

This section outlines the naming conventions for schemas within our PostgreSQL databases.

## Naming Rules

### 1. Case

- Use **lowercase letters** for all schema names.
- Separate words with underscores (`_`). This follows the `snake_case` convention.

```sql
-- Good
sales
product_data
reporting

-- Bad
SalesSchema -- (PascalCase)
product-data -- (Hyphen instead of underscore)
productdata -- (No separation)
```

### 2. Descriptive Names

- Choose names that clearly and concisely describe the purpose or content of the schema.
- Schema names should reflect a logical grouping of related tables, views, functions, and other database objects.

```sql
-- Good - Clearly indicates the purpose.
hr
finance
inventory
web_analytics

-- Bad - Vague or unclear
sch1
data_group
misc
```

### 3. Avoid Prefixes

- Similar to database names, avoid using redundant prefixes like `sch_` for schema names.

```sql
-- Good
public

-- Bad
sch_public
```

### 4. Project or Application-Specific Names (If Needed)

- If a database contains objects for multiple projects or applications, consider using schemas to group related objects and use project or application-specific schema names.
- This is especially important in:
  - **Multi-Tenant Databases**: Each tenant can have a dedicated schema, such as tenant1_data, and tenant2_reports.
  - **Large Projects**: Modules or domains within a project can be organized into separate schemas, such as `finance_core` and `hr_reports`.

```sql
-- Example: Multiple projects in one database
finance_data
hr_reports
tenant1_billing
tenant2_accounts
```

### 5. Avoid Reserved Words

- Do not use PostgreSQL reserved keywords as schema names.

### 6. The `public` Schema

- The `public` schema is the default schema in PostgreSQL. Use it for objects that are shared across the entire database or don't belong to a specific project or application. For example, use `public` for general-purpose tables like `countries` or `currencies` that are used globally across multiple projects.
- Avoid using the `public` schema as a dumping ground. Organize objects into appropriate schemas even if they are not part of a specific project.

### 7. Special Characters

- Avoid using special characters other than underscores in schema names.

### 8. Length

- Keep schema names reasonably short, but prioritize clarity over brevity.

## Rationale

- **Organization**: Schemas provide a way to organize database objects into logical groups, making the database easier to understand and manage.
- **Namespace Management**: Schemas help to prevent naming conflicts by providing separate namespaces for objects.
- **Security**: Schemas can be used to control access to database objects by granting different permissions to different users or roles on specific schemas.
- **Maintainability**: Well-organized schemas make it easier to maintain and update the database schema.

## Examples

**Good Schema Names**:

- `sales`
- `marketing`
- `hr`
- `finance`
- `reporting`
- `projectx_core`
- `app_data`

**Bad Schema Names**:

- `schema1` (Not descriptive)
- `sch_stuff` (Unnecessary prefix)
- `DataSchema` (Mixed case)
- `public` (Generally, avoid putting everything in public)

## Exceptions

- **Legacy Systems**: Existing schemas that don't follow these conventions might require a gradual migration plan.
- **External Systems**: Schemas that need to integrate with external systems might need to follow different naming conventions.

Document any exceptions to these rules in the [Exceptions](exceptions.md) file.

## Usage Notes

- When creating objects within a schema, you can prefix the object name with the schema name (e.g., `CREATE TABLE <schema_name>.<table_name> ...`).
- You can set the `search_path` to include the schemas you use most frequently, so you don't have to qualify object names with the schema name every time (e.g., `SET search_path TO <schema_name>, public;`).
- Be mindful of the `search_path` when working with schemas, as it determines the order in which schemas are searched for objects.

## Default Schema

- **`public`**: Unless a different default schema is specified during database creation or altered afterward, new objects will be created in the `public` schema by default.

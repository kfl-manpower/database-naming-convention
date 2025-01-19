# Columns

This section outlines the naming conventions for columns within tables in our PostgreSQL databases.

## Naming Rules

### 1. Case

- Use **lowercase letters** for all column names.
- Separate words with underscores (`_`). This follows the `snake_case` convention.

```sql
-- Good
first_name
order_date
product_id

-- Bad
FirstName -- (PascalCase)
orderDate -- (No separation)
product-id -- (Hyphen instead of underscore)
```

### 2. Descriptive Names

- Choose names that clearly and concisely describe the data stored in the column, aligning with the business domain to make the schema more intuitive for stakeholders.
- Column names should be meaningful and self-explanatory.

```sql
-- Good - Clearly indicates the data stored
customer_id
email
total_amount
is_active

-- Bad - Vague or unclear
data1
col2
info
```

### 3. Avoid Abbreviations

- Avoid abbreviations unless they are very well-known and widely understood within the context of your application or industry.
- For a list of accepted abbreviations, see the [Abbreviations](abbreviations.md) file.
- Prioritize clarity over brevity.

```sql
-- Good (Generally)
phone_number
street_address

-- Potentially bad (unless 'ph_num' and 'st_addr' are universally understood in your context)
ph_num
st_addr
```

### 4. Primary Key Columns

- For surrogate primary keys (e.g., auto-incrementing integers), use `id` or `<table_name>_id` (e.g., `customer_id`, `product_id`) as recommended in the [Primary Keys](primary-keys.md) section.

```sql
-- Good
id
customer_id
order_id
```

### 5. Foreign Key Columns

- Name foreign key columns using the format `<referenced_table_name>_id` (e.g., `customer_id`, `order_id`). This clearly indicates the relationship to the referenced table.
- For composite keys, include all relevant referenced columns in the name for clarity (e.g., `customer_region_id` for a foreign key referencing `customer_id` and `region_id`).
- Refer to the [Foreign Keys](foreign-keys.md) section for more details on naming conventions for foreign keys.

```sql
-- Good
customer_id -- (referencing the 'id' column of the 'customers' table)
product_id -- (referencing the 'id' column of the 'products' table)
customer_region_id -- (referencing 'customer_id' and 'region_id')
```

### 6. Boolean Columns

- Use a prefix like `is_`, `has_`, or `can_` to indicate boolean values (true/false).

```sql
-- Good
is_active
is_valid
has_discount
can_edit
```

### 7. Date and Time Columns

- Use descriptive names that clearly indicate the nature of the date or time information. Consider adding a suffix like `_at` or `_on` for clarity.

```sql
-- Good
created_at
updated_at
order_date
start_time
expiration_date
```

### 8. Avoid Data Type Suffixes

- Avoid including data type information in column names (e.g., `name_text`, `amount_decimal`). Data types might change, but good names should be stable.
- If you must include type information for clarity, consider using more general terms like `_code`, `_flag`, `_value` as the suffix.

```sql
-- Generally avoid
customer_name_text
order_total_float

-- Better (if a suffix is truly necessary)
status_code
is_valid_flag
```

### 9. Avoid Reserved Wordsd

- Do not use PostgreSQL reserved keywords as column names.
- Examples of reserved words to avoid include:
  - `user` (Consider using `app_user` or `customer_user` instead.)
  - `order` (Consider using `purchase_order` or `sales_order` instead.)
  - `select` (Consider using `selection` or `query_selection` instead.)

### 10. Special Characters

- Avoid using special characters other than underscores in column names.

### 11. Length

- Keep column names reasonably short, but prioritize clarity over brevity.

## Rationale

- **Readability**: Using `snake_case` and descriptive names make SQL queries and database schemas easier to read and understand.
- **Consistency**: Consistent naming conventions improve the overall consistency of the database.
- **Maintainability**: Well-named columns are easier to maintain and modify.
- **Collaboration**: Clear naming conventions facilitate collaboration among team members.

## Examples

**Good Column Names**:

- `customer_id`
- `first_name`
- `last_name`
- `email`
- `phone_number`
- `order_date`
- `total_amount`
- `is_active`
- `created_at`
- `updated_at`

**Bad Column Names**:

- `custID` (Mixed case, abbreviation)
- `data1` (Vague)
- `order_total_float` (Data type suffix)
- `order-date` (Hyphen instead of underscore)
- `user` (Reserved word)

## Exceptions

- **Legacy Systems**: Existing columns that don't follow these conventions might require a gradual migration plan.
- **External Systems**: Columns that need to integrate with external systems might need to follow different naming conventions.

Document any exceptions to these rules int he [Exceptions](exceptions.md) file.

## Best Practices

- **Define Column Names Clearly**: When creating tables, carefully consider the data type, constraints (e.g., `NOT NULL`, `UNIQUE`), and default values for each column.
- **Document Column Meanings**: Use the `COMMENT` command in SQL to add descriptions to columns, explaining their purpose and any special considerations. This is especially important for columns with names that are not immediately obvious.
- **Example of adding comments**:

  ```sql
  CREATE TABLE employees (
      id SERIAL PRIMARY KEY,
      first_name VARCHAR(255) NOT NULL,
      last_name VARCHAR(255) NOT NULL,
      email VARCHAR(255) UNIQUE
  );

  COMMENT ON COLUMN employees.first_name IS "The employee's first name";
  COMMENT ON COLUMN employees.last_name IS "The employee's last name";
  ```

- **Consider Data Domains (Optional)**: For large databases, consider using data domains to define reusable column types with specific constraints. This can improve consistency and reduce redundancy.

# General Principles of Database Naming Conventions

These general principles underpin our PostgreSQL database naming conventions and should be applied consistently across all database objects. They are designed to promote clarity, maintainability, and collaboration.

## 1. Clarity

- **Descriptive and Unambiguous**: Names should clearly and accurately reflect the purpose, meaning, and content of the database object. Avoid vague or ambiguous terms that could be misinterpreted.
  - Example: Use `order_date` instead of `date`.
- **Self-Documenting**: Strive to make names self-explanatory so that their meaning is readily apparent without needing to refer to external documentation.

## 2. Consistency

- **Uniform Application**: Apply the same naming rules and patterns throughout the entire database and across different projects whenever possible. This consistency is crucial for reducing confusion and errors.
  - When integrating with legacy systems, document and plan for gradual alignment to the standard naming conventions.
- **Standard Case**: Adhere to `snake_case` convention (lowercase with underscores) for all database object names, unless specifically documented as an exception.

## 3. Conciseness

- **Brevity**: Keep names as short as possible without sacrificing clarity. Shorter names are easier to type, read, and remember.
- **Avoid Redundancy**: Do not include redundant prefixes or suffixes unless they add significant value or are necessary to avoid naming conflicts in very large schemas. For example, avoid prefixes like `tbl_` for table names (e.g., use `clients` instead of `tbl_clients`).
- **Meaningful Abbreviations**: Use abbreviations sparingly, and only when they are well-established and widely understood within the context of your database and application (e.g., `id`, `amt`, `qty`).

## 4. Meaningful

- **Reflect Business Concepts**: Names should align with the terminology used in the business domain or application that the database supports. This makes it easier for developers and stakeholders to understand the database schema.
- **Purpose-Driven**: The name should clearly indicate the role and function of the object within the database.

## 5. Lowercase with Underscores (`snake_case`)

- **Standard Convention**: Use lowercase letters for all database object names. Separate words with underscores (`_`).
- **PostgreSQL Case-Insensitivity**: This convention aligns with PostgreSQL's handling of unquoted identifiers, which are folded to lowercase. Using `snake_case` ensures consistency regardless of how identifiers are written in queries.
- **Examples**:
  - `client_id`
  - `first_name`
  - `drivers_license_no`

## 6. Avoid Reserved Words

- **Prevent Conflicts**: Do not use PostgreSQL reserved keywords as names for database objects. This can lead to syntax errors and unexpected behavior.
- **Consult Documentation**: Refer to the official [PostgreSQL documentation](https://www.postgresql.org/docs/current/sql-keywords-appendix.html) for a list of reserved keywords.
- **Workarounds**: If you must use a reserved keyword, modify it slightly (e.g., use a synonym). For example, instead of `order`, use `purchase_order`.

## 7. Pluralization

- **Tables**: Use plural nouns for table names (e.g., `clients`, `employees`, `coordinators`). This reflects that tables typically store collections of entities.
- **Other Objects**: Use singular or plural forms as appropriate for other object types (e.g., functions, views, etc.) based on their specific purpose and what they represent.

## 8. Be Mindful of Data Types

- **Avoid embedding data types in names**: In general, avoid including data types in column names (e.g., `customer_name_text`). Data types might change, but good names should be stable.
- **Use suffix in exceptional cases**: You can consider adding a suffix to indicate data type or purpose if it greatly enhances clarity:
  - Examples:
    - Timestamps: `created_at`, `updated_at`
    - Booleans: `is_active`, `has_access`.

## 9. Versioning and Exceptions

- **Document Changes**: If significant changes are made to these naming conventions, update the version number and maintain a changelog.
- **Justify Exceptions**: Any deviations from these conventions must be clearly documented and justified. See the [Exceptions](exceptions.md) document for details

These general principles serve as the foundation for our specific naming rules. By adhering to them, we can create PostgreSQL databases that are well-structured, understandable, and maintainable. Remember that consistency is key to achieving these goals.

# Database Naming Conventions

## Purpose

This repository contains the standard naming conventions for database objects within PostgreSQL environments. Consistent naming, improves code readability, maintainability, and collaboration among developers. We advocate for the use of `snake_case` as the preferred naming style for most database objects.

## Key Principles

- **Clarity**: Names should be descriptive and unambiguous.
- **Consistency**: Apply the same naming rules throughout the database.
- **Conciseness**: Strive for brevity without sacrificing clarity.
- **Meaningful**: Names should reflect the meaning and role of the object.
- **Lowercase with Underscores (`snake_case`)**: Use lowercase letters for all database object names, separating words with underscores (e.g., `first_name`, `last_name`).
- **Avoid Reserved Words**: Do not use PostgreSQL reserved keywords as object names.

**For a detailed explanation of each section and examples, please refer to the [documentation index](docs/index.md).**

## Versioning

This document follows semantic versioning (MAJOR.MINOR.PATCH).

**Current Version**: 1.0 (2025-01-17)

**Changelog**:
- **v1.0 (2025-01-17):** Initial version of the naming conventions document.

## Exceptions

While these naming conventions should be followed in most cases, there might be rare exceptions due to:

- **Legacy Systems**: Existing databases that don't conform to these conventions might require gradual refactoring rather than immediate renaming.
- **Integration with External Systems**: External systems with different naming requirements might necessitate deviations from these conventions.
- **Specific Project Requirements**: In exceptional cases, a project might have unique needs that justify a different approach.

**All exceptions should be clearly documented and justified.** The goal is to minimize deviations and maintain consistency as much as possible.

For details on the general exception policy and a list of specific exceptions, see the [Exceptions](docs/exceptions.md) document.

## Contribution Guidelines

To contribute:
1. **Report Issues**: Use the [naming-guidelines-update](./.github/ISSUE_TEMPLATE/naming-guidelines-update.md) template to suggest updates or report problems.
2. **Submit Pull Requests**: Follow the [pull request template](./.github/PULL_REQUEST_TEMPLATE.md) to propose changes.
3. **Add Examples**: Contribute to practical examples to the `doc/examples/` folder.
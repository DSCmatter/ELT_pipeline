# ELT Pipeline with Snowflake & dbt

## Overview

This project sets up an ELT pipeline using Snowflake and dbt, following best practices for modular and scalable data transformations.

## Steps to Set Up

### 1. Setup Snowflake Environment

- Create a Snowflake account.
- Configure a new database, schema, and warehouse.
- Generate user credentials and set up access.

#### Snowflake Setup Steps:

- Set up a Snowflake warehouse and assign compute resources.
- Create roles and grant necessary permissions.
- Configure network policies and security settings.
- Like in the image below:
- Make sure to write your username in this line `grant role dbt_roLe to user <username>`

![snowflake-config](https://github.com/user-attachments/assets/f8f22542-7b49-47f3-bc3a-f382d22c3049)

### Setup dbt 
- with `dbt init`
- Configure your dbt-project
 
### 2. Configure `dbt_profile.yaml`

- Configure `profiles.yml` with Snowflake credentials:
  ```yaml
  models:
    snowflake_workshop:
      staging:
        materialized: view
        snowflake_warehouse: dbt_wh
      marts:
        materialized: table
        snowflake_warehouse: dbt_wh
  ```

### 3. Create Source and Staging Files

- Define `sources` in `models/schema.yml` to map raw data.
- Create staging models (`models/staging/`) for data standardization.

### 4. Macros (D.R.Y. Principle)

- Define reusable macros in `macros/`.
- Example macro to handle null values:
  ```sql
  {% macro replace_null(column, value) %}
    coalesce({{ column }}, {{ value }})
  {% endmacro %}
  ```

### 5. Transform Models (Fact Tables & Data Marts)

- Create fact and dimension tables under `models/marts/`.
- Build transformation models using SQL and CTEs.

### 6. Generic and Singular Tests

- Define tests in `tests/` and `schema.yml`.
- Example:
  ```yaml
  tests:
    - unique
    - not_null
  ```
- Run tests: `dbt test`

## Running the Pipeline

```sh
# Initialize dbt project
$ dbt init my_project

# Run transformations
$ dbt run
OR
$ dbr run -s <filename>

# Run tests
$ dbt test
```

* After doing tests & models make sure to check out snowflake or snowflake worksheet to see your models/tables/columns.

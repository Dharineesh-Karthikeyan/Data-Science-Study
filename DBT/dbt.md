# 📦 dbt (Data Build Tool) 
---

## 🚀 What is dbt?

**dbt (data build tool)** is an open-source tool that enables **analytics engineers** to transform data in their warehouse by writing SQL-based **modular, version-controlled, and testable** transformations.

- Works with modern data warehouses: BigQuery, Snowflake, Redshift, Postgres
- Separates **data transformation (T in ELT)** from extraction and loading
- Encourages software engineering best practices: modular SQL, testing, CI/CD, and documentation

##### **dbt helps you write SQL as building blocks, not as monolithic queries.**
**Modular SQL** means breaking down complex SQL transformations into **smaller, reusable models** that each perform a specific task.
- Improves **readability** and **maintainability**
- Encourages **layered architecture** (e.g., staging → intermediate → marts)
- Enables easier **testing** and debugging

---

## 🔧 How dbt Works

- You write SQL queries as **models**.
- dbt compiles and executes them in your data warehouse as tables/views.
- You add **tests**, **documentation**, and **version control** (via Git).
- dbt handles **dependency management**, **order of execution**, and **incremental loading**.

---

## 📁 dbt Project Folder Structure

```
my_project/
│
├── dbt_project.yml          # Project config (name, version, paths)
├── models/                  # Where SQL transformation models live
│   ├── staging/             # Staging layer (raw → cleaned)
│   ├── marts/               # Business-level models
│   └── example_model.sql    # Sample model
├── tests/                   # Custom test SQLs (singular tests)
├── macros/                  # Reusable Jinja macros
├── snapshots/               # Historical snapshots of data
├── seeds/                   # Static CSV files to be loaded as tables
└── target/                  # Compiled output (ignored in Git)
```

## 🔄 Staging vs Marts in dbt

| Feature            | Staging Models                      | Marts Models                            |
|--------------------|--------------------------------------|------------------------------------------|
| Purpose            | Clean and standardize raw data       | Transform data for business use          |
| Logic              | 1:1 mapping to source tables         | Joins, aggregations, calculations        |
| Naming Convention  | `stg_` prefix                        | `dim_`, `fct_`, or `int_` prefixes       |
| Use of `source()`  | Yes                                  | Rare (uses `ref()` on staging models)    |
| Example            | `stg_orders`, `stg_customers`        | `fct_orders`, `dim_customers`            |
| Complexity         | Low (basic transformations)          | High (business logic and KPIs)           |
| Testing Focus      | Column-level tests (e.g. not null)   | Business logic validation                |

> ✅ Staging = clean raw data  
> ✅ Marts = business-ready tables

---

## 🛠️ Setting Up dbt with dbt-core (CLI)

## ⚙️ Setting Up Your dbt Project (with dbt-core)

✅ 1. Install dbt-core (with your adapter)

Install dbt and the appropriate adapter for your data warehouse:

```bash
# Install dbt
pip install dbt-core

# Install adapters for Postgres,Snowflake, BigQuery, Redshift, use:
# pip install dbt-postgres
# pip install dbt-snowflake
# pip install dbt-bigquery
# pip install dbt-redshift
```

✅ 2. Initialize a New Project

```bash
dbt init my_project
```
✅ 3. Set up profiles.yml config file
You will be prompted to:
- Choose a warehouse adapter
- Enter connection details (host, port, schema (OR) oauth, service account etc.)
- This will be the profile in `profiles.yml` for our connection.

✅ 4. Test Your Connection

Run the following command to verify that dbt can connect to your warehouse:

```bash
dbt debug
```

---

## 📄 Key Components

## 🗂️ Common `.yml` Files in dbt Projects

| File Name         | Purpose                                                                 |
|-------------------|--------------------------------------------------------------------------|
| `dbt_project.yml` | Main config: project name, model paths, default materializations       |
| `profiles.yml`    | Stores DB connection settings (in `~/.dbt/`)                           |
| `schema.yml`      | Define model tests, column descriptions, relationships, docs           |
| `packages.yml`    | Lists external dbt packages to install (like dbt_utils)                |

### 📌 Tip:
While `dbt_project.yml` is created automatically inside the project folder, `profiles.yml` lives in the user folder (`~/.dbt/`), and `schema.yml` should be added manually per model folder to maintain modularity and clean testing.

---

### 📌 Models
- SQL files in `/models` and are named as **'xxxx.sql'**
- dbt compiles them and builds them in the warehouse
- Use `{{ ref('model_name') }}` to declare dependencies

```sql
-- models/orders.sql
SELECT * FROM {{ ref('raw_orders') }}
WHERE order_status = 'shipped'
```

Run:
```bash
dbt run --select orders
```

---

### 📌 Materializations

Controls **how** dbt builds your model in the warehouse:

| Materialization | Description                          |
|-----------------|--------------------------------------|
| `view`          | Default. Creates a view              |
| `table`         | Creates a new table every time       |
| `incremental`   | Only adds new data (with logic)      |
| `ephemeral`     | Not persisted; inlined as CTE        |

Default Materializations is **views**. 

**Set materialization** in a model's config block:
```sql
{{ config(materialized='incremental') }}

SELECT * FROM source_table
WHERE updated_at > (SELECT max(updated_at) FROM {{ this }})
```
You can also set the materialization for all model's within a folder by adding it in the **'dbt_project.yml'** file:
```yaml
# dbt_project.yml

models:
  my_project:  # replace with your actual project name
    staging:   # folder name inside /models
      +materialized: view

    marts:
      +materialized: table
```

- This sets all models in the `models/staging/` folder to be materialized as **views**
- All models in the `models/marts/` folder will be materialized as **tables**
- You can override this at the model level using `{{ config(materialized='...') }}` in the SQL file

---

### 📌 Seeds
- Load CSV files into your warehouse
- Place files in the `/seeds` directory
- Run:
```bash
dbt seed
```

---

### 📌 Snapshots
- Track slowly changing dimensions (SCDs)

Each snapshot must:
- Define a **unique key** that identifies each record
- Use a **strategy** (`check` or `timestamp`)
- Optionally define which fields to track changes on

🧠 Strategies Explained

| Strategy   | Description                                                                 |
|------------|-----------------------------------------------------------------------------|
| `check`    | Compares the current values of specified columns with the previous version |
| `timestamp`| Looks for new values based on a last-updated timestamp column              |


#### ✅ `check` Strategy (SCD Type 2 with value change)

Use this when the source table **doesn't have an updated_at field** but you want to track changes in selected columns.

```sql
-- snapshots/customers_snapshot.sql
{% snapshot customers_snapshot %}
{{
    config(
        target_schema='snapshots',
        unique_key='customer_id',
        strategy='check',
        check_cols=['email', 'status']
    )
}}

SELECT * FROM {{ source('raw', 'customers') }}

{% endsnapshot %}
```

> If `email` or `status` changes, a new version is recorded.


#### ✅ `timestamp` Strategy (SCD Type 2 with updated timestamp)

Use this when the source has a reliable **`updated_at`** or `last_modified` column.

```sql
-- snapshots/customers_snapshot.sql
{% snapshot customers_snapshot %}
{{
    config(
        target_schema='snapshots',
        unique_key='customer_id',
        strategy='timestamp',
        updated_at='updated_at'
    )
}}

SELECT * FROM {{ source('raw', 'customers') }}

{% endsnapshot %}
```

> dbt will insert new records whenever `updated_at` is more recent than the previously stored version.


#### ⏳ Snapshot Table Columns

When snapshots run, dbt stores:
- `dbt_valid_from`: timestamp when record became valid
- `dbt_valid_to`: timestamp when record was superseded (null if current)
- All original columns from the source and updated columns (Since Type 2 SCD)


#### ▶️ Running Snapshots

```bash
dbt snapshot
```

#### 🎯 Real-World Use Cases

- Tracking customer status or plan history
- Monitoring product price changes
- Capturing changes in financial or subscription systems

---

---

### 📌 Macros
- Reusable parts of code (functions) that can be called across models
- Includes parameters that can be passed
- The output should be singular value
  
```sql
{% macro is_weekend(day_col) %}
  CASE WHEN extract(dow FROM {{ day_col }}) IN (0,6) THEN true ELSE false END
{% endmacro %}
```

#### 📥 Calling a Macro in a Model

You can call this macro inside a model like this:

```sql
-- models/orders_with_weekend_flag.sql

SELECT
  order_id,
  order_date,
  {{ is_weekend("order_date") }} AS is_weekend
FROM {{ ref('stg_orders') }}
```

> ✅ `{{ is_weekend("order_date") }}` will be replaced with the CASE statement at compile time.

---

## ✅ dbt Testing

### 🧪 Generic Tests (YAML-based, reusable)

Defined in your `.yml` files alongside models:

```yaml
models:
  - name: orders
    columns:
      - name: order_id
        tests:
          - not_null
          - unique
```

Built-in tests include:
- `not_null`
- `unique`
- `accepted_values`
- `relationships`

Run tests:
```bash
dbt test
```

---

### 🧪 Singular Tests (Custom SQL logic)

Custom test logic placed in `tests/` folder:
```sql
-- tests/test_orders_positive_amount.sql
SELECT * FROM {{ ref('orders') }}
WHERE amount <= 0
```

> Test fails if query returns any rows

---

### ✅ Generic vs Singular Test: Comparison

| Feature          | Generic Test                     | Singular Test                       |
|------------------|----------------------------------|-------------------------------------|
| Reusability       | High (parameterized)             | Custom to specific use case         |
| Location          | YAML file                        | SQL file in `tests/`                |
| Use Case          | Column-level constraints         | Complex row-level business logic    |

---

## Documentation and Data Lineage/DAG

Documentation helps better understand the dependencies and flow of data within the system and helps with data governance and security.

### 🔹 `ref()` Function

The `ref()` function is used to **reference other models** in dbt. It:
- Automatically defines **dependencies** between models
- Ensures models run in the correct **order**
- Helps dbt build the **DAG** (Directed Acyclic Graph)

```sql
SELECT * FROM {{ ref('stg_orders') }}
```

> Instead of hardcoding schema.table names, `ref()` keeps it modular and portable.

---

### 🔹 DAG (Directed Acyclic Graph)

dbt automatically builds a **DAG** based on how models reference each other using `ref()`.

- The DAG defines **the execution order** of models
- You can visualize it using:
```bash
dbt docs generate && dbt docs serve
```

> DAG ensures upstream models are run before downstream ones.

---

### 🔹 Lineage

**Lineage** refers to the **flow of data** from raw sources through transformations to final outputs.

- dbt visualizes lineage via model dependencies
- Helps you trace:
  - Where a field originates from
  - What downstream reports or models rely on it
- Crucial for debugging and impact analysis

---

## 📚 Interview Questions & Answers

### ❓ Q1: What is dbt and why is it useful?
**Answer**: dbt is a transformation framework that brings modularity, testing, and version control to SQL workflows. It makes analytics engineering reproducible and scalable.

---

### ❓ Q2: What is materialization in dbt?
**Answer**: It defines how the SQL model is built in the data warehouse — as a `view`, `table`, `incremental` model, or non-persistent `ephemeral`.

---

### ❓ Q3: What's the difference between generic and singular tests?
**Answer**: Generic tests are reusable YAML-defined constraints. Singular tests are custom SQL queries for more complex logic.

---

### ❓ Q4: How does dbt handle dependencies?
**Answer**: Via the `ref()` function, which defines the model DAG and ensures correct execution order.

---

### ❓ Q5: What is the use of `dbt run`, `dbt seed`, and `dbt test`?
**Answer**:
- `dbt run`: Compiles and executes models
- `dbt seed`: Loads CSV files into tables
- `dbt test`: Executes defined tests on models

---

### ❓ Q6: What are ephemeral models and when do you use them?
**Answer**: Ephemeral models are not materialized in the warehouse. They're used as CTEs for intermediate transformations in larger queries.

---

## 🧠 Tips for Interviews

- Be prepared to draw or explain a dbt DAG (model dependency graph) or data lineage
- Use terms like modular SQL, ref(), materializations, DAG, lineage
- Show understanding of version control and CI/CD in analytics
- Talk about combining dbt with tools like **Airflow**, **GitHub Actions**, or **Dataform**

---

## 🔗 Additional Resources

- [Youtube Link](https://www.youtube.com/watch?v=C6BNAfaeqXY)

---

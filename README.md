# SQL Educational Repository — From Local Experiments to Production

This repository is an end‑to‑end training resource for learning how to use SQL across
different environments. It begins with a small SQLite database so you can
practice core query mechanics locally, then introduces PostgreSQL for
production‑grade workloads, and concludes with cloud‑warehouse examples using
BigQuery and Snowflake.

Rather than focusing solely on syntax, the exercises and explanations here
emphasize **why** you use each tool and the trade‑offs involved. The goal is to
prepare you to write correct SQL, choose the right database for the job, and
understand the operational considerations that come with moving from a notebook
on your laptop to a production pipeline.

For a Machine Learning Engineer, SQL is not just about pulling data — it’s about building robust,
efficient queries for feature engineering, data validation, and reporting,
so keep that in mind as you work in this repository.

## Contents

* `data/sql_practice.db` — A SQLite database with four tables: contractors,
jobs, photos, and events. The schema has been seeded with realistic
timestamps and activities so you can practice joins, window functions,
aggregations, cohort analyses, and feature engineering.

* `src/sql_practice_course.ipynb` — A Jupyter notebook that walks through three
progressive levels of SQL practice:

    1. **Level 1: Local databases**. Exercises using SQLite (and optionally
    DuckDB) that cover the basics: selecting, filtering, sorting, grouping,
    and joining tables.

    2. **Level 2: PostgreSQL.** Exercises that mimic working against a
    remote, networked database. Examples demonstrate how to connect with
    SQLAlchemy and run queries against a PostgreSQL instance. Exercises
    explore joins and anti‑joins in a production context.

    3. **Level 3: Cloud warehouses.** High‑level examples for connecting to
    BigQuery and Snowflake and questions that encourage you to think about
    analytics at scale. You’ll practice writing queries that aggregate data
    across large tables and implement simple feature‑engineering logic.

* **practice_questions.sql** — The original list of 35+ questions grouped by
difficulty. You can adapt these to any database by adjusting the syntax.

* **hints.json** — Lightweight hints for many of the questions. The hints are
intentionally vague to encourage problem solving.

## How to Use This Repo

1. **Start in the notebook.** Open `sql_practice_course.ipynb` in Jupyter
(or any compatible environment) and run the cells. The first section
introduces a helper function called `run_sql()` that connects to the
bundled SQLite database.

2. **Write your queries.** Each exercise includes a code cell with a
comment that says `# TODO.` Replace the comment with your SQL query and
run the cell to see the results. If you get stuck, check the hint in the
preceding markdown cell or consult the `hints.json` file.

3. **Experiment with DuckDB.** For analytical tasks or working with
columnar formats like Parquet, DuckDB can be an excellent local engine.
The notebook shows how to connect in‑memory or via a file. Try reading
the data into DuckDB and writing queries against it.

4. **Graduate to PostgreSQL.** When you’re comfortable with local queries,
try running similar queries on a PostgreSQL database. You’ll need to
provision a database (for example on a cloud service or locally via
Docker), create the same schema, and load the data. The notebook
includes example code using SQLAlchemy to manage connections.

5. **Explore BigQuery and Snowflake.** The final part of the notebook
includes code snippets that show how to authenticate and run queries on
Google BigQuery and Snowflake using their Python clients. You will need
credentials and project/warehouse information to run these examples.

### Running the Exercises

Each level builds on the previous one but you can dip into any section as
needed. The exercises are intentionally open‑ended: you may devise
alternative query strategies using subqueries, common table expressions,
window functions, or different join types. Try to implement each query
in more than one way and compare their performance.

### Beyond Syntax: Operational Tips

Moving SQL into production involves more than getting the query right.
Here are a few operational considerations you will encounter as you scale
up:

* **Secrets and configuration.** Never hard‑code passwords or API keys in
your code. Use environment variables or a secret manager. Client
libraries for PostgreSQL, BigQuery, and Snowflake all support loading
credentials this way.

* **Connection pooling.** In web services or batch jobs that run many
queries, use a connection pool to avoid opening and closing connections
repeatedly. SQLAlchemy includes pooling by default.

* **Parameterized queries.** Pass parameters to your queries rather than
concatenating strings. This prevents SQL injection and helps the
database optimizer reuse query plans.

* **Chunking and streaming.** When reading large tables into Python, use
chunked reads (e.g. chunksize in pandas) or streaming cursors (e.g.
server‑side cursors in psycopg) to avoid running out of memory.

* **Indexing and query planning.** Learn how to read your database’s
execution plans (e.g. EXPLAIN in PostgreSQL) and create indexes on
columns that are frequently filtered or joined. Even in cloud
warehouses, you can structure your tables (partitions, clustering) to
reduce costs and improve performance.

* **Testing and migrations.** Use migration tools (such as Alembic for
PostgreSQL) to manage schema changes and test frameworks (such as dbt
tests) to validate assumptions about your data.

## Extending the Course

The notebook and dataset provided here are a starting point. To deepen
your understanding:

* **Write additional questions.** Think of new business questions you might
answer with this data—e.g. cohort retention analysis, photo upload
latency by job type, or contractor productivity metrics.

* **Measure performance.** Time your queries and try to improve them.
Compare how the same query performs in SQLite, DuckDB, PostgreSQL,
BigQuery, and Snowflake. Note how indexing, partitions, and cluster
keys change performance.

* **Integrate with Python.** Use the output of your queries as features
in a machine learning workflow or plug them into a dashboarding tool.
By the end of this course you should feel comfortable switching between
local experimentation and production systems, adapting your SQL to the
capabilities of each environment, and considering the operational
implications of your data pipelines.

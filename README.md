# End-to-End ETL Data Pipeline

A Python data engineering project that extracts public data from the JSONPlaceholder API, validates and transforms it, and loads it into a PostgreSQL database.

The pipeline demonstrates an **Extract → Transform → Load (ETL)** workflow with logging, environment-based configuration, database upserts, and automated tests.

## Architecture

```text
JSONPlaceholder API
        |
        v
   Extract (Python requests)
        |
        v
 Transform + Validate (Python)
        |
        v
 PostgreSQL (upsert records)
        |
        v
 Query and analyze with SQL
```

## Features

- Retrieves posts from a public REST API.
- Validates required fields and normalizes text.
- Rejects invalid records and reports how many were skipped.
- Loads records into PostgreSQL using conflict-safe upserts.
- Creates the destination table automatically.
- Uses environment variables for database configuration.
- Logs pipeline progress and errors.
- Includes unit tests for transformation logic.

## Tech stack

- Python 3.10+
- REST APIs and JSON
- PostgreSQL and SQL
- Requests, python-dotenv, pytest

## Project structure

```text
etl-data-pipeline/
├── etl/
│   ├── __init__.py
│   ├── extract.py
│   ├── transform.py
│   ├── load.py
│   └── pipeline.py
├── sql/
│   └── schema.sql
├── tests/
│   └── test_transform.py
├── .env.example
├── .gitignore
├── requirements.txt
├── docker-compose.yml
└── README.md
```

## Run locally

### 1. Start PostgreSQL

Install Docker Desktop, then run:

```bash
docker compose up -d
```

This starts PostgreSQL on port 5432.

### 2. Create and activate a virtual environment

Windows PowerShell:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

macOS/Linux:

```bash
python3 -m venv .venv
source .venv/bin/activate
```

### 3. Install dependencies and configure

```bash
pip install -r requirements.txt
```

Copy `.env.example` to `.env`. The default values match the included Docker Compose configuration. Do not commit your real `.env` file or passwords.

### 4. Run the pipeline

From the project root:

```bash
python -m etl.pipeline
```

### 5. Run tests

```bash
pytest -q
```

## Verify the loaded data

Connect to PostgreSQL and run:

```sql
SELECT COUNT(*) FROM posts;

SELECT user_id, COUNT(*) AS post_count
FROM posts
GROUP BY user_id
ORDER BY post_count DESC;

SELECT id, title
FROM posts
ORDER BY id
LIMIT 10;
```

## Data source

The sample pipeline uses the free JSONPlaceholder API: https://jsonplaceholder.typicode.com/posts

It provides sample post records for development and learning, not real customer or business data.

## Possible extensions

- Schedule daily runs with Apache Airflow.
- Add data-quality metrics and a rejected-records table.
- Store pipeline run history and execution duration.
- Add a dashboard using Power BI or Streamlit.
- Replace the sample API with a real open-data source.
- Containerize the Python application alongside PostgreSQL.

## Learning outcomes

This project demonstrates API ingestion, data validation, transformation, relational database loading, SQL querying, configuration management, logging, and automated testing.

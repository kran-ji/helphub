# HelpHub

## 1. The Idea

### Pitch

HelpHub is a mini data engineering project that matches volunteers with suitable community events. It ingests volunteer event postings and user preference surveys, stores the data in a lightweight local lakehouse, transforms and validates it, then uses semantic search to recommend the best-fit opportunities.

The project demonstrates a modern data pipeline in a hackathon-friendly form: batch ingestion, Parquet storage, SQL transformations, data quality checks, vector indexing, and a simple app for personalized recommendations.

### Core Features

- Volunteer preference survey with interests, skills, location, availability, and commitment level.
- Event catalog ingestion from CSV, JSON, or manually added records.
- Local data lake using raw and cleaned Parquet files.
- dbt transformation models for cleaning, standardizing, and validating event data.
- Vector search over event descriptions and metadata.
- Top 3 personalized volunteer recommendations.
- Short AI-generated explanation for why each event matches the user.
- Simple pipeline dashboard showing records ingested, cleaned, embedded, and recommended.
- Recommendation logging for future analysis and evaluation.

## 2. The Tech Stack

```text
Streamlit -> Python Pipeline -> DuckDB + Parquet -> dbt -> ChromaDB -> Embeddings/LLM
```

### Streamlit

Streamlit provides the web interface. Volunteers fill in their preferences, organizations can add or upload event data, and the app displays recommended opportunities.

### Python Pipeline

Python scripts handle ingestion and pipeline glue. They load event data, write raw files, trigger transformations, create embeddings, and refresh the vector index.

### Parquet

Parquet files act as the local data lake. Raw, cleaned, and analytics-ready datasets can be stored efficiently without needing cloud infrastructure.

### DuckDB

DuckDB is the local SQL engine. It can query Parquet files directly and works well as a lightweight warehouse for a 1-day data engineering project.

### dbt

dbt handles the transformation layer. It creates staging and mart tables, standardizes messy data, and runs quality checks such as non-null IDs, valid dates, and accepted categories.

### ChromaDB

ChromaDB stores vector embeddings of cleaned volunteer events. When a user submits preferences, the app searches ChromaDB for semantically similar events.

### Embeddings Model

An embeddings model converts event descriptions and user preferences into vectors. OpenAI can be used for quality, while SentenceTransformers or Ollama can be used as a local fallback.

### Optional LLM

An LLM can generate short explanations for matches, such as why an education event fits a user who prefers youth mentoring and weekend volunteering.

## 3. Roadmap Plan

### Phase 1: Project Setup

- Create the GitHub repository.
- Set up the Python environment.
- Install Streamlit, DuckDB, dbt-duckdb, ChromaDB, and the embedding provider.
- Create the project folders:

```text
data/
  raw/
  bronze/
  silver/
  gold/
pipeline/
dbt/
app/
```

### Phase 2: Seed Data

- Create 50-200 realistic volunteer event records.
- Include fields such as title, organization, description, category, location, date, skills required, commitment level, and remote/in-person mode.
- Create a few sample user preference records for testing.

### Phase 3: Ingestion

- Build a Python ingestion script.
- Load raw event data from CSV or JSON.
- Write raw records into Parquet files.
- Track basic ingestion stats such as total rows and duplicate event IDs.

### Phase 4: Transformation

- Configure dbt with DuckDB.
- Build staging models for events and user preferences.
- Standardize categories, locations, date fields, and text fields.
- Add dbt tests for required columns and accepted values.
- Produce a cleaned events table for recommendation.

### Phase 5: Vector Index

- Combine event title, description, category, skills, and location into a searchable text field.
- Generate embeddings for cleaned event records.
- Store vectors and metadata in ChromaDB.
- Add a refresh script so the index can be rebuilt after new data arrives.

### Phase 6: Recommendation App

- Build the Streamlit survey form.
- Convert user preferences into a search query.
- Generate the user preference embedding.
- Query ChromaDB for the top matching events.
- Display the top 3 matches with title, organization, date, location, match score, and explanation.

### Phase 7: Polish and Demo

- Add a pipeline status panel.
- Log recommendation requests into DuckDB or Parquet.
- Prepare a short demo flow:
  1. Show raw event data.
  2. Run the pipeline.
  3. Show cleaned records and validation.
  4. Submit a volunteer survey.
  5. Display personalized recommendations.

### Stretch Goals

- Add Prefect orchestration for a visible pipeline DAG.
- Add CSV upload for new organization events.
- Add feedback buttons such as "good match" and "not relevant."
- Compare vector search results with simple keyword matching.
- Add a small analytics page for popular causes and recommendation trends.

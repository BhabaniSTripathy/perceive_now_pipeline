##Overview

This prototype demonstrates an **ingestion + traceability pipeline** using a **public dataset (~30,000 rows)**.  
It shows how raw data is ingested, fingerprinted for traceability, embedded for semantic search, and served with an API.

##Pipeline Steps

**Ingestion:**  
- Download a public dataset (CSV or TSV).  
- Store raw and cleaned data in **DuckDB**.  
- Add a `source_fingerprint` column (SHA256 for each row).

**Embedding:**  
- Use the HuggingFace **`all-MiniLM-L6-v2`** model to embed text data (titles or fields).  
- Store vectors in a **FAISS** index for fast semantic search.

**Serving:**  
- Expose a **FastAPI** route `POST /search`:
  - Accepts a text query.
  - Returns top 3 matching results.
  - Includes metadata: source file, timestamp, trust score.


##Schema Inference

- Uses **pandas** to infer schema from the data.  
- Cleans rows with missing critical fields and removes duplicates.

For production:
- Orchestrate with **Airflow** for scheduling.
- Manage cloud infra with **Terraform** (S3, Snowflake).
- Store raw data in **S3**, query with **Snowflake** or **DuckDB**.
- Use **Weaviate** or **Pinecone** for scalable vector DB.
- Containerize with **Docker**; deploy **FastAPI** behind an API Gateway.

##Run Locally

```bash
pip install -r requirements.txt
python pipeline.py

<h1 align="center"><strong>AWS Bedrock RAG Application with Streamlit + FastAPI</strong></h1>

## Overview

Welcome to the **AWS-Bedrock-Streamlit-RAG-Application** repository. This project provides a Streamlit UI and a FastAPI backend for an agentic Retrieval-Augmented Generation (RAG) workflow powered by Amazon Bedrock Knowledge Bases. Users can upload files to S3, query the knowledge base, and receive grounded answers in a chat interface.

## Task Description

The objective of this project is to build an end-to-end RAG experience that combines:

- A Streamlit UI for chat and file uploads.
- A FastAPI backend that retrieves relevant chunks from Amazon Bedrock Knowledge Bases and generates answers.
- AWS S3 for file storage and an optional chat endpoint using Google Gemini for direct Q&A.

### Requirements

- Provide a chat UI with history and upload capabilities.
- Retrieve grounded answers from an Amazon Bedrock Knowledge Base.
- Expose API endpoints for chat and retrieval.
- Run locally or via Docker Compose with minimal setup.

## Repository Structure

This repository contains the following key files and folders:

1. **frontend/app.py**: Streamlit entrypoint that renders the chat UI, sidebar uploads, and metrics.
2. **frontend/components/**: UI components for chat, sidebar, and metrics.
3. **frontend/utils/**: Session state helpers and an AWS client for S3 uploads and connection checks.
4. **backend/main.py**: FastAPI app exposing `/chat` and `/retrieve_chunks` endpoints with request/response logging.
5. **backend/bedrock.py**: Retrieval logic using Amazon Bedrock Knowledge Bases and a Bedrock-hosted LLM.
6. **backend/requests.py** and **backend/responses.py**: Pydantic request/response models.
7. **backend/logger.py**: Rich logger configuration with console and file logging.
8. **Dockerfile.backend** and **Dockerfile.frontend**: Container images for backend and frontend.
9. **docker-compose.yml**: Orchestration for running both services locally.
10. **pyproject.toml** and **uv.lock**: Python dependencies and lockfile (managed by `uv`).

## Installation and Setup

1. **Clone the Repository**

   ```bash
   git clone https://github.com/mehmetalpayy/AWS-Bedrock-Streamlit-RAG-Application.git
   cd AWS-Bedrock-Streamlit-RAG-Application
   ```

2. **Configure Environment Variables**

   Create a `.env` file with the following values:

   ```bash
   AWS_ACCESS_KEY_ID=your_key
   AWS_SECRET_ACCESS_KEY=your_secret
   AWS_DEFAULT_REGION=eu-central-1
   KNOWLEDGE_BASE_ID=your_kb_id
   S3_BUCKET=your_bucket
   ```

   Optional:

   ```bash
   DATA_SOURCE_ID=your_data_source_id
   BACKEND_URL=http://localhost:8000
   ```

3. **Run with Docker Compose**

   ```bash
   docker compose up --build
   ```

   Frontend: `http://localhost:8501`

   Backend: `http://localhost:8000`

4. **Run Locally (Without Docker)**

   ```bash
   uv venv .venv
   uv sync
   source .venv/bin/activate
   uvicorn backend.main:app --reload --host 0.0.0.0 --port 8000
   streamlit run frontend/app.py --server.address 0.0.0.0 --server.port 8501
   ```

## API Usage

- **POST `/retrieve_chunks`**

  ```bash
  curl -X POST http://localhost:8000/retrieve_chunks \
    -H "Content-Type: application/json" \
    -d '{"query":"What does the knowledge base say about X?"}'
  ```

- **POST `/chat`**

  ```bash
  curl -X POST http://localhost:8000/chat \
    -H "Content-Type: application/json" \
    -d '{"user_input":"Summarize the project goals."}'
  ```

## Design and Engineering Decisions

- **RAG on Bedrock**: Retrieval uses `AmazonKnowledgeBasesRetriever` with a Bedrock-hosted LLM for answer generation.
- **Dual Endpoints**: `/retrieve_chunks` focuses on KB-grounded answers while `/chat` provides direct LLM responses.
- **Streamlit UI**: Simple, fast iteration for chat + file uploads with session state tracking.
- **S3 Uploads**: Uploaded files are stored in S3 and can be ingested into the knowledge base externally.
- **Observability**: Rich logging provides readable console output and file logs under `logs/`.

## Limitations and Future Work

- **Ingestion Pipeline**: Uploading files to S3 does not automatically trigger KB ingestion.
- **Error Handling**: Frontend displays errors but could add retry logic and more detailed status.
- **Security**: Environment variables are required; consider IAM roles or secrets management in production.
- **Evaluation**: Add evaluation scripts and retrieval quality metrics for production readiness.

## Contributing

If you would like to contribute to this project:

1. Fork the repository.
2. Create a new branch for your changes.
3. Make your modifications and commit them.
4. Submit a pull request with a clear description of the changes.

## Contact

For any questions or feedback, please contact me at [mehmetcompeng@gmail.com](mailto:mehmetcompeng@gmail.com).

---

Thank you for visiting the AWS Bedrock Streamlit RAG Application repository. I hope you find it useful!

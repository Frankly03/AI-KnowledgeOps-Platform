# AI KnowledgeOps Platform - A RAG-based System

**Enterprise-Grade Document Intelligence (RAG + Agents + MLOps)**

Turn scattered knowledge (PDFs, docs, chats, web pages) into an intelligent, queryable knowledge base with retrieval-augmented generation (RAG), agent workflows, and production-ready deployment.

---

## 🚀 Vision
Most organizations lose time and money searching for information buried in documents or messages.  
The AI KnowledgeOps Platform reduces time-to-answer and knowledge loss by converting raw content into an actionable knowledge system.

---

## 🚀 Phase 1: The Core RAG API (MVP)

This initial phase focuses on creating a rock-solid, containerized RAG pipeline that can be built upon.

**Core Capabilities of this MVP:**
-   **Ingestion API:** Manually upload PDF or TXT files.
-   **Document Processing:** Robust text extraction, chunking, and embedding.
-   **Vector Storage:** Uses ChromaDB running in a separate container for persistence.
-   **Query API:** Takes a user question and returns a well-formed answer with citations from the source documents.
-   **Containerized:** The entire application is orchestrated with Docker Compose for easy setup and deployment.

---

## 🏗️ Architecture (High-Level)

![Architecture Diagram](docs/architecture.png)

1. **Ingestion** → Upload/connector → Pre-process (chunking)  
2. **Embedding** → Generate vector embeddings  
3. **Storage** → Persist in vector DB (ChromaDB)  
4. **Retrieval** → Query + retrieve relevant chunks  
5. **LLM** → Generate answer with citations  
6. **API** → FastAPI endpoints for integration  
7. **Frontend (later)** → Web app interface  

---

## 🛠️ Tech Stack

-   **Backend Framework:** Python, FastAPI
-   **LLM/RAG Orchestration:** LangChain
-   **Vector Database:** ChromaDB
-   **Embedding Model:** `all-MiniLM-L6-v2` (local, from Sentence Transformers)
-   **LLM for Generation:** OpenAI `gpt-3.5-turbo` (configurable)
-   **Containerization/Deployment:** Docker & Docker Compose
-   **Frontend (later)**: React + Tailwind  
-   **MLOps (later)**: GitHub Actions, Prometheus/Grafana  

---

## 📋 Prerequisites

-   [Docker](https://www.docker.com/get-started) and Docker Compose installed.
-   An OpenAI API Key.

---

## ⚙️ Setup & Running the Project

1.  **Clone the repository:**
    ```bash
    git clone <your-repo-url>
    cd <your-repo-name>
    ```

2.  **Configure your environment:**
    -   Copy the example environment file:
        ```bash
        cp .env.example .env
        ```
    -   Edit the `.env` file and add your OpenAI API key:
        ```
        OPENAI_API_KEY="sk-..."
        ```

3.  **Build and run the containers:**
    ```bash
    docker-compose up --build
    ```
    This command will build the FastAPI application container and pull the ChromaDB image. It will start both services. The API will be available at `http://localhost:8000`.

---

## 📡 API Endpoints

You can access the interactive API documentation (provided by Swagger UI) at `http://localhost:8000/docs`.

### 1. Ingest a Document

-   **Endpoint:** `POST /api/v1/ingest`
-   **Description:** Upload a `.txt` or `.pdf` file to be processed, chunked, embedded, and stored in the vector database.
-   **Request:** `multipart/form-data` with a file attached.
-   **Response:**
    ```json
    {
      "success": true,
      "message": "File 'your_document.pdf' processed and indexed successfully."
    }
    ```

### 2. Query the Knowledge Base

-   **Endpoint:** `POST /api/v1/query`
-   **Description:** Ask a question against the knowledge base.
-   **Request Body:**
    ```json
    {
      "query": "What are the top 3 recurring bugs?"
    }
    ```
-   **Response:**
    ```json
    {
      "answer": "The top three recurring bugs are related to authentication failures, data synchronization issues, and UI rendering problems on mobile devices.",
      "sources": [
        "Source: your_document.pdf, Page: 4",
        "Source: your_document.pdf, Page: 7"
      ]
    }
    ```

---

## 📂 Project Structure

```
.
├── .env.example
├── .gitignore
├── Dockerfile
├── README.md
├── docker-compose.yml
├── requirements.txt
└── src/
    ├── api/
    │   ├── __init__.py
    │   ├── main.py
    │   └── routers/
    │       ├── __init__.py
    │       ├── ingest.py
    │       └── query.py
    ├── core/
    │   ├── __init__.py
    │   ├── chunking.py
    │   ├── embedding.py
    │   ├── generation.py
    │   ├── pipeline.py
    │   └── retrieval.py
    ├── db/
    │   ├── __init__.py
    │   └── vector_store.py
    ├── schemas/
    │   ├── __init__.py
    │   └── models.py
    └── utils/
        ├── __init__.py
        └── config.py
```
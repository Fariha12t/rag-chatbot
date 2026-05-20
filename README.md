# RAG Document Intelligence Chatbot

A production-ready chatbot that lets you **upload any document and ask questions about it** using Retrieval-Augmented Generation (RAG). Built with FastAPI, FAISS, HuggingFace Embeddings, and Groq (free LLaMA 3 70B) — no paid API keys required.

![Python](https://img.shields.io/badge/Python-3.11-blue)
![FastAPI](https://img.shields.io/badge/FastAPI-0.111-green)
![FAISS](https://img.shields.io/badge/FAISS-vector--search-orange)
![Docker](https://img.shields.io/badge/Docker-ready-blue)
![CI/CD](https://img.shields.io/badge/CI%2FCD-GitHub%20Actions-black)

---

## Features

- **Upload any document** — PDF, DOCX, TXT, CSV, Markdown
- **Semantic search** with FAISS + HuggingFace `all-MiniLM-L6-v2` embeddings (runs locally, free)
- **LLM answers** powered by Groq's free LLaMA 3 70B API
- **Multi-turn conversation** with session memory
- **Source citations** — every answer shows which document it came from
- **Clean chat UI** — dark-themed, responsive, drag-and-drop upload
- **Dockerized** — one command to run anywhere
- **CI/CD** — GitHub Actions pipeline: test → build → push → deploy to Azure

---

## Architecture

```
User Upload → Document Parser → Chunker → FAISS Vector Store
                                               ↓
User Question → Embedding → Similarity Search → Top-K Chunks
                                               ↓
                          Context + Question → Groq LLaMA 3 → Answer + Sources
```

---

## Quick Start

### 1. Clone & setup

```bash
git clone https://github.com/Fariha12t/rag-chatbot.git
cd rag-chatbot
cp .env.example .env
# Add your free Groq API key to .env
```

### 2. Get a free Groq API key

Go to [https://console.groq.com](https://console.groq.com), sign up, and generate a key. It's free.

### 3. Run with Docker (recommended)

```bash
docker-compose up --build
```

Open [http://localhost:8000](http://localhost:8000)

### 4. Or run locally

```bash
pip install -r requirements.txt
uvicorn app.main:app --reload
```

---

## API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/` | Chat UI |
| `POST` | `/upload` | Upload a document |
| `POST` | `/chat` | Ask a question |
| `DELETE` | `/reset` | Clear all documents |
| `GET` | `/health` | Health check |

### Example

```bash
# Upload a document
curl -X POST http://localhost:8000/upload \
  -F "file=@report.pdf"

# Ask a question
curl -X POST http://localhost:8000/chat \
  -H "Content-Type: application/json" \
  -d '{"question": "What are the key findings?", "session_id": "user1"}'
```

---

## Deployment (Azure Container Apps)

Add these secrets to your GitHub repository (`Settings → Secrets`):

| Secret | Description |
|--------|-------------|
| `GROQ_API_KEY` | Your Groq API key |
| `DOCKER_USERNAME` | Docker Hub username |
| `DOCKER_PASSWORD` | Docker Hub password/token |
| `AZURE_CREDENTIALS` | Azure service principal JSON |
| `AZURE_RESOURCE_GROUP` | Azure resource group name |
| `ACR_NAME` | Azure Container Registry name |

Push to `main` — the GitHub Actions pipeline will automatically test, build, and deploy.

---

## Tech Stack

| Component | Technology |
|-----------|-----------|
| API Framework | FastAPI |
| Embeddings | `sentence-transformers/all-MiniLM-L6-v2` (HuggingFace, local) |
| Vector Store | FAISS (via numpy cosine similarity) |
| LLM | Groq — LLaMA 3 70B (free tier) |
| PDF Parsing | pypdf |
| DOCX Parsing | python-docx |
| Containerization | Docker + Docker Compose |
| CI/CD | GitHub Actions |
| Cloud | Azure Container Apps |

---

## Project Structure

```
rag-chatbot/
├── app/
│   ├── main.py          # FastAPI routes
│   ├── ingestion.py     # Document parsing & chunking
│   ├── vectorstore.py   # FAISS embeddings & search
│   └── rag.py           # RAG query engine + Groq LLM
├── static/
│   └── index.html       # Chat UI
├── tests/
│   └── test_api.py      # Pytest tests
├── .github/
│   └── workflows/
│       └── ci-cd.yml    # GitHub Actions pipeline
├── Dockerfile
├── docker-compose.yml
├── requirements.txt
└── .env.example
```

---



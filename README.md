# Document Intelligence & RAG API

A learning project that lets users upload text-based PDF documents and ask questions about them. It uses local Ollama models for embeddings and answer generation, and persistent ChromaDB storage for document passages.

> **Status:** Educational prototype. Run and test the project before describing features as working.

## Features

- Upload PDF files through a FastAPI endpoint.
- Extract text while preserving filename and page number.
- Split extracted text into overlapping chunks.
- Create embeddings locally with Ollama.
- Persist chunks and embeddings in ChromaDB.
- Retrieve relevant passages for a question.
- Generate answers from retrieved document passages.
- Return source filenames, page numbers, and excerpts.
- Report when no relevant passage is found.
- Requires no OpenAI API key or paid API call.

## Requirements

- Python 3.10 or newer.
- [Ollama](https://ollama.com/) installed and running.
- Enough memory and disk space for the selected local models.

## Install local models

Start Ollama, then download the models:

```bash
ollama pull llama3.2
ollama pull nomic-embed-text
```

The chat model generates answers. The embedding model converts document passages and questions into vectors for retrieval.

## Install the project

Clone the repository:

```bash
git clone https://github.com/YOUR-USERNAME/document-intelligence-rag.git
cd document-intelligence-rag
```

Create and activate a Python virtual environment:

```bash
python -m venv .venv
```

Linux or macOS:

```bash
source .venv/bin/activate
```

Windows PowerShell:

```powershell
.venv\Scripts\Activate.ps1
```

Install dependencies:

```bash
pip install -r requirements.txt
```

## Run the API

Make sure Ollama is running, then start the API:

```bash
uvicorn app.main:app --reload
```

Open the interactive API documentation:

```text
http://127.0.0.1:8000/docs
```

## Use the API

### Check service status

```http
GET /health
```

### Upload a PDF

Use `POST /documents` in `/docs` and upload a text-based PDF. The API extracts text, creates embeddings, and stores passages in ChromaDB.

Scanned PDFs may not contain extractable text and are not supported unless OCR is added.

### Ask a question

Use `POST /questions` with:

```json
{
  "question": "What does the document say about maintenance?"
}
```

The response includes an answer and retrieved source excerpts.

## Persistent storage

ChromaDB stores its local index in `chroma_db/`. The index persists across API restarts as long as the folder remains. This folder is local data and is excluded from Git with `.gitignore`.

## Configuration

Optional settings can be provided as environment variables:

```text
CHROMA_DB_PATH=chroma_db
OLLAMA_URL=http://localhost:11434
OLLAMA_CHAT_MODEL=llama3.2
OLLAMA_EMBED_MODEL=nomic-embed-text
```

See `.env.example` for placeholder configuration. Do not commit secrets or private documents.

## Limitations

- Local models need suitable computer resources and can be slower than hosted services.
- The index is local and is not configured for multiple users.
- There is no authentication or access control.
- Scanned PDFs require OCR, which is not included.
- Retrieval settings are basic and have not been benchmarked.
- Model answers may be incorrect; verify them using the cited excerpts.
- This is a learning prototype, not a production document-management system.

## Privacy and safety

Documents and prompts are processed by the local Ollama service configured by the user. Use documents you are allowed to process. Do not upload confidential documents to a public repository. Review your local Ollama setup and network exposure before using sensitive files.

## Tests

Automated tests are planned for PDF extraction, chunking, persistent indexing, retrieval, citations, and API error handling. Add exact test instructions after implementing and running the test suite.

## Inspiration and attribution

This is an independent learning project. Credit and comply with licenses for any third-party code, documentation, data, or assets used.

## Resume description

After implementing and testing the project, describe only what you personally verified. Example:

> Built a FastAPI document-Q&A prototype using local Ollama models, PDF text extraction, persistent ChromaDB storage, and source-referenced answers.

Only list technologies and results that you actually used and tested.

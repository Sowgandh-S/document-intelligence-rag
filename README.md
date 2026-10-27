# Document Intelligence & RAG API

A learning project that lets you upload text-based PDF documents and ask questions about them. It uses OpenAI embeddings, persistent ChromaDB storage, and an LLM to generate answers grounded in retrieved passages with source references.

> **Status:** Learning project. Test the application and its setup instructions before describing features as working.

## Features

- Upload PDF documents through a FastAPI endpoint.
- Extract text and preserve the filename and page number.
- Split document text into overlapping chunks.
- Generate embeddings with OpenAI.
- Store chunks and embeddings persistently in ChromaDB.
- Retrieve relevant passages to answer questions.
- Return source filenames, page numbers, and excerpts.
- Respond when no relevant information is found.
- View and test endpoints through FastAPI’s interactive API documentation.

## Technology

- Python
- FastAPI
- ChromaDB
- OpenAI embeddings and chat completion
- pypdf
- Uvicorn

## Project structure

```text
document-intelligence-rag/
├── app/
│   └── main.py
├── chroma_db/              # Created locally; do not commit
├── .env                    # Local secrets; do not commit
├── .env.example
├── .gitignore
├── requirements.txt
└── README.md
```

## Requirements

- Python 3.10 or newer.
- An OpenAI API key.
- Internet access when calling the OpenAI API.

## Setup

Clone the repository and enter its folder:

```bash
git clone https://github.com/YOUR-USERNAME/document-intelligence-rag.git
cd document-intelligence-rag
```

Create and activate a virtual environment:

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

Set your OpenAI API key as an environment variable.

Linux or macOS:

```bash
export OPENAI_API_KEY="your-key-here"
```

Windows PowerShell:

```powershell
$env:OPENAI_API_KEY="your-key-here"
```

Start the API:

```bash
uvicorn app.main:app --reload
```

Open the interactive API documentation:

```text
http://127.0.0.1:8000/docs
```

Never commit a real API key. The `.env.example` file should contain placeholders only.

## API usage

### Health check

```http
GET /health
```

Returns API status, indexed chunk count, and vector-store name.

### Upload a PDF

Use `POST /documents` in `/docs` and select a text-based PDF. The API extracts text, creates chunks and embeddings, and stores them in ChromaDB.

Example response:

```json
{
  "filename": "sample-guide.pdf",
  "pages": 5,
  "chunks_added": 12,
  "total_indexed_chunks": 12,
  "storage": "chroma_db"
}
```

The exact values depend on the uploaded document and local configuration.

### Ask a question

Use `POST /questions` with a JSON body:

```json
{
  "question": "What does the document say about maintenance?"
}
```

The response includes an answer and source details when relevant passages are retrieved.

## Persistent storage

ChromaDB stores its local index in the `chroma_db/` directory. The index should remain available after restarting the server, provided that directory is preserved.

The database is local development data and should not normally be committed to GitHub. To use another storage path, set the `CHROMA_DB_PATH` environment variable.

## Configuration

The app uses:

```text
OPENAI_API_KEY=your_key_here
CHROMA_DB_PATH=chroma_db
```

`CHROMA_DB_PATH` is optional. Do not put real credentials in tracked files.

## Data and privacy

Use only documents you are allowed to upload and process. Avoid confidential, personal, or sensitive documents in this demo. Uploaded document text is sent to the configured embedding and language-model provider.

## Limitations

- The project is an educational prototype, not a production document-management service.
- The vector index is local to the machine and is not configured for multiple users.
- The index is not automatically cleared or partitioned by user.
- Scanned PDFs may need OCR because text extraction alone may not read images.
- Retrieval thresholds and chunk sizes are basic demo settings and need evaluation.
- LLM answers can be incorrect; verify them against the cited passages.
- Authentication, authorization, upload-size limits, and production monitoring are not included.

## Tests

Automated tests should be added for document extraction, chunking, persistent indexing, retrieval, citations, and error cases. Add test instructions here after the test suite exists and has been run successfully.

## Future improvements

- Add automated tests and a GitHub Actions workflow.
- Add file-size limits and stronger document validation.
- Add OCR for scanned PDFs.
- Evaluate chunking and retrieval settings on a test dataset.
- Add document deletion and index management.
- Add authentication and access controls before supporting multiple users.
- Add Docker packaging after verifying local operation.
- Add logging and latency measurement without recording sensitive document contents.

## Inspiration and attribution

This is a learning project. Credit and comply with the licenses of any third-party code, data, or assets used. Do not copy a repository and present it as independently created.

## Resume description

After you have run and tested the project, describe only what you personally implemented. For example:

> Built a FastAPI document Q&A prototype using PDF extraction, OpenAI embeddings, persistent ChromaDB storage, and source-referenced answers.

Only use this wording if those features work in your version. Do not claim production deployment, security certification, or measured accuracy without evidence.

# DocChat — RAG-Powered Document Q&A Chatbot

A conversational AI app that lets you upload any PDF and ask questions about it in plain English. Answers are grounded strictly in the document — no hallucination from general training data.

Built from scratch as a portfolio project to learn RAG architecture.

🔗 **[Live Demo →](https://hsnx999-rag-chatbot.streamlit.app)**

---

## Demo

Upload a PDF → ask questions → get accurate answers streamed in real time.

Tested on research papers, resumes, and technical reports.

---

## How it works

The app follows a 6-step pipeline split into two phases:

**At upload time (runs once per document):**

1. **Load** — PyPDF parses the document page by page
2. **Chunk** — text is split into 1000-character overlapping segments
3. **Embed** — each chunk is converted to a vector using all-MiniLM-L6-v2
4. **Store** — vectors are saved to ChromaDB on disk

**At query time (runs on every question):**

5. **Retrieve** — the question is embedded and the most similar chunks are found via cosine similarity
6. **Generate** — retrieved chunks are injected into a prompt and LLaMA 3.1 streams the answer

The 200-character overlap between chunks ensures answers that span chunk boundaries are never missed.

---

## Tech stack

| Library | Role |
|---|---|
| LangChain | Pipeline orchestration |
| ChromaDB | Local vector database |
| HuggingFace all-MiniLM-L6-v2 | Lightweight local embeddings |
| Groq LLaMA 3.1 8B | Fast, free LLM inference |
| Streamlit | Web UI |
| PyPDF | PDF text extraction |

---

## Run it locally

### Prerequisites

- Python 3.13.13
- A free Groq API key from console.groq.com

### Setup

Clone the repo and create a virtual environment:

    git clone https://github.com/hsnx999/rag-chatbot.git
    cd rag-chatbot
    python -m venv .venv
    source .venv/bin/activate
    pip install -r requirements.txt

Create a .env file in the project root:

    GROQ_API_KEY=your_key_here

Run the app:

    streamlit run app.py

Open http://localhost:8501 in your browser, upload a PDF, and start asking questions.

---

## Project structure

    rag-chatbot/
    ├── app.py                      # Streamlit UI and session state
    ├── src/
    │   ├── document_processor.py   # PDF loading and chunking
    │   ├── vector_store.py         # ChromaDB embeddings and retrieval
    │   └── rag_chain.py            # Prompt template and LLM streaming
    ├── requirements.txt
    └── .env.example

---

## What I learned building this

- How RAG works at the implementation level, not just conceptually
- Why chunk overlap matters for retrieval quality
- How vector similarity search finds semantically related text even when the exact words do not match
- How to stream LLM responses token by token in a web UI
- Debugging Python 3.13 dependency conflicts across a complex ML library ecosystem
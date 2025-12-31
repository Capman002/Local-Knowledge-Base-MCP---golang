<p align="center">
  <img src="https://img.shields.io/badge/Go-1.22+-00ADD8?style=for-the-badge&logo=go&logoColor=white" alt="Go Version"/>
  <img src="https://img.shields.io/badge/MCP-Protocol-8B5CF6?style=for-the-badge&logo=anthropic&logoColor=white" alt="MCP Protocol"/>
  <img src="https://img.shields.io/badge/Google_AI-Gemini-4285F4?style=for-the-badge&logo=google&logoColor=white" alt="Google AI"/>
  <img src="https://img.shields.io/badge/License-MIT-green?style=for-the-badge" alt="License"/>
</p>

<h1 align="center">🏗️ Senior Architect RAG</h1>

<p align="center">
  <strong>Zero-Dependency MCP Server for Software Architecture Knowledge Base</strong>
</p>

<p align="center">
  A powerful RAG (Retrieval-Augmented Generation) server that provides semantic search over your software architecture literature collection. Built with Go, leveraging Google AI embeddings and an embedded vector database for zero external dependencies.
</p>

---

## ✨ Highlights

<table>
<tr>
<td width="50%">

### 🚀 Zero Dependencies

- ❌ No Docker required
- ❌ No ChromaDB/Pinecone/Weaviate
- ❌ No Ollama or local LLMs
- ❌ No background services

</td>
<td width="50%">

### ⚡ Instant Startup

- Single binary execution
- ~50MB RAM footprint
- Embedded vector database
- Production-ready out of the box

</td>
</tr>
</table>

---

## 🏛️ Architecture

```
┌─────────────────────┐      JSON-RPC (stdio)      ┌──────────────────────────────────────────┐
│                     │◄──────────────────────────►│                                          │
│   Claude Desktop    │                            │     Senior Architect RAG (Go Binary)     │
│   Gemini Code       │                            │  ┌──────────────┐    ┌────────────────┐  │
│   Any MCP Client    │                            │  │  chromem-go  │    │   Google AI    │  │
│                     │                            │  │  (VectorDB)  │    │  (Embeddings)  │  │
└─────────────────────┘                            │  └──────────────┘    └────────────────┘  │
                                                   └──────────────────────────────────────────┘
```

**Tech Stack:**

- **Language:** Go 1.22+
- **Embeddings:** Google AI `text-embedding-004` (768 dimensions)
- **Vector Store:** [chromem-go](https://github.com/philippgille/chromem-go) (embedded)
- **Protocol:** [MCP (Model Context Protocol)](https://modelcontextprotocol.io/)
- **PDF Parser:** [ledongthuc/pdf](https://github.com/ledongthuc/pdf)

---

## 🛠️ Available Tools

The MCP server exposes the following tools to AI assistants:

| Tool                          | Description                                                                                                                 |
| ----------------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| `consultar_base_conhecimento` | Semantic search over the knowledge base. Returns the top 5 most relevant document fragments for a given technical question. |
| `verificar_status_vectordb`   | Health check for the vector database. Returns document count and memory usage.                                              |

### Example Usage

```
User: "Explique o padrão CQRS e quando usar"

AI uses: consultar_base_conhecimento("CQRS padrão quando usar")

Returns: Relevant fragments from DDD, System Design, and Microservices books
```

---

## 📚 Supported Literature

This RAG system is designed to index software architecture books and technical PDFs:

| Category           | Example Books                                               |
| ------------------ | ----------------------------------------------------------- |
| **System Design**  | System Design Interview Vol. 1 & 2 (Alex Xu)                |
| **DDD**            | Implementing Domain-Driven Design (Vaughn Vernon)           |
| **Go Programming** | The Go Programming Language, Learning Go, Concurrency in Go |
| **Microservices**  | Building Microservices (Sam Newman)                         |
| **Software Craft** | The Pragmatic Programmer, Clean Code principles             |
| **Resilience**     | Chaos Engineering: System Resiliency in Practice            |

---

## 🚀 Quick Start

### Prerequisites

- **Go 1.22+** (only for building)
- **Google AI API Key** ([Get one here](https://aistudio.google.com/app/apikey))

### 1. Clone & Build

```powershell
# Clone the repository
git clone https://github.com/Capman002/Local-Knowledge-Base-MCP---golang.git
cd Local-Knowledge-Base-MCP---golang

# Build binaries
go build -o senior-architect-rag.exe .
go build -o ingest.exe ./cmd/ingest
```

### 2. Configure Environment

Create a `.env` file:

```env
# Required - Google AI Studio API Key
GOOGLE_API_KEY=your-api-key-here

# Optional - Customize these if needed
EMBEDDING_MODEL=text-embedding-004
COLLECTION_NAME=biblioteca_arquitetura
DB_PATH=vector_db
```

### 3. Index Your Documents

Place your PDF books in the `biblioteca_docs/` folder, then run:

```powershell
.\ingest.exe
```

**Output:**

```
╔════════════════════════════════════════════════════════════╗
║  📚 Ingestor de Documentos                                 ║
╚════════════════════════════════════════════════════════════╝

📁 biblioteca_docs | 🧠 text-embedding-004
🔑 API Key: ****xxxx
📚 18 PDFs encontrados

📖 System Design Interview.pdf... 342 chunks ✅ 342 indexados
📖 Implementing DDD.pdf... 856 chunks ✅ 856 indexados
...

✅ Concluído: 18 arquivos, 4521 chunks, 4521 total no banco
```

### 4. Configure Your MCP Client

#### Claude Desktop / Antigravity

Add to your MCP configuration file:

**Windows:** `%APPDATA%\Claude\claude_desktop_config.json`  
**macOS:** `~/Library/Application Support/Claude/claude_desktop_config.json`

```json
{
  "mcpServers": {
    "senior-architect-rag": {
      "command": "C:/Senior-Architect-RAG/senior-architect-rag.exe",
      "args": [],
      "env": {}
    }
  }
}
```

> 💡 The `.env` file is automatically loaded from the executable's directory.

---

## 📁 Project Structure

```
Senior-Architect-RAG/
├── main.go                      # MCP Server (handles tool calls)
├── cmd/
│   └── ingest/
│       └── main.go              # PDF Ingestor (chunking + embedding)
├── biblioteca_docs/             # Your PDF collection (git-ignored)
├── vector_db/                   # Persistent vector store (auto-created)
├── senior-architect-rag.exe     # Built MCP server binary
├── ingest.exe                   # Built ingestor binary
├── .env                         # Your API key (git-ignored)
├── .env.example                 # Template for environment variables
├── claude_config_example.json   # Example MCP client configuration
└── README.md
```

---

## ⚙️ Configuration Options

| Variable          | Default                  | Description                                |
| ----------------- | ------------------------ | ------------------------------------------ |
| `GOOGLE_API_KEY`  | _(required)_             | Your Google AI Studio API key              |
| `EMBEDDING_MODEL` | `text-embedding-004`     | Google embedding model to use              |
| `COLLECTION_NAME` | `biblioteca_arquitetura` | Name of the vector collection              |
| `DB_PATH`         | `vector_db`              | Path to store the vector database          |
| `DOCS_DIR`        | `biblioteca_docs`        | Path to your PDF documents (ingestor only) |

---

## 📊 Performance Comparison

| Aspect           | Traditional RAG (Docker) | Senior Architect RAG |
| ---------------- | ------------------------ | -------------------- |
| **Dependencies** | Docker, ChromaDB, Python | None                 |
| **Startup Time** | 10-30 seconds            | Instant              |
| **RAM Usage**    | ~500MB+                  | ~50MB                |
| **Disk Space**   | ~2GB (containers)        | ~20MB (binary + DB)  |
| **Maintenance**  | Container updates        | Zero                 |
| **Portability**  | Docker required          | Single binary        |

---

## 🔧 How It Works

### Ingestion Pipeline

```
PDF Files → Text Extraction → Chunking (800 chars, 150 overlap)
         → Embedding (Google AI) → Vector Storage (chromem-go)
```

### Query Pipeline

```
User Question → Embedding (Google AI) → Semantic Search (Top 5)
             → Return Fragments with Sources and Similarity Scores
```

### Chunking Strategy

- **Chunk Size:** 800 characters
- **Overlap:** 150 characters
- **Batch Size:** 20 documents per API call
- **Deduplication:** SHA-256 hash-based document IDs

---

## 🐛 Troubleshooting

### "Cannot extract text from PDF"

Some PDFs are scanned images without embedded text. Use OCR tools (Adobe Acrobat, ABBYY FineReader) to convert them first.

### "API Key not found"

Ensure your `.env` file is in the same directory as the executable, or set `GOOGLE_API_KEY` as a system environment variable.

### "Port already in use" (doesn't apply)

This server uses **stdio** communication, not HTTP. No ports are opened.

---

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

<p align="center">
  <strong>Built with ❤️ for the Software Architecture Community</strong>
</p>

<p align="center">
  <a href="https://github.com/Capman002/Local-Knowledge-Base-MCP---golang">
    <img src="https://img.shields.io/github/stars/Capman002/Local-Knowledge-Base-MCP---golang?style=social" alt="GitHub Stars"/>
  </a>
</p>

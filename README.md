# 🏗️ Senior Architect RAG

> **Zero-Dependency MCP Server** para consulta de literatura de Arquitetura de Software via RAG.

**Stack:** Go + Google AI (Gemini) + chromem-go (banco vetorial embutido)

## ✨ Zero Dependências Externas

- ❌ ~~Docker~~
- ❌ ~~ChromaDB Server~~
- ❌ ~~Ollama~~
- ❌ ~~Terminal rodando~~

**Apenas:** Execute o binário e pronto!

## 🏛️ Arquitetura

```text
┌─────────────────┐     ┌─────────────────────────────────────┐
│   Claude/IDE    │────▶│     Go MCP Server (tudo embutido)   │
│   (Cliente)     │     │  ┌─────────────┐  ┌──────────────┐  │
└─────────────────┘     │  │ chromem-go  │  │  Google AI   │  │
                        │  │ (VectorDB)  │  │ (Embeddings) │  │
                        │  └─────────────┘  └──────────────┘  │
                        └─────────────────────────────────────┘
```

## 📋 Pré-requisitos

Apenas **Go 1.22+** para compilar (uma vez):

```powershell
go version
```

## 🚀 Instalação (Uma Vez)

```powershell
cd C:\Senior-Architect-RAG

# Compilar
go build -o senior-architect-rag.exe .
go build -o ingest.exe ./cmd/ingest
```

## 📚 Indexando seus Livros (Uma Vez)

1. Coloque seus PDFs na pasta `biblioteca_docs/`

2. Execute:

   ```powershell
   .\ingest.exe
   ```

   > A ingestão usa a API do Google AI para gerar embeddings.

## ⚙️ Configuração MCP

Adicione ao seu Antigravity/Claude Desktop:

```json
    "senior-architect-rag": {
      "command": "C:/Senior-Architect-RAG/senior-architect-rag.exe",
      "args": [],
      "env": {}
    }
```

A API key é lida automaticamente do arquivo `.env`.

## 🔧 Arquivo .env

Crie um arquivo `.env` na raiz do projeto:

```env
GOOGLE_API_KEY=sua-api-key-aqui
EMBEDDING_MODEL=text-embedding-004
COLLECTION_NAME=biblioteca_arquitetura
DB_PATH=vector_db
```

## 🛠️ Tools Disponíveis

### `consultar_base_conhecimento`

Consulta a base de conhecimento via busca semântica.

### `verificar_status_vectordb`

Health check do banco vetorial.

## 📁 Estrutura

```text
Senior-Architect-RAG/
├── senior-architect-rag.exe   # Servidor MCP
├── ingest.exe                 # Ingestor de PDFs
├── vector_db/                 # Banco vetorial (criado automaticamente)
├── biblioteca_docs/           # Seus PDFs
├── .env                       # Sua API key (gitignored)
└── README.md
```

## 📊 Vantagens

| Aspecto      | Antes (Docker)            | Agora (Embutido) |
| ------------ | ------------------------- | ---------------- |
| Dependências | Docker, ChromaDB          | Nenhuma          |
| Startup      | Precisa iniciar serviços  | Instantâneo      |
| RAM          | ~500MB (ChromaDB)         | ~50MB            |
| Manutenção   | Containers para gerenciar | Zero             |

## 📜 Licença

MIT

<p align="center">
  <img src="https://img.shields.io/badge/Go-1.22+-00ADD8?style=for-the-badge&logo=go&logoColor=white" alt="Go Version"/>
  <img src="https://img.shields.io/badge/MCP-Protocol-8B5CF6?style=for-the-badge&logo=anthropic&logoColor=white" alt="MCP Protocol"/>
  <img src="https://img.shields.io/badge/Google_AI-Embeddings-4285F4?style=for-the-badge&logo=google&logoColor=white" alt="Google AI"/>
  <img src="https://img.shields.io/badge/License-MIT-green?style=for-the-badge" alt="License"/>
</p>

<h1 align="center">🏗️ Senior Architect RAG</h1>

<p align="center">
  <strong>Zero-Dependency MCP Server for Local Knowledge Base</strong>
</p>

<p align="center">
  Um servidor MCP que permite que assistentes de IA consultem sua base de conhecimento local via busca semântica (RAG). Indexe seus PDFs e faça perguntas em linguagem natural.
</p>

---

## ✨ Características

- **Zero Dependências Externas** - Sem Docker, sem servidores externos, sem Python
- **Banco Vetorial Embutido** - chromem-go persiste localmente
- **Embeddings via Google AI** - Modelo `text-embedding-004` (768 dimensões)
- **Protocolo MCP** - Compatível com Claude Desktop, Antigravity, e outros clientes MCP
- **Comunicação Stdio** - Sem portas HTTP expostas

---

## 🏛️ Arquitetura

```
┌─────────────────────┐      JSON-RPC (stdio)      ┌──────────────────────────────────────────┐
│                     │◄──────────────────────────►│                                          │
│   Claude Desktop    │                            │     Senior Architect RAG (Go Binary)     │
│   Antigravity       │                            │  ┌──────────────┐    ┌────────────────┐  │
│   Outro MCP Client  │                            │  │  chromem-go  │    │   Google AI    │  │
│                     │                            │  │  (VectorDB)  │    │  (Embeddings)  │  │
└─────────────────────┘                            │  └──────────────┘    └────────────────┘  │
                                                   └──────────────────────────────────────────┘
```

---

## 🛠️ Tools Disponíveis

### `consultar_base_conhecimento`

Consulta a base de conhecimento via busca semântica.

| Parâmetro  | Tipo   | Obrigatório | Descrição                                        |
| ---------- | ------ | ----------- | ------------------------------------------------ |
| `pergunta` | string | ✅          | A questão técnica ou padrão que deseja pesquisar |

**Retorna:** Os 5 fragmentos mais relevantes com score de similaridade, fonte e conteúdo.

---

### `verificar_status_vectordb`

Verifica o status do banco vetorial.

| Parâmetro  | Tipo | Obrigatório | Descrição             |
| ---------- | ---- | ----------- | --------------------- |
| _(nenhum)_ | -    | -           | Não requer parâmetros |

**Retorna:** Status do banco, contagem de documentos indexados e uso de memória.

---

## 🚀 Instalação

### Pré-requisitos

- **Go 1.22+** (para compilar)
- **Google AI API Key** - [Obter gratuitamente](https://aistudio.google.com/app/apikey)

### Compilar

```bash
git clone https://github.com/Capman002/Local-Knowledge-Base-MCP---golang.git
cd Local-Knowledge-Base-MCP---golang

go build -o senior-architect-rag.exe .
go build -o ingest.exe ./cmd/ingest
```

### Configurar

Crie um arquivo `.env` na raiz do projeto:

```env
# Obrigatório
GOOGLE_API_KEY=sua-api-key-aqui

# Opcional (valores padrão mostrados)
EMBEDDING_MODEL=text-embedding-004
COLLECTION_NAME=biblioteca_arquitetura
DB_PATH=vector_db
```

### Indexar Documentos

1. Coloque seus PDFs na pasta `biblioteca_docs/`
2. Execute o ingestor:

```bash
./ingest.exe
```

---

## ⚙️ Configuração MCP

### Claude Desktop / Antigravity

Adicione ao arquivo de configuração:

**Windows:** `%APPDATA%\Claude\claude_desktop_config.json`  
**macOS:** `~/Library/Application Support/Claude/claude_desktop_config.json`

```json
{
  "mcpServers": {
    "senior-architect-rag": {
      "command": "C:/caminho/para/senior-architect-rag.exe",
      "args": [],
      "env": {}
    }
  }
}
```

---

## 📁 Estrutura do Projeto

```
Senior-Architect-RAG/
├── main.go                      # Servidor MCP
├── cmd/ingest/main.go           # Ingestor de PDFs
├── biblioteca_docs/             # Seus PDFs (git-ignored)
├── vector_db/                   # Banco vetorial (auto-criado)
├── .env                         # Configurações (git-ignored)
├── .env.example                 # Template de configuração
└── claude_config_example.json   # Exemplo de config MCP
```

---

## 🔧 Variáveis de Ambiente

| Variável          | Padrão                   | Descrição                            |
| ----------------- | ------------------------ | ------------------------------------ |
| `GOOGLE_API_KEY`  | _(obrigatório)_          | Chave da API Google AI Studio        |
| `EMBEDDING_MODEL` | `text-embedding-004`     | Modelo de embeddings                 |
| `COLLECTION_NAME` | `biblioteca_arquitetura` | Nome da collection no banco vetorial |
| `DB_PATH`         | `vector_db`              | Caminho do banco vetorial            |
| `DOCS_DIR`        | `biblioteca_docs`        | Caminho dos PDFs (apenas ingestor)   |

---

## 🔍 Detalhes Técnicos

| Componente               | Implementação                      |
| ------------------------ | ---------------------------------- |
| **Linguagem**            | Go                                 |
| **Versão**               | 3.0.0                              |
| **Embeddings**           | Google AI `text-embedding-004`     |
| **Banco Vetorial**       | chromem-go (embutido, persistente) |
| **Chunk Size**           | 800 caracteres                     |
| **Chunk Overlap**        | 150 caracteres                     |
| **Resultados por Query** | Top 5                              |
| **Protocolo**            | MCP via stdio                      |

---

## 🐛 Solução de Problemas

### "GOOGLE_API_KEY não definida"

Certifique-se de que o arquivo `.env` existe no mesmo diretório do executável.

### "Nenhum PDF encontrado"

Verifique se a pasta `biblioteca_docs/` existe e contém arquivos `.pdf`.

### "Nenhum resultado para a consulta"

Execute `verificar_status_vectordb` para verificar se há documentos indexados. Se a contagem for zero, execute `ingest.exe`.

### "Erro ao extrair texto do PDF"

Alguns PDFs são imagens escaneadas sem texto embutido. Use ferramentas de OCR para convertê-los.

---

## 🤝 Contribuindo

Contribuições são bem-vindas! Veja [CONTRIBUTING.md](CONTRIBUTING.md) para detalhes.

---

## 📄 Licença

Este projeto está licenciado sob a Licença MIT - veja [LICENSE](LICENSE) para detalhes.

---

## 🔗 Dependências

- [mark3labs/mcp-go](https://github.com/mark3labs/mcp-go) - Biblioteca MCP para Go
- [philippgille/chromem-go](https://github.com/philippgille/chromem-go) - Banco vetorial embutido
- [ledongthuc/pdf](https://github.com/ledongthuc/pdf) - Parser de PDF
- [joho/godotenv](https://github.com/joho/godotenv) - Carregamento de .env

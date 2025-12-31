# Contribuindo

Olá! 👋 Contribuições são bem-vindas.

## 📜 Termos

Contribuições para este projeto são liberadas ao público sob a [licença MIT](LICENSE).

Este projeto segue um [Código de Conduta](CODE_OF_CONDUCT.md). Ao participar, você concorda em seguir seus termos.

---

## 🔧 Pré-requisitos

- **Go 1.22+** - [Download](https://go.dev/doc/install)
- **Google AI API Key** - [Obter aqui](https://aistudio.google.com/app/apikey)

---

## 🚀 Submetendo um Pull Request

1. Fork e clone o repositório
2. Compile para verificar:
   ```bash
   go build -o senior-architect-rag.exe .
   go build -o ingest.exe ./cmd/ingest
   ```
3. Crie uma branch: `git checkout -b minha-feature`
4. Faça suas mudanças e commit
5. Push e abra um PR para a branch `main`

---

## 📝 Convenção de Commits

| Tipo       | Descrição           |
| ---------- | ------------------- |
| `feat`     | Nova funcionalidade |
| `fix`      | Correção de bug     |
| `docs`     | Documentação        |
| `refactor` | Refatoração         |
| `chore`    | Manutenção          |

---

## 🏗️ Estrutura

```
├── main.go                 # Servidor MCP
├── cmd/ingest/main.go      # Ingestor de PDFs
├── biblioteca_docs/        # PDFs (git-ignored)
├── vector_db/              # Banco vetorial
└── .env                    # Configurações
```

---

## 📚 Recursos

- [MCP Protocol](https://modelcontextprotocol.io/)
- [chromem-go](https://github.com/philippgille/chromem-go)
- [mcp-go](https://github.com/mark3labs/mcp-go)

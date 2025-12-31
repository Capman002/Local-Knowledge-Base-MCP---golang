# Segurança

## 🚨 Reportando Vulnerabilidades

**Não reporte vulnerabilidades através de issues públicas.**

Entre em contato através do GitHub para reportar problemas de segurança.

### O que Incluir

- Descrição do problema
- Passos para reproduzir
- Impacto potencial

---

## 🔒 Considerações de Segurança

### API Keys

- A `GOOGLE_API_KEY` é armazenada no arquivo `.env`
- O arquivo `.env` está no `.gitignore`
- **Nunca** commite sua API key

### Dados Locais

- Os PDFs em `biblioteca_docs/` ficam apenas no seu computador
- O banco vetorial `vector_db/` também é local
- Ambos estão no `.gitignore`

### Comunicação

- O servidor usa **stdio**, não HTTP
- Nenhuma porta de rede é aberta

# ADR 001 — POC LangChain: Assistente RAG de Boas Práticas Angular

**Data:** 2026-06-07
**Status:** Proposto

## Contexto

Estou fazendo um curso de LangChain e quero um repositório de POC pequeno para consolidar os conceitos na prática. Preciso de um caso de uso que:

- Exercite os blocos fundamentais do LangChain (prompts, chains, embeddings, vector store, retriever);
- Tenha escopo pequeno o suficiente para terminar;
- Permita **provar** que o RAG funciona (resposta vinda dos meus documentos, não do conhecimento de treino do modelo).

O domínio escolhido é **boas práticas de Angular** (skills, arquitetura, design), porque:

1. É um assunto que domino — consigo avaliar a qualidade das respostas;
2. Angular 17+ (signals, standalone components, control flow `@if/@for`) é recente e muda rápido, o que evidencia quando a resposta vem do RAG e não do modelo;
3. Posso incluir convenções internas minhas (ex.: prefixo `app-feature-`) como "marcador": se a resposta citar isso, o RAG está comprovadamente funcionando.

## Decisão

Criar a POC com o seguinte escopo, em ordem de implementação:

### 1. Chain básica (aquecimento)
- `PromptTemplate` + modelo + parser usando LCEL (`prompt | llm | parser`).
- Ex.: "explique este conceito de Angular em 3 níveis: júnior, pleno, sênior".

### 2. Ingestão de documentos (coração da POC)
- Fonte: [Angular Style Guide](https://angular.dev/style-guide) + markdowns próprios sobre arquitetura/design em `docs/`.
- Pipeline: `DocumentLoader` (markdown/web) → `TextSplitter` (chunking) → `OpenAIEmbeddings` → Pinecone.

### 3. RAG chain
- Retriever do Pinecone + prompt que injeta o contexto + LLM.
- Pergunta de teste: *"quando devo usar standalone components vs NgModules?"* → resposta baseada nos documentos, com citação da fonte.

### 4. Memória de conversa (opcional)
- Histórico de chat para perguntas de follow-up ("e nesse caso, como fica o lazy loading?").

### 5. Agent com tool (stretch goal)
- Agent que decide: buscar no índice de boas práticas OU responder direto.

### Stack
- Python 3.12 (ambiente conda `langchain` — ver README do curso)
- `langchain`, `langchain-openai`, `langchain-pinecone`, `python-dotenv`
- Chaves em `.env` (`OPENAI_API_KEY`, `PINECONE_API_KEY`) — fora do git

### Estrutura do repositório

```
poc-langchain-angular/
├── .env                  # chaves de API (no .gitignore!)
├── docs/                 # markdowns de boas práticas Angular (base de conhecimento)
├── ingestion.py          # carrega docs/ → chunks → embeddings → Pinecone
├── main.py               # chat RAG via terminal
└── README.md
```

## Fora de escopo (decisão consciente)

| Item | Por quê |
|---|---|
| Interface web (Streamlit etc.) | Adiciona ruído sem ensinar LangChain |
| Múltiplos LLMs / fallbacks | Complexidade prematura |
| LangGraph | É o passo seguinte, não a primeira POC |

## Consequências

- A POC fica focada em RAG, que é o caso de uso mais comum de LangChain no mercado.
- Interação só via terminal — suficiente para validar os conceitos.
- Critério de sucesso claro: perguntar sobre uma convenção interna presente apenas nos meus docs e receber a resposta correta com citação da fonte.
- Evoluções futuras (LangGraph, interface web, avaliação automática com LangSmith) ficam para ADRs seguintes.

# Inbox Advising Agent

## Architecture

```mermaid
flowchart LR
  C[Client (Swagger or HTML)] -->|POST /advise (JSON body)| API[FastAPI API]
  API --> SAN[Sanitize email]
  SAN --> RED[Redact PII]
  RED --> TRIAGE[ChatGPT · Parse / Triage → ParsedIntent]
  TRIAGE -->|topic = advising| RET[Embeddings + FAISS search]
  RET --> RULES[Rules: prereqs, credits, modality, term]
  RULES --> RERANK[ChatGPT · Rerank + citations]
  TRIAGE -->|topic ≠ advising| HANDLERS[Handlers: meeting, rec letter, hold, forms]
  RERANK --> DRAFT[ChatGPT · Draft ≤ 250 words + 1 question]
  HANDLERS --> DRAFT
  DRAFT --> HIL[Human review (approve / edit)]
  HIL --> RESP[JSON response: summary, recs, draft, intent]
  RESP --> C

  subgraph Data_Config [Data & Config]
    CAT[(catalog.json)] --> RET
    CFG[(config.yaml / .env)] --> API
    API --> LOG[(Non-PII logs & metrics)]
  end


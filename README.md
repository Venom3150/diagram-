# Inbox Advising Agent

## Architecture

```mermaid
flowchart LR
  C[Client (Swagger/HTML)] -->|POST /advise (body)| API[FastAPI API]
  API --> SAN[Sanitize email]
  SAN --> RED[Redact PII]
  RED --> TRIAGE[ChatGPT: Parse/Triage -> ParsedIntent]
  TRIAGE -->|topic=advising| RET[Embeddings + FAISS search]
  RET --> RULES[Rules: prereqs, credits, modality, term]
  RULES --> RERANK[ChatGPT: Re-rank + Citations]
  TRIAGE -->|topic!=advising| HANDLERS[Handlers: meeting, rec letter, hold, forms]
  RERANK --> DRAFT[ChatGPT: Draft <=250w + 1 Q]
  HANDLERS --> DRAFT
  DRAFT --> HIL[Human Review (approve/edit)]
  HIL --> RESP[JSON response: summary, recs, draft, intent]
  RESP --> C
  subgraph Data_and_Config
    CAT[(catalog.json)] --> RET
    CFG[(config.yaml/.env)] --> API
    LOG[(Non-PII Logs & Metrics)] <-- API
  end

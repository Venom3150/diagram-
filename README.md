## Architecture

```mermaid
flowchart LR
  C[Client (Swagger/HTML)] -->|POST /advise {body}| API[FastAPI API]
  API --> SAN[Sanitize email]
  SAN --> RED[Redact PII]
  RED --> TRIAGE[(ChatGPT\nParse/Triage → ParsedIntent)]
  TRIAGE -->|topic=advising| RET[(Embeddings+FAISS\nCourse Search)]
  RET --> RULES[Rules: prereqs, credits,\nmodality, term]
  RULES --> RERANK[(ChatGPT\nRe-rank + Citations)]
  TRIAGE -->|topic≠advising| HANDLERS[Topic Handlers:\nmeeting, rec letter, hold, forms]
  RERANK --> DRAFT[(ChatGPT\nDraft ≤250w + 1 Q)]
  HANDLERS --> DRAFT
  DRAFT --> HIL[Human Review\n(approve/edit)]
  HIL --> RESP[JSON Response:\nsummary, recs, draft, intent]
  RESP --> C
  subgraph Data & Config
    CAT[(catalog.json)] --> RET
    CFG[(config.yaml/.env)] --> API
    LOG[(Non-PII Logs & Metrics)] <-- API
  end

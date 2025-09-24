
```mermaid
flowchart LR
  C[Client (Swagger or HTML)]
  API[FastAPI API]
  SAN[Sanitize email]
  RED[Redact PII]
  TRIAGE[ChatGPT Parse/Triage -> ParsedIntent]
  RET[Embeddings + FAISS search]
  RULES[Rules: prereqs, credits, modality, term]
  RERANK[ChatGPT Rerank + citations]
  HANDLERS[Handlers: meeting, rec letter, hold, forms]
  DRAFT[ChatGPT Draft <= 250 words + 1 question]
  HIL[Human review (approve/edit)]
  RESP[JSON response: summary, recs, draft, intent]
  CAT[(catalog.json)]
  CFG[(config.yaml / .env)]
  LOG[(Non-PII logs & metrics)]

  C -->|POST /advise| API
  API --> SAN
  SAN --> RED
  RED --> TRIAGE
  TRIAGE -->|topic = advising| RET
  RET --> RULES
  RULES --> RERANK
  RERANK --> DRAFT
  TRIAGE -->|topic != advising| HANDLERS
  HANDLERS --> DRAFT
  DRAFT --> HIL
  HIL --> RESP
  RESP --> C

  subgraph Data_Config [Data & Config]
    CAT --> RET
    CFG --> API
    API --> LOG
  end




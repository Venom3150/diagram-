flowchart LR
  C[Client (Swagger/HTML)] -->|POST /advise (body)| API[FastAPI API]
  API --> SAN[Sanitize email]
  SAN --> RED[Redact PII]
  RED --> TRIAGE[ChatGPT:<br/>Parse/Triage -> ParsedIntent]
  TRIAGE -->|topic=advising| RET[Embeddings + FAISS<br/>Course Search]
  RET --> RULES[Rules:<br/>prereqs, credits, modality, term]
  RULES --> RERANK[ChatGPT:<br/>Re-rank + Citations]
  TRIAGE -->|topic!=advising| HANDLERS[Topic Handlers:<br/>meeting, rec letter, hold, forms]
  RERANK --> DRAFT[ChatGPT:<br/>Draft <=250w + 1 Q]
  HANDLERS --> DRAFT
  DRAFT --> HIL[Human Review<br/>(approve/edit)]
  HIL --> RESP[JSON Response:<br/>summary, recs, draft, intent]
  RESP --> C
  subgraph Data_and_Config
    CAT[(catalog.json)] --> RET
    CFG[(config.yaml/.env)] --> API
    LOG[(Non-PII Logs & Metrics)] <-- API
  end

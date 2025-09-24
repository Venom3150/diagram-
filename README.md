
```mermaid
flowchart LR
  C[Client Swagger or HTML]
  API[FastAPI API]
  SAN[Sanitize email]
  RED[Redact PII]
  TRIAGE[ChatGPT Parse and Triage to ParsedIntent]
  RET[Embeddings and FAISS search]
  RULES[Rules prereqs credits modality term]
  RERANK[ChatGPT Rerank with citations]
  HANDLERS[Handlers meeting rec letter hold forms]
  DRAFT[ChatGPT Draft max 250 words plus 1 question]
  HIL[Human review approve or edit]
  RESP[JSON response summary recs draft intent]
  CAT[(catalog json)]
  CFG[(config env)]
  LOG[(Non PII logs and metrics)]

  C --> API
  API --> SAN
  SAN --> RED
  RED --> TRIAGE
  TRIAGE --> RET
  RET --> RULES
  RULES --> RERANK
  RERANK --> DRAFT
  TRIAGE --> HANDLERS
  HANDLERS --> DRAFT
  DRAFT --> HIL
  HIL --> RESP
  RESP --> C

  subgraph Data_Config [Data and Config]
    CAT --> RET
    CFG --> API
    API --> LOG
  end






```mermaid
flowchart TB
  %% ---------- Clients ----------
  C1[Faculty UI or Swagger]
  C2[Email Providers IMAP or Graph]
  C1 -->|POST advise / approve draft| GWAY
  C2 -->|poll or webhook| INGEST

  %% ---------- Edge / API ----------
  GWAY[API Gateway + Auth]
  GWAY --> APP[FastAPI App]

  %% ---------- Ingestion ----------
  subgraph ING[Ingestion]
    direction TB
    INGEST[Ingest Service]
    QUEUE[Job Queue]
    INGEST --> QUEUE
  end
  APP --> QUEUE

  %% ---------- Pipeline Workers ----------
  subgraph PIPE[Processing Pipeline Workers]
    direction TB
    SAN[Sanitize email]
    RED[Redact PII]
    TRIAGE[LLM triage to ParsedIntent]
    RET[Retrieval adapter]
    RULES[Rules prereqs credits modality term]
    RERANK[LLM rerank with citations]
    DRAFT[LLM draft <= 250 words + one question]
    HIL[Human in the loop review UI]
    SAN --> RED --> TRIAGE
    TRIAGE -->|advising| RET --> RULES --> RERANK --> DRAFT
    TRIAGE -->|not advising| HANDLERS[Handlers meeting rec letter hold forms] --> DRAFT
    DRAFT --> HIL --> RESP[JSON response]
  end

  %% work dispatch
  QUEUE --> SAN
  RESP --> APP
  APP --> C1

  %% ---------- Data and Retrieval ----------
  subgraph DATA[Data and Retrieval]
    direction TB
    DB[(Relational DB emails runs drafts)]
    BLOB[(Object store attachments)]
    CAT[(catalog json or csv)]
    EMB[Embeddings job]
    INDEX[(FAISS index)]
    CAT --> EMB --> INDEX
    RET --> INDEX
    APP --> DB
    APP --> BLOB
    PIPE --> DB
  end

  %% ---------- Observability and Safety ----------
  subgraph OBS[Observability and Safety]
    direction TB
    LOGS[(Structured logs)]
    METRICS[(Latency accuracy safety metrics)]
    TRACES[(Traces)]
    POLICY[PII policy allowlist retention TTL]
    APP --> LOGS
    PIPE --> LOGS
    APP --> METRICS
    PIPE --> METRICS
    APP --> TRACES
    PIPE --> TRACES
    RED --> POLICY
  end

  %% ---------- Evaluation ----------
  subgraph EVAL[Evaluation and QA]
    direction TB
    GOLD[Golden email set]
    CM[Confusion matrix and draft quality rubric]
    RUNNER[Eval runner]
    GOLD --> RUNNER --> CM
    APP --> RUNNER
  end

  %% ---------- Ops ----------
  subgraph OPS[Ops and Deployment]
    direction TB
    CFG[(config env secrets manager)]
    CI[CI CD]
    DOCKER[Docker image]
    HEALTH[healthchecks retries backoff]
    CFG --> APP
    CFG --> PIPE
    DOCKER --> CI
    CI --> APP
    CI --> PIPE
    APP --> HEALTH
    PIPE --> HEALTH
  end





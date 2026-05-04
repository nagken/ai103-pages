# Domain 5 — Implement Information Extraction Solutions (10–15%)

> The AI-103 evolution of "knowledge mining" + "Document Intelligence". The headline tools are now **Azure AI Search** for retrieval and **Content Understanding** for ingestion-grade extraction. This domain is about **building the grounding pipeline** that powers RAG and agents.

## Mind map

```mermaid
mindmap
  root((Information Extraction))
    Ingest
      Documents
      Images
      Audio
      Video
    Index
      Hybrid
      Vector
      Semantic
      Integrated vectorization
    Enrich
      Built-in skills
      Custom skills
      OCR
      Layout
      Field extraction
    RAG ingestion
      Chunking
      Citations
      Provenance
    Connect to agents
      Tools
      Knowledge stores
      Workflows
    Content Understanding
      Multimodal pipelines
      Markdown output
      Structured fields
      Analyzers
```

## Build retrieval and grounding pipelines

```mermaid
flowchart LR
    SRC[Sources - blob, SharePoint, web, audio, video] --> CU[Content Understanding analyzer]
    CU --> CLEAN[Clean markdown + structured fields]
    CLEAN --> CHUNK[Chunking strategy]
    CHUNK --> EMB[Embeddings - integrated vectorization]
    EMB --> IDX[Azure AI Search index - hybrid + vector]
    IDX --> RANK[Semantic ranker]
    RANK --> AGT[Agent tool / RAG workflow]
```

### Search modes — pick consciously

| Mode | When | Notes |
| --- | --- | --- |
| **Keyword (BM25)** | Exact strings, IDs, codes | Cheap, deterministic |
| **Vector** | Semantic similarity, paraphrasing | Needs embedding model + vector field |
| **Hybrid** | Default for enterprise RAG | Best recall in practice |
| **Semantic ranker** | Re-rank top-N for precision + captions + answers | Quality lift on top of hybrid |
| **Integrated vectorization** | You want indexer to embed + chunk | Push-button RAG ingestion |

> Trap: "Improve answer quality without retraining" → **enable hybrid + semantic ranker** before suggesting a bigger model.

## Enrichment skills

```mermaid
flowchart LR
    DOC[Document] --> SK[Skillset]
    SK --> OCR[OCR for scanned text]
    SK --> LAY[Layout analysis - tables, sections]
    SK --> ENT[Entity / key phrase extraction]
    SK --> EMB[Embedding skill]
    SK --> CUST[Custom Web API skill / function tool]
    SK --> CU[Content Understanding skill]
    SK --> IDX[Azure AI Search index]
```

| Skill | Use it for |
| --- | --- |
| **OCR** | Image-only PDFs, scans, photos of documents |
| **Layout** | Tables, sections, reading order, page-aware chunks |
| **Field extraction** | Pull invoice / contract / form fields |
| **Embedding skill** | Vectorize chunks during indexing |
| **Custom skill** | Bespoke logic via Azure Function / API |
| **Content Understanding skill** | Multimodal extraction → markdown + JSON |

## Configure RAG ingestion

```mermaid
flowchart TD
    R[Raw source] --> A[Content Understanding analyzer]
    A --> M[Markdown + structured fields]
    M --> C{Chunk strategy}
    C -- Page / heading aware --> C1[Heading-aware chunks]
    C -- Token-windowed --> C2[Fixed token + overlap]
    C1 --> V[Embed chunks]
    C2 --> V
    V --> I[AI Search index with vector + semantic config]
    I --> Q[Agent / app retrieves top-k]
    Q --> CITE[Citations + provenance]
```

Checklist for a production RAG ingestion:

- **One analyzer per content type** (contracts, invoices, slides, screenshots, audio, video).
- **Heading-aware chunks** beat fixed windows for prose; **layout-aware** for tables.
- **Vector field + semantic configuration** on the index; **hybrid query** at retrieval time.
- **Citations** = chunk IDs + source URI + page / segment.
- **Re-index on change** (push or pull); track **freshness** as a SLO.
- **Access control** — index per tenant, or per-document security trimming via filter.

## Extract content from documents

```mermaid
flowchart LR
    PDF[Document - PDF, image, mixed] --> CU[Content Understanding pipeline]
    CU --> OCR2[OCR]
    CU --> LAY2[Layout]
    CU --> FX[Field extraction]
    OCR2 --> OUT[Clean output]
    LAY2 --> OUT
    FX --> OUT
    OUT --> MD[Markdown for RAG]
    OUT --> JSON[Structured JSON for downstream apps]
    MD --> AGT[Agent tool / RAG context]
    JSON --> SOR[System of record]
```

| Output | Use it for |
| --- | --- |
| **Markdown** | Drop into prompts and RAG; preserves structure for the LLM |
| **Structured JSON / fields** | Update business systems, validate, audit |
| **Pro-mode analyzers** | Multi-step reasoning over complex layouts |

## Connect retrieval directly to agents

```mermaid
flowchart LR
    AGT[Foundry Agent] --> TOOL[Tool: AI Search]
    AGT --> TOOL2[Tool: Content Understanding]
    AGT --> TOOL3[Tool: Custom function]
    TOOL --> IDX[Hybrid + semantic index]
    TOOL2 --> ANALYZER[Pro-mode analyzer]
    AGT --> KNOW[Knowledge store - vector + provenance]
```

Patterns:

- Expose **AI Search** as an agent tool so the agent can pick query type (keyword, vector, hybrid).
- Expose **Content Understanding analyzers** as tools for on-demand structured extraction.
- Persist **knowledge stores** for long-term agent memory (vector + metadata + ACL).

## Decision flow

```mermaid
flowchart TD
    Q[Scenario] --> S{What is asked?}
    S -- Find relevant chunks across many docs --> SR[Azure AI Search hybrid + semantic]
    S -- Turn doc into clean grounded text --> CU[Content Understanding analyzer]
    S -- Pull specific fields from a form --> FE[Field extraction analyzer]
    S -- Vector similarity only --> V[Vector index]
    S -- Re-rank for higher precision --> SEM[Add semantic ranker]
    S -- Embed during ingestion automatically --> IV[Integrated vectorization]
    S -- Agent needs retrieval at runtime --> AT[Expose AI Search as a tool]
```

## Domain summary

- **Content Understanding ingests; AI Search retrieves.** Memorize this split.
- **Hybrid + semantic ranker** is the safe default for RAG quality questions.
- **Integrated vectorization** is the answer when the question says "automatically embed during indexing".
- **Pro-mode analyzers** handle complex multi-step extraction; **single-task** for simple narrow output.
- Always think **chunk strategy + citations + provenance + freshness** when designing ingestion.

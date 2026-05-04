# Domain 1 — Plan and Manage an Azure AI Solution (25–30%)

> The single biggest domain by weight. It is **less about coding** and more about **picking the right Foundry primitive, deploying it correctly, securing it, and governing it**. Most AI-103 wrong-answer traps live here: choosing a standalone service when a Foundry Tool exists, using keys when managed identity is available, or forgetting evaluators / safety filters.

## Mind map

```mermaid
mindmap
  root((Plan and Manage))
    Choose Foundry services
      Model selection
        LLMs
        Small language models
        Multimodal
        Code models
      Foundry Tools
        Translator
        Content Understanding
        AI Search
        Custom functions
      Retrieval method
        Vector
        Hybrid
        Semantic
      Agent integrations
        Memory
        Tools
        Knowledge
    Set up in Foundry
      Foundry hub + project
      Connections
      Deployment
        Standard
        Provisioned
        Batch
        Containers
      CI CD pipelines
    Manage and monitor
      Quota and rate limits
      Cost
      Drift and grounding
      Safety events
      Index health
    Secure
      Managed identity
      Keyless credentials
      Private networking
      Entra ID + RBAC
      Customer managed keys
    Responsible AI
      Safety filters
      Risk detection
      Evaluators
      Trace logging
      Provenance
      Approval workflows
      Tool access control
```

## Choose the right Foundry primitive

```mermaid
flowchart TD
    A[Requirement] --> B{What is being produced?}
    B -- Text answer from a prompt --> C[LLM via Foundry model deployment]
    B -- Tools + multi step reasoning --> D[Foundry Agent Service]
    B -- Image generation --> E[Image generation model in Foundry]
    B -- Video generation --> F[Video generation model in Foundry]
    B -- Search over enterprise data --> G[Azure AI Search as a Foundry connection]
    B -- Doc layout / fields / markdown --> H[Content Understanding analyzer]
    B -- Translate text or speech --> I[Translator in Foundry Tools]
    B -- Speech in or out --> J[Azure AI Speech via Foundry]
    C --> K{Need grounding on private data?}
    K -- Yes --> L[RAG: AI Search + Content Understanding ingestion]
    K -- No --> M[Direct prompt with safety filters]
```

### Model selection cheat table

| Need | Pick |
| --- | --- |
| Long-form reasoning, complex tool use | **Large LLM** (frontier reasoning model) |
| High-volume, latency- and cost-sensitive task | **Small language model (SLM)** |
| Image + text input, "look at this and tell me…" | **Multimodal model** |
| Structured code generation / completion | **Code model** |
| Generate an image from a prompt | **Image generation model** |
| Generate a short clip from a prompt or storyboard | **Video generation model** |
| Convert documents into clean grounded markdown | **Content Understanding analyzer** (Foundry Tool) |
| Translate text, with terminology control | **Translator** (Foundry Tool) |
| Find relevant chunks from millions of docs | **Azure AI Search** (Foundry connection) |

> AI-103 trap: if the question mentions **chunks, ranking, embeddings, or "retrieval"**, the right answer is **AI Search**, not a model. If the question mentions **clean markdown, structured fields, layout, OCR + tables**, the right answer is **Content Understanding**, not Document Intelligence prebuilt models alone.

## Foundry deployment options

```mermaid
flowchart LR
    subgraph DEPLOY[Foundry deployment options]
      S[Standard - pay-per-call, shared capacity]
      P[Provisioned - reserved capacity, predictable latency]
      B[Batch - large volume, async, lower cost]
      C[Container / on-prem - data residency, disconnected]
    end
    USE[Workload requirement] --> DEPLOY
```

| Choose | When |
| --- | --- |
| **Standard** | Spiky usage, low or unknown traffic, prototypes |
| **Provisioned (PTU)** | Predictable QPS, strict latency SLOs, regulated workloads |
| **Batch** | Bulk eval, dataset labeling, offline summarization |
| **Containers** | Air-gapped, sovereign cloud, edge, strict residency |

## Set up Foundry — projects, hubs, connections

```mermaid
flowchart TD
    HUB[Foundry hub] --> PRJ1[Project: customer-support-agent]
    HUB --> PRJ2[Project: claims-extractor]
    PRJ1 --> CONN1[Connection: Azure AI Search]
    PRJ1 --> CONN2[Connection: Storage Account]
    PRJ1 --> CONN3[Connection: Application Insights]
    PRJ1 --> DEP[Model deployments]
    PRJ1 --> AGT[Agents]
    PRJ1 --> EVAL[Evaluations]
    HUB --> POL[Policies, networking, identity, CMK]
```

- **Hub** = shared resource: networking, policy, identity, CMK, default storage and AI Search.
- **Project** = workspace where deployments, connections, agents, evaluations, and traces live.
- **Connections** = how a project reaches data, search, telemetry, and tools without storing keys in code.
- **Connect an app to a project** using the Foundry SDK + a project endpoint — credentials come from `DefaultAzureCredential` / managed identity.

## CI/CD for Foundry projects

```mermaid
flowchart LR
    DEV[Dev project] --> EVAL[Batch + safety evals]
    EVAL --> APPROVE{Pass thresholds?}
    APPROVE -- No --> FAIL[Block release + open issue]
    APPROVE -- Yes --> STG[Stage project]
    STG --> CANARY[Canary traffic shift]
    CANARY --> PROD[Prod project]
    PROD --> MON[Continuous evals + drift monitor]
    MON --> EVAL
```

- Promote **agent definitions, prompt templates, model deployment names, and evaluation thresholds** as code.
- Treat evaluations as **release gates** (quality, groundedness, safety, latency, cost).
- Use Azure DevOps or GitHub Actions to call the Foundry SDK / CLI; store secrets in **Key Vault** referenced via managed identity.

## Manage quota, scale, rate limits, cost

| Lever | What to watch | Mitigation |
| --- | --- | --- |
| **TPM / RPM quota** | 429s on a deployment | Increase quota, split deployments, route to PTU |
| **PTU utilization** | Underuse vs over-spill | Right-size; spill to standard when above PTU |
| **Token cost** | $/conversation | Smaller model for routing, larger only when needed |
| **Tool latency** | Tail latency | Cache tool results, batch, async fan-out |
| **Index size** | Storage + query cost | Tier old chunks, prune, separate cold index |
| **Agent loop length** | Runaway tool calls | Step limit, budget guard, deterministic tool router |

## Monitor — quality, drift, safety, grounding

```mermaid
flowchart LR
    APP[Foundry app or agent] --> TRC[Tracing - prompt, tools, tokens, latency]
    TRC --> AI[Application Insights]
    APP --> EV[Evaluators - quality + safety + grounding]
    EV --> LA[Log Analytics]
    LA --> DASH[Azure Monitor workbook]
    AI --> DASH
    APP --> CS[Content Safety signals]
    CS --> DASH
    APP --> IDX[AI Search index telemetry]
    IDX --> DASH
```

Track: **groundedness, relevance, fluency, fabrication rate, safety blocks, latency P95, tokens in/out, tool error rate, index freshness**.

## Secure AI systems

| Control | Default answer on AI-103 |
| --- | --- |
| Authentication to Foundry | **Microsoft Entra ID + managed identity** (keyless) |
| Connection from app → Foundry project | **`DefaultAzureCredential` + project endpoint** |
| Connection from project → AI Search / Storage | **System-assigned managed identity + RBAC role** |
| Network exposure | **Private endpoints + disable public network access** |
| Secret storage | **Key Vault references**, never inline keys |
| Data at rest | **Customer-managed keys (CMK)** for regulated workloads |
| Tool access from agent | **Tool allowlist + per-tool RBAC + approval policy** |

> Trap: if a question shows an API key in the answer choices and another option uses managed identity, the managed identity option is almost always correct on AI-103.

## Responsible AI across generative + agentic systems

```mermaid
flowchart LR
    IN[User input] --> PF[Prompt shields - jailbreak + indirect injection]
    PF --> MOD[Content Safety filters]
    MOD --> LLM[Model / agent]
    LLM --> EVAL[Evaluators - groundedness, relevance, safety]
    EVAL --> OUT[Response]
    LLM --> TRC[Trace + provenance metadata]
    LLM --> APR{Sensitive action?}
    APR -- Yes --> H[Human approval workflow]
    APR -- No --> OUT
```

Required building blocks:

- **Safety filters** — Content Safety categories (hate, violence, sexual, self-harm) + severity thresholds.
- **Prompt shields** — block direct **jailbreaks** and **indirect prompt injection** (especially text embedded in images / docs).
- **Risk detection** — protected-material, groundedness, blocklists, custom policies.
- **Evaluators** — built-in (groundedness, relevance, fluency, coherence, safety) + custom.
- **Trace logging + provenance** — every tool call, retrieved chunk, citation, and decision recorded.
- **Approval workflows** — human-in-the-loop on destructive or external actions.
- **Tool-access control + oversight modes** — `ask`, `auto`, `deny`; allowlist of MCP / function tools.

## Domain summary

- **Foundry-first vocabulary** — projects, connections, Foundry Tools, Agent Service.
- **Keyless by default** — managed identity, Entra ID, Key Vault.
- **Right-size deployment** — Standard for spiky, PTU for predictable, Batch for volume, Containers for sovereign.
- **Treat evals + safety as gates**, not optional add-ons.
- **Observe everything** — tracing, evaluators, drift, index health, cost.

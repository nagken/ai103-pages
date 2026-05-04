# Microsoft Foundry Deep Dive

> A focused tour of the platform that powers AI-103. If a question feels like it could go many ways, the **Foundry-native primitive** is usually the right answer.

## Why Foundry is the center of AI-103

AI-102 had you stitching Azure OpenAI + AI Search + Bot Framework + prompt flow + Document Intelligence by hand. AI-103 expects you to compose those capabilities **inside a Foundry project**, with **Foundry Tools, connections, agents, evaluations, and tracing** as first-class objects.

```mermaid
flowchart LR
    DEV[Developer] --> SDK[Foundry SDK]
    SDK --> PRJ[Foundry project]
    PRJ --> DEPS[Model deployments]
    PRJ --> TOOLS[Foundry Tools]
    PRJ --> AGENTS[Agents]
    PRJ --> EVAL[Evaluations]
    PRJ --> TRACE[Traces]
    PRJ --> CONN[Connections]
    CONN --> SRCH[Azure AI Search]
    CONN --> STO[Azure Storage]
    CONN --> AIS[App Insights]
    CONN --> CUST[Custom function endpoint]
    PRJ --> HUB[Foundry hub]
    HUB --> NET[Networking + private endpoints]
    HUB --> ID[Managed identity + RBAC]
    HUB --> POL[Policy + CMK + audit]
```

## Hubs, projects, connections — what each owns

| Object | Owns |
| --- | --- |
| **Hub** | Networking, default storage + AI Search, identity, policy, customer-managed keys, audit settings |
| **Project** | Model deployments, agents, prompt assets, evaluations, datasets, traces, connections |
| **Connection** | A reusable pointer to an external resource (AI Search index, storage container, App Insights, MCP / custom tool endpoint) — credentials managed by Foundry, not in code |

## Foundry Tools — the first-class building blocks

| Tool | Purpose | Comes back as |
| --- | --- | --- |
| **AI Search** | Retrieval against an indexed knowledge base | Top-k chunks with citations |
| **Content Understanding** | Multimodal extraction → markdown / structured fields | Grounded text + JSON |
| **Translator** | Glossary-driven translation, language detection | Translated text |
| **Code interpreter** | Run code on uploaded files | Result + generated artifacts |
| **Custom function tool** | Your own API or Azure Function | Whatever your tool returns |
| **OpenAPI / API tool** | Wrap an existing REST API | Structured response |

> Trap: when a scenario maps to a Foundry Tool, picking a "raw" service call instead is usually the wrong AI-103 answer. The exam wants composition through the platform.

## Model classes in the Foundry catalog

```mermaid
mindmap
  root((Foundry models))
    LLMs
      Frontier reasoning
      General chat
    Small language models
      Cost / latency-sensitive
      Edge / containerized
    Multimodal
      Image + text in
      Audio in
    Code models
      Code completion
      Code reasoning
    Generation
      Image generation
      Video generation
    Embeddings
      Text
      Multimodal
```

## Foundry Agent Service — anatomy

```mermaid
flowchart TD
    A[Agent definition] --> ROLE[Role + goal + system instructions]
    A --> M[Model deployment]
    A --> T[Tools allowlist with JSON schemas]
    A --> K[Knowledge - AI Search / Content Understanding]
    A --> MEM[Memory - thread + long-term store]
    A --> SAFE[Safety - prompt shields, filters, blocklists]
    A --> POL[Autonomy mode + budgets]
    A --> EV[Evaluation hooks]
    A --> TR[Tracing]
```

Lifecycle:

1. **Define** the agent in a project (versioned).
2. **Test** with curated datasets via batch evaluations.
3. **Promote** through environments using CI/CD with eval gates.
4. **Monitor** with continuous evals + tracing in production.
5. **Iterate** — most regressions are fixed at the **prompt / tool / retrieval** layer before touching the model.

## Deployment options — when to pick what

```mermaid
flowchart TD
    REQ[Requirement] --> Q{Latency / volume / placement?}
    Q -- Spiky, low to mid traffic --> S[Standard]
    Q -- Predictable QPS + tight SLA --> P[Provisioned PTU]
    Q -- Massive offline jobs --> B[Batch]
    Q -- Sovereign / disconnected / edge --> C[Container]
```

| Lever | Standard | Provisioned (PTU) | Batch | Containers |
| --- | --- | --- | --- | --- |
| Capacity | Shared | Reserved | Async pool | Yours |
| Latency | Variable | Predictable | Hours | Local |
| Cost model | Per token | Reserved units | Lower per token | Infra cost |
| Use it for | Prototypes, spiky apps | Production chat / agents | Eval, summarization | Sovereign / disconnected |

## Connecting an app safely

```mermaid
sequenceDiagram
    participant App as App Service / Container Apps / Function
    participant MI as Managed Identity
    participant KV as Key Vault
    participant PRJ as Foundry Project
    participant DEP as Model deployment
    App->>MI: Acquire token (audience: Foundry)
    MI-->>App: Access token
    App->>KV: Resolve any non-AI secrets via Key Vault reference
    App->>PRJ: Call project endpoint with token
    PRJ->>DEP: Route to deployment
    DEP-->>App: Streamed response
```

Required posture for AI-103-style answers:

- **Managed identity** on the host (App Service, Container Apps, Function, AKS pod).
- **RBAC role** assigned on the Foundry project / hub.
- **`DefaultAzureCredential`** in code; no keys in config.
- **Private endpoint** + **disable public network access** for regulated workloads.
- **Customer-managed keys** when sovereignty / compliance requires it.

## Per-service "what shows up where in Foundry"

| Service | Foundry surface |
| --- | --- |
| Azure OpenAI models | Model catalog → deployments in a project |
| Azure AI Search | Connection → exposed as **AI Search Foundry Tool** |
| Content Understanding | Connection / built-in → **analyzers** as a Foundry Tool |
| Translator | Foundry Tool |
| Speech | SDK + Foundry connection for agent voice modality |
| Document Intelligence | Used inside a Content Understanding analyzer or a custom skill |
| Application Insights | Connection target for tracing |
| Storage | Connection for datasets, files, knowledge stores |

## Foundry CI/CD reference flow

```mermaid
flowchart LR
    CODE[Repo: agents.yaml + prompts + eval datasets] --> CI[GitHub Actions / Azure DevOps]
    CI --> DEV[Dev project deploy]
    DEV --> EVAL[Run evaluators against curated datasets]
    EVAL --> GATE{Pass thresholds?}
    GATE -- No --> FAIL[Block + open issue]
    GATE -- Yes --> STG[Stage project deploy]
    STG --> CANARY[Canary traffic]
    CANARY --> PROD[Prod project deploy]
    PROD --> CONT[Continuous evals + drift alerts]
    CONT --> CI
```

## Foundry deep-dive takeaways

- **Default to the Foundry-native primitive.**
- **Default to keyless auth and private networking.**
- **Default to evaluators as release gates.**
- **Default to multi-agent + tools + memory** when work decomposes.
- **Default to Content Understanding + AI Search** for grounding.
- **Default to tracing + continuous evals** for production.

If you internalize those defaults, most AI-103 wrong answers eliminate themselves.

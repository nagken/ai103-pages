# Azure Architecture Center

> The **[Azure Architecture Center](https://learn.microsoft.com/en-us/azure/architecture/)** is Microsoft's official catalog of reference architectures, design patterns, decision guides, and Well-Architected workload guidance. Browse the full catalog at [learn.microsoft.com/azure/architecture/browse](https://learn.microsoft.com/en-us/azure/architecture/browse/) and filter by product, category, or scenario.

## How to use it for AI-103

1. **Find a reference architecture close to your workload** in the browse catalog.
2. Read the **Well-Architected** review for that scenario.
3. Steal the **Bicep / Terraform** from the linked sample.
4. Adapt the **decision guides** (network, identity, data) to your constraints.

## Top entry points

| Resource | Why it matters for AI-103 |
| --- | --- |
| [Architecture Center home](https://learn.microsoft.com/en-us/azure/architecture/) | Curated landing page; start here. |
| [Browse architectures](https://learn.microsoft.com/en-us/azure/architecture/browse/) | Filterable catalog. Filter on **AI + Machine Learning** and **Azure OpenAI**. |
| [AI architecture design](https://learn.microsoft.com/en-us/azure/architecture/ai-ml/) | The AI/ML pillar — RAG, agents, evaluation, content safety. |
| [Cloud design patterns](https://learn.microsoft.com/en-us/azure/architecture/patterns/) | Retry, Circuit Breaker, CQRS, Saga, Throttling, Cache-Aside. |
| [Well-Architected for AI workloads](https://learn.microsoft.com/en-us/azure/well-architected/ai/) | Five pillars applied to AI. |

## Reference architectures most relevant to AI-103

| Architecture | What it shows |
| --- | --- |
| [Baseline OpenAI end-to-end chat reference](https://learn.microsoft.com/en-us/azure/architecture/ai-ml/architecture/baseline-openai-e2e-chat) | Production RAG: AOAI + AI Search + APIM + private endpoints + identity. |
| [AI Foundry chat baseline](https://learn.microsoft.com/en-us/azure/architecture/ai-ml/architecture/baseline-azure-ai-foundry-chat) | Foundry-native RAG with promptflow and managed online endpoint. |
| [Multi-agent custom AI app](https://learn.microsoft.com/en-us/azure/architecture/ai-ml/architecture/multi-agent-custom-ai-app) | Orchestrating multiple agents via Foundry Agent Service or AutoGen. |
| [Azure OpenAI fine-tuning architecture](https://learn.microsoft.com/en-us/azure/architecture/ai-ml/architecture/azure-openai-fine-tuning) | When to fine-tune, how to land it, evaluation gates. |
| [Azure AI Search performance tuning](https://learn.microsoft.com/en-us/azure/architecture/ai-ml/guide/azure-ai-search-performance-tuning-best-practices) | Hybrid + semantic ranker + scale tuning. |
| [Image processing with AI services](https://learn.microsoft.com/en-us/azure/architecture/ai-ml/idea/image-classification-with-vision-services) | Image Analysis vs Custom Vision decision. |
| [Knowledge mining with AI Search](https://learn.microsoft.com/en-us/azure/architecture/solution-ideas/articles/knowledge-mining-business-process-management) | End-to-end knowledge mining pipeline. |

## Decision guides worth bookmarking

- [Choose an AI service](https://learn.microsoft.com/en-us/azure/architecture/ai-ml/guide/choose-ai-services)
- [Choose a chat solution architecture](https://learn.microsoft.com/en-us/azure/architecture/ai-ml/guide/choose-chat-architecture)
- [Designing a RAG solution](https://learn.microsoft.com/en-us/azure/architecture/ai-ml/guide/rag/rag-solution-design-and-evaluation-guide)
- [Choose a data store](https://learn.microsoft.com/en-us/azure/architecture/guide/technology-choices/data-store-decision-tree)

## Patterns you should know for the exam

| Pattern | When to apply |
| --- | --- |
| **Retry / Circuit Breaker** | Calls into AOAI / AI Search to absorb transient throttling. |
| **Throttling / Rate Limiting** | Per-tenant quota over a shared model deployment (often via APIM). |
| **Cache-Aside** | Cache embeddings or semantic-cache responses to reduce token cost. |
| **Sidecar / Ambassador** | Inject content safety + telemetry around model calls. |
| **Saga** | Multi-step agent orchestration with compensations. |
| **Backends for Frontends (BFF)** | UI calls a thin API; API calls AOAI with managed identity. |

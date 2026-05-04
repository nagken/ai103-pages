# Common Pitfalls and Distractor Patterns

> Mistakes that look right on the exam but lose points. Each entry pairs the wrong choice most candidates pick with the correct one and the rule that distinguishes them.

## Retrieval and Search

### "Use the semantic ranker" when the question is really about query type

**Pitfall**: Selecting *semantic ranker* whenever a question mentions "improve search relevance."

**Reality**: Semantic ranker reorders the top 50 results from an existing query. It does not change *which* documents are retrieved. If exact-match terms are missing from results, you need **hybrid search** (BM25 + vector with RRF), not semantic ranker.

**Rule**: Semantic ranker is an L2 reranker. It assumes L1 retrieval is good. Fix L1 first.

### Confusing Azure AI Search semantic ranker with Semantic Kernel

**Pitfall**: Treating *semantic kernel* as a search feature.

**Reality**: **Semantic Kernel** is an open-source SDK for orchestrating prompts, plugins, and planners — unrelated to Azure AI Search. **Semantic ranker** is a paid Azure AI Search feature.

### Over-chunking long structured documents

**Pitfall**: Splitting a 200-page manual into 256-token chunks for "better recall."

**Reality**: Loses cross-section context. Use **parent-child chunking**: index small chunks for retrieval, return the parent section to the model.

## Generative AI and Agents

### Using API keys instead of managed identity for service-to-service calls

**Pitfall**: Storing the Azure OpenAI API key in App Service configuration.

**Reality**: Failing the security pillar of the question. Production agents use **managed identity** through Foundry connections. Keys are local-development only.

### "Just raise the temperature" to fix hallucinations

**Pitfall**: Adjusting temperature when answers are wrong.

**Reality**: Hallucinations are a *grounding* problem. Lower temperature reduces creativity but does not introduce facts. Add **RAG** with **groundedness detection**, or pin retrieval to authoritative sources.

### Choosing fine-tuning over RAG for "more accurate answers"

**Pitfall**: Recommending fine-tune when the source data changes weekly.

**Reality**: Fine-tuning bakes data into weights — stale within a release cycle. **RAG** is correct for evolving knowledge. Fine-tune only for tone, format, or latency-critical small models.

### Confusing PTUs with capacity reservations

**Pitfall**: Treating PTUs like a discount.

**Reality**: **PTUs** are reserved Azure OpenAI throughput with predictable latency. They are billed continuously regardless of utilization. Choose only when you have sustained load and SLA requirements.

## Responsible AI and Content Safety

### Prompt Shields direct vs indirect

**Pitfall**: Enabling only direct attack detection on a RAG agent.

**Reality**: Direct catches user-typed jailbreaks. **Indirect** catches adversarial instructions smuggled in retrieved documents (the more dangerous case in RAG). Always enable both for RAG workloads.

### Blocklist as the only content filter

**Pitfall**: Adding regex patterns to a blocklist instead of using severity-based filters.

**Reality**: Blocklists are for **specific known phrases**. Severity filters (hate, sexual, self-harm, violence) are the primary line of defense and adapt to paraphrase.

### Putting groundedness check in front of the model

**Pitfall**: Calling groundedness on the user's question.

**Reality**: Groundedness checks the **answer** against the **sources** — it runs after generation, not before.

## Identity and Networking

### Public endpoint Foundry "for development speed"

**Pitfall**: Leaving public network access on a production project to avoid private DNS setup.

**Reality**: WAF security pillar rejects this. Production: **private endpoint** + VNet hub + disabled public access. Use a separate dev project if developers need easier access.

### Forgetting the indexer's identity

**Pitfall**: Granting the search *service* identity Storage Blob Data Reader, but the indexer still fails.

**Reality**: Confirm the indexer references the system or user-assigned identity in its data-source definition. Permissions on the principal alone aren't enough.

## Evaluation and Operations

### Confusing relevance with groundedness

**Pitfall**: Reporting "relevance is high, the agent is good."

**Reality**: A response can be on-topic and still hallucinated. Pair **relevance** with **groundedness** for RAG. Pair relevance with **similarity** to gold answers when you have labeled data.

### Logging prompts at INFO level into Application Insights

**Pitfall**: Treating prompts and completions like normal app logs.

**Reality**: Prompts can contain PII and prompt-injection payloads. Use **Foundry tracing** (which redacts on export) or scrub before sending. Configure data residency and retention policies on the Application Insights workspace.

### "Just route to GPT-5 for everything"

**Pitfall**: Using the largest model regardless of task.

**Reality**: WAF cost pillar fails. Use the **smallest model that meets quality bar** per evaluator. Pair larger models with caching (semantic cache via APIM) and routing tiers.

## Foundry-Specific Gotchas

### Treating Foundry projects as throwaway

**Pitfall**: Creating a new project per feature.

**Reality**: Connections, tracing, evaluations, and deployments are project-scoped. **One project per workload**, multiple environments via deployment slots or separate environments. Use a hub when projects need to share network/identity.

### Mixing agent SDKs in the same workload

**Pitfall**: Some endpoints use the OpenAI Assistants SDK, others use the new Foundry Agents SDK.

**Reality**: Pick one. Foundry Agent Service is the forward path; Assistants is being aligned. Mixed SDKs fragment tracing and evaluation.

### Forgetting tool-call evaluation

**Pitfall**: Evaluating only the final response.

**Reality**: For tool-using agents, evaluate **tool selection accuracy** and **argument correctness** as separate scores. The final response can be right by accident.

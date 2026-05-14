# Microsoft Learn Summaries

> Tight summaries of every major service covered in this AI-103 guide, organized the Microsoft Learn way: **Overview -> Components -> Key concepts -> Integrations**. Use this page as a 60-second refresher before diving into the domain pages.

## Microsoft Foundry (the AI-103 hub)

- **Overview** - Foundry is the unified portal/SDK that unifies what used to live across separate Azure AI portals: model catalog, project workspaces, agents, evaluations, observability, and content safety.
- **Components** - Hub, Project, Model Catalog, Agents, Evaluations, Tracing, Content Safety, Connected Resources.
- **Key concepts** - Hub-Project hierarchy; project-scoped connections; managed identity for outbound calls; evaluations as first-class artifacts.
- **Integrations** - Azure OpenAI, Azure AI Search, Azure AI Document Intelligence, Azure AI Speech, Azure AI Vision, Azure AI Language, Application Insights for tracing.
- Source: [Foundry overview on Microsoft Learn](https://learn.microsoft.com/azure/ai-foundry/what-is-azure-ai-foundry).

## Azure OpenAI (in Foundry)

- **Overview** - Provisioned access to OpenAI models (GPT-4o, GPT-4.1, o-series, embeddings, DALL-E, Whisper) with Azure security, networking, and identity.
- **Components** - Resource, Deployment, Quota, Content Filter, Assistants/Agents API.
- **Key concepts** - Standard vs Provisioned-Throughput Units (PTU); regional availability; data residency; system + user message roles; tools/function calling.
- **Integrations** - Foundry Agents, AI Search (vector), App Insights, Azure RBAC.
- Source: [Azure OpenAI service](https://learn.microsoft.com/azure/ai-services/openai/overview).

## Azure AI Search (vector + hybrid)

- **Overview** - Managed search index with full-text, vector, hybrid, and semantic ranking. Backbone of RAG patterns in Foundry.
- **Components** - Index, Indexer, Skillset, Data source, Vectorizer, Semantic configuration.
- **Key concepts** - Push vs pull indexing; integrated vectorization; chunking + overlap; HNSW vs exhaustive KNN; reranker.
- **Integrations** - Azure OpenAI (embeddings), Foundry RAG, Storage Blob, Cosmos DB, SQL DB.
- Source: [Azure AI Search](https://learn.microsoft.com/azure/search/search-what-is-azure-search).

## Azure AI Document Intelligence

- **Overview** - Pre-built and custom models that extract text, tables, key-value pairs, and structured fields from documents.
- **Components** - Layout, Read, Prebuilt (invoice/receipt/ID/W-2/...), Custom Extraction, Custom Classification.
- **Key concepts** - Document analysis vs custom training; sample size for custom models; confidence scores; Studio for labeling.
- **Integrations** - Foundry agents (file tools), Logic Apps, Functions, Azure Storage.
- Source: [Document Intelligence](https://learn.microsoft.com/azure/ai-services/document-intelligence/).

## Azure AI Speech

- **Overview** - Speech-to-text, text-to-speech, speaker recognition, translation, and custom voice.
- **Components** - STT (real-time + batch), TTS (neural voices), Custom Speech, Custom Voice, Speech Translation.
- **Key concepts** - Endpoint deployment for custom models; SSML for prosody; phrase lists for STT bias; pronunciation assessment.
- **Integrations** - Foundry agents (voice), Live Translation, Communication Services.
- Source: [Azure AI Speech](https://learn.microsoft.com/azure/ai-services/speech-service/).

## Azure AI Language

- **Overview** - NLU pipeline: PII redaction, entity recognition, sentiment, summarization, classification, CLU, custom QA.
- **Components** - Prebuilt features (NER, sentiment, key phrases, language detection), Custom NER, CLU, Custom Text Classification, Custom QA.
- **Key concepts** - Project + deployment lifecycle; multi-label vs single-label classification; intent + entity model in CLU.
- **Integrations** - Foundry agents, Power Automate, Logic Apps.
- Source: [Azure AI Language](https://learn.microsoft.com/azure/ai-services/language-service/).

## Azure AI Vision

- **Overview** - Image analysis, OCR (Read), object/face detection, spatial analysis, video indexer.
- **Components** - Image Analysis 4.0 (Florence), Read OCR, Face, Custom Vision (classify + detect), Video Indexer.
- **Key concepts** - Florence-based foundation model; tags + captions + dense captions + objects; bounding boxes; Custom Vision quick training.
- **Integrations** - Document Intelligence, Foundry agents (multimodal), Storage.
- Source: [Azure AI Vision](https://learn.microsoft.com/azure/ai-services/computer-vision/).

## Content Safety

- **Overview** - Standalone moderation API used directly or as a built-in filter on Azure OpenAI deployments.
- **Components** - Text moderation, Image moderation, Prompt Shields, Groundedness detection, Protected material detection.
- **Key concepts** - Severity levels (Safe / Low / Medium / High); custom blocklists; jailbreak detection; XPIA detection.
- **Integrations** - Azure OpenAI content filters, Foundry guardrails, Logic Apps.
- Source: [Content Safety](https://learn.microsoft.com/azure/ai-services/content-safety/overview).

## Application Insights (for AI tracing)

- **Overview** - APM telemetry store. In Foundry, every agent + chat invocation can emit OpenTelemetry traces and prompt/response logs.
- **Components** - Workspace-based App Insights, OTel exporter, Foundry Tracing UI.
- **Key concepts** - Correlated end-to-end traces; sampling; KQL across `requests`, `dependencies`, `traces`.
- **Integrations** - Foundry Project, Azure Monitor.
- Source: [Foundry observability](https://learn.microsoft.com/azure/ai-foundry/concepts/trace).

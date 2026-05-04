# AI-103 — Reference Architectures

> Real architectures from the [Azure Architecture Center](https://learn.microsoft.com/azure/architecture/browse/) that map directly to AI-103 exam topics. Each entry calls out which **skills-measured area** it reinforces.

## Domain 1 — Plan and Manage Azure AI Solution (25–30%)

| Architecture | Topic it reinforces |
| --- | --- |
| [Baseline Microsoft Foundry chat reference architecture](https://learn.microsoft.com/azure/architecture/ai-ml/architecture/baseline-microsoft-foundry-chat) | Foundry resource + projects, Agent Service, deployments, identity |
| [Baseline Microsoft Foundry chat in an Azure landing zone](https://learn.microsoft.com/azure/architecture/ai-ml/architecture/baseline-microsoft-foundry-landing-zone) | Private endpoints, managed identity, CMK, policy guardrails, hub-spoke |
| [AI and ML in multitenant solutions](https://learn.microsoft.com/azure/architecture/guide/multitenant/approaches/ai-and-ml) | Tenant isolation, PTU vs Standard, cost attribution |
| [Advanced monitoring for Azure OpenAI through a gateway](https://learn.microsoft.com/azure/architecture/ai-ml/guide/azure-openai-gateway-monitoring) | Tracing, App Insights, token analytics, chargeback via APIM |
| [MLOps technical paper](https://learn.microsoft.com/azure/architecture/ai-ml/guide/mlops-technical-paper) | CI/CD with eval gates, reused for GenAIOps |

## Domain 2 — Implement Generative AI + Agentic Solutions (30–35%)

| Architecture | Topic it reinforces |
| --- | --- |
| [Baseline OpenAI end-to-end chat](https://learn.microsoft.com/azure/architecture/ai-ml/architecture/baseline-openai-e2e-chat) | RAG chat with App Service / Container Apps + AI Search + Key Vault |
| [Baseline Microsoft Foundry chat (Agent Service)](https://learn.microsoft.com/azure/architecture/ai-ml/architecture/baseline-microsoft-foundry-chat) | Tools, memory, tracing, content safety, autonomy via Foundry Agent Service |
| [AI agent orchestration patterns](https://learn.microsoft.com/azure/architecture/ai-ml/guide/ai-agent-design-patterns) | Single vs multi-agent, A2A protocol, MCP tools |
| [Build language model pipelines with memory](https://learn.microsoft.com/azure/architecture/ai-ml/openai/architecture/openai-language-model-memory) | Conversation state, function calling |
| [Extract and analyze call center data](https://learn.microsoft.com/azure/architecture/ai-ml/openai/architecture/call-center-openai-analytics) | Foundry Tools, summarization, evaluation |

## Domain 3 — Implement Computer Vision Solutions (10–15%)

| Architecture | Topic it reinforces |
| --- | --- |
| [Vision AI solutions with Azure](https://learn.microsoft.com/azure/architecture/ai-ml/architecture/vision-ai-solutions) | Where multimodal models + Content Understanding fit |
| [Image classification on Azure](https://learn.microsoft.com/azure/architecture/ai-ml/architecture/intelligent-apps-image-processing) | Vision pipeline as contrast against generation |
| [Visual search](https://learn.microsoft.com/azure/architecture/solution-ideas/articles/visual-search) | Embeddings + vector search for images |

## Domain 4 — Implement Text Analysis + Speech Solutions (10–15%)

| Architecture | Topic it reinforces |
| --- | --- |
| [Speech-to-text transcription pipeline](https://learn.microsoft.com/azure/architecture/ai-ml/architecture/speech-to-text-transcription-pipeline) | STT + custom speech for domain accuracy |
| [Conversational AI customer service](https://learn.microsoft.com/azure/architecture/ai-ml/architecture/conversational-ai) | Voice-in / voice-out agent pattern |
| [Suggest content tags with NLP](https://learn.microsoft.com/azure/architecture/ai-ml/architecture/website-content-tagging-keyphrase) | Entity / key phrase extraction |

## Domain 5 — Implement Information Extraction Solutions (10–15%)

| Architecture | Topic it reinforces |
| --- | --- |
| [Knowledge mining with Azure AI Search](https://learn.microsoft.com/azure/architecture/solution-ideas/articles/ai-search-skillsets) | Skillsets, enrichment, hybrid + semantic search |
| [Knowledge mining business search engine](https://learn.microsoft.com/azure/architecture/solution-ideas/articles/business-process-search-engine) | Custom skills, integrated vectorization |
| [Automate document processing with AI Document Intelligence](https://learn.microsoft.com/azure/architecture/example-scenario/ai/automate-document-processing-azure-form-recognizer) | Document Intelligence → grounded data (Content Understanding evolution) |

## How to study with these

1. Read the matching domain page in this guide first.
2. Open the architecture; trace **identity → networking → data → AI service → observability**.
3. Re-answer the [exam decision reference](06-exam-cheatsheet.md) with that architecture as the worked example.

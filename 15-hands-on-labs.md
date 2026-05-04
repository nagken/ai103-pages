# Hands-On Labs and Sample Repositories

> Curated, executable references for AI-103 topics. All links point to official Microsoft Learn modules, Azure-Samples repositories, or Foundry quickstarts — no third-party content.

## Microsoft Learn — Sandbox-Backed Modules

| Module | What you build |
| --- | --- |
| [Get started with Azure OpenAI Service](https://learn.microsoft.com/training/modules/get-started-openai/) | Deploy a model, run completions and chat in the playground, call from code. |
| [Build a RAG-based generative AI app with Azure AI Foundry](https://learn.microsoft.com/training/modules/build-copilot-ai-studio/) | End-to-end RAG over your data using a Foundry project, AI Search, and Azure OpenAI. |
| [Develop generative AI apps with Azure OpenAI and the Azure AI Foundry SDK](https://learn.microsoft.com/training/modules/ai-foundry-sdk/) | Use the Foundry SDK to manage projects, connections, models, and prompts in code. |
| [Build agents with Azure AI Agent Service](https://learn.microsoft.com/training/modules/ai-agent-fundamentals/) | Define agents, tools, threads, runs; observe traces. |
| [Implement Azure AI Content Safety](https://learn.microsoft.com/training/modules/moderate-content-detect-harm-azure-ai-content-safety/) | Severity scoring, blocklists, prompt shields, groundedness. |
| [Implement vector search with Azure AI Search](https://learn.microsoft.com/training/modules/improve-search-results-vector-search/) | Build a vector index, embed content, query with hybrid + semantic ranker. |
| [Develop solutions with Azure AI Document Intelligence](https://learn.microsoft.com/training/paths/extract-information-from-text-with-azure-ai-services/) | Prebuilt + custom models, layout analysis, programmatic extraction. |
| [Develop conversational agents with Azure AI Speech](https://learn.microsoft.com/training/modules/recognize-translate-speech/) | Real-time speech-to-text, voice synthesis for agents. |

## Azure-Samples Reference Repositories

| Repository | Purpose |
| --- | --- |
| [Azure-Samples/azure-search-openai-demo](https://github.com/Azure-Samples/azure-search-openai-demo) | Reference RAG implementation: chat over your own data with AI Search + Azure OpenAI. The single most-cited starting point. |
| [Azure-Samples/AI-Gateway](https://github.com/Azure-Samples/AI-Gateway) | API Management as an AI gateway: token limits, semantic caching, multi-region routing, content safety policy fragments. |
| [Azure-Samples/contoso-creative-writer](https://github.com/Azure-Samples/contoso-creative-writer) | Multi-agent Foundry application with evaluation pipelines and tracing. |
| [Azure-Samples/contoso-chat](https://github.com/Azure-Samples/contoso-chat) | RAG retail chat with prompt flow, evaluations, and CI/CD via azd. |
| [Azure-Samples/get-started-with-ai-agents](https://github.com/Azure-Samples/get-started-with-ai-agents) | Minimal Foundry agent template you can deploy with `azd up`. |
| [Azure-Samples/openai](https://github.com/Azure-Samples/openai) | Catalog of Azure OpenAI samples: chat completions, embeddings, fine-tuning, function calling, image generation. |
| [Azure-Samples/azure-search-vector-samples](https://github.com/Azure-Samples/azure-search-vector-samples) | Vector and hybrid search examples in Python, C#, JavaScript. |
| [Azure-Samples/cognitive-services-content-safety-samples](https://github.com/Azure-Samples/Content-Safety-Samples) | Prompt Shields, severity, blocklists, groundedness. |

## Foundry Quickstarts

| Quickstart | Outcome |
| --- | --- |
| [Create a Foundry project](https://learn.microsoft.com/azure/ai-foundry/how-to/create-projects) | Provision a project, hub, and connections. |
| [Deploy your first agent](https://learn.microsoft.com/azure/ai-foundry/agents/quickstart) | First agent run with file search and code interpreter. |
| [Add Azure AI Search as a tool](https://learn.microsoft.com/azure/ai-foundry/agents/how-to/tools/azure-ai-search) | Connect an existing index as a retrieval tool. |
| [Trace agent runs](https://learn.microsoft.com/azure/ai-foundry/how-to/develop/trace-application) | Wire OpenTelemetry tracing into Application Insights. |
| [Run evaluations](https://learn.microsoft.com/azure/ai-foundry/how-to/develop/evaluate-sdk) | Built-in groundedness, relevance, retrieval evaluators against a dataset. |

## Azure Developer CLI Templates

| Command | Template |
| --- | --- |
| `azd init -t azure-search-openai-demo` | Full RAG reference architecture, deployable in one command. |
| `azd init -t get-started-with-ai-agents` | Foundry agents template. |
| `azd init -t contoso-chat` | RAG with prompt flow + evaluations + GitHub Actions. |

> Tip: `azd template list --source awesome-azd` returns the official template catalog. Filter for `ai-` templates to see the AI-specific set.

## Practice Datasets

| Dataset | Use it for |
| --- | --- |
| [HotpotQA](https://learn.microsoft.com/azure/ai-foundry/concepts/evaluation-evaluators/built-in-evaluators) | Multi-hop reasoning evaluations. |
| [MS MARCO passages](https://microsoft.github.io/msmarco/) | Retrieval benchmarking for vector and hybrid search. |
| Microsoft sample policy / HR / product manuals (in `azure-search-openai-demo`) | RAG correctness practice. |

## Recommended Practice Path

1. Deploy `azure-search-openai-demo` with `azd up` and observe the full retrieval+generation flow.
2. Replace the sample data with your own documents; tune chunking and analyze relevance.
3. Add Content Safety prompt shields and groundedness detection.
4. Wire a Foundry agent that calls the same AI Search index as a tool.
5. Add evaluations with the built-in retrieval, groundedness, and relevance evaluators.
6. Front the agent with API Management using the AI-Gateway template.

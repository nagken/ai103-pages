# Glossary and Acronym Reference

> Authoritative, exam-focused definitions for the terms, acronyms, and product names that appear in AI-103 scenarios. Use this page as a quick lookup; concept pages remain the place for deeper context.

## Core Microsoft Foundry Terms

| Term | Definition |
| --- | --- |
| **Microsoft Foundry** | Unified Azure platform for building, evaluating, and operating generative AI applications and agents. Successor surface to Azure AI Studio. |
| **Foundry project** | The deployment unit that groups model deployments, agents, connections, evaluations, and tracing for a workload. |
| **Foundry hub** | Optional shared resource that centralizes networking, identity, storage, and connections across multiple projects. |
| **Agent Service** | Managed runtime that hosts agent definitions, threads, runs, and tool invocations behind a stable REST API. |
| **Agent thread** | Persistent conversation state object that stores messages, tool calls, and run history for one user session. |
| **Run** | One execution of an agent over a thread; produces tool calls, model outputs, and observable telemetry. |
| **Connected agent** | An agent referenced as a tool inside another agent (multi-agent orchestration pattern). |
| **Connection** | A typed credential reference (Azure OpenAI, AI Search, Storage, custom REST) that an agent or project uses without embedding secrets. |
| **Tool** | A capability an agent can invoke: built-in (file search, code interpreter, browser), Azure-hosted (AI Search, Azure Functions), or custom (OpenAPI, MCP). |

## Generative AI and Retrieval

| Term | Definition |
| --- | --- |
| **RAG** | Retrieval-Augmented Generation — retrieve grounding context from an index, inject into the prompt, then generate. |
| **Vector search** | Similarity search using embeddings (cosine, dot product, Euclidean) over a vector field. |
| **Hybrid search** | Combines BM25 keyword scoring with vector similarity using Reciprocal Rank Fusion (RRF). |
| **Semantic ranker** | Azure AI Search L2 reranker that uses a Microsoft-trained model to rescore the top results from BM25 or hybrid. |
| **Embedding** | A dense numeric vector representing text or image semantics; produced by models such as `text-embedding-3-large`. |
| **Chunking** | Splitting source documents into retrievable units; common strategies are fixed-size, semantic, and parent-child. |
| **Grounding** | The practice of constraining model answers to retrieved context to reduce hallucination. |
| **Groundedness detection** | Azure AI Content Safety capability that scores whether a response is supported by provided sources. |
| **Indexer** | Azure AI Search component that ingests data from a source, optionally enriches it, and writes to an index. |
| **Skillset** | Pipeline of cognitive skills applied during indexing (OCR, entity recognition, embedding, custom). |

## Prompting, Models, and Evaluation

| Term | Definition |
| --- | --- |
| **System prompt** | Top-level instruction that defines an agent's role, constraints, and output format. |
| **Few-shot prompting** | Including a small number of input/output examples in the prompt to steer behavior. |
| **Tool calling** | Model-emitted function invocations that the runtime executes and feeds back as observations. |
| **Token** | The model's basic input/output unit; pricing, rate limits, and context windows are measured in tokens. |
| **Context window** | Maximum tokens (input + output) the model can process in a single call. |
| **Temperature** | Sampling parameter; lower values produce deterministic output, higher values produce diverse output. |
| **Top-p (nucleus sampling)** | Restricts sampling to tokens whose cumulative probability reaches `p`. |
| **PTU** | Provisioned Throughput Unit — reserved Azure OpenAI capacity that provides predictable latency and throughput. |
| **Evaluation** | Programmatic scoring of model or agent outputs against datasets using built-in or custom evaluators. |
| **Built-in evaluator** | Microsoft-provided scorers: relevance, groundedness, coherence, fluency, similarity, retrieval, content safety. |

## Responsible AI and Content Safety

| Term | Definition |
| --- | --- |
| **Prompt Shields** | Azure AI Content Safety feature detecting jailbreak attempts and indirect prompt injection. |
| **Indirect prompt injection** | Attack where adversarial instructions are smuggled through retrieved content (RAG, email, web). |
| **Protected material detection** | Content Safety capability that flags copyrighted text or code in model output. |
| **Custom blocklist** | Project-specific text patterns that Content Safety blocks regardless of severity. |
| **Severity level** | 0/2/4/6 scale across hate, sexual, self-harm, violence categories used by Content Safety. |
| **Responsible AI Standard** | Microsoft's internal governance framework covering accountability, transparency, fairness, reliability, privacy, inclusiveness. |

## Identity, Networking, and Operations

| Term | Definition |
| --- | --- |
| **Managed identity** | Azure-issued service identity (system or user assigned) that authenticates without secrets. |
| **Entra ID** | Microsoft's identity platform (formerly Azure AD); issues tokens for users, apps, and managed identities. |
| **Private endpoint** | Network interface in your VNet that exposes an Azure service over a private IP. |
| **Private Link** | The umbrella service that powers private endpoints. |
| **API Management (APIM)** | Azure gateway used in front of AI services for token rate limits, semantic caching, content filtering, and routing. |
| **Application Insights** | Application performance monitoring service; the telemetry sink for Foundry tracing. |
| **Tracing** | OpenTelemetry-based instrumentation that captures spans for prompts, tool calls, and runs. |

## Adjacent Azure AI Services

| Term | Definition |
| --- | --- |
| **Azure AI Search** | Vector + keyword + semantic search service used as the retrieval backbone for RAG. |
| **Azure AI Content Understanding** | Multimodal extraction service (documents, audio, video, image) producing structured fields and grounded summaries. |
| **Azure AI Document Intelligence** | OCR and layout extraction service with prebuilt and custom models. |
| **Azure AI Speech** | Speech-to-text, text-to-speech, real-time conversation, and translation. |
| **Azure AI Vision** | Image analysis, OCR, video summarization, and image generation. |
| **Azure AI Language** | Entity, sentiment, key-phrase, summarization, and conversational language understanding. |
| **Translator** | Document and real-time text translation. |

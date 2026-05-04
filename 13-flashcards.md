# Flashcards: Active Recall

> Click any card to reveal the answer. Use the **Domain pager bottom-right** to switch between exam areas. ~50 cards across 6 domains.

<section class="fc-section" data-fc-title="Foundry & Project Setup">
<h2>1 · Foundry & Project Setup</h2>

<div class="flashcard-grid">

<div class="flashcard"><div class="fc-q">Foundry hub vs project — what does each own?</div><div class="fc-a"><strong>Hub</strong> = shared resources (storage, key vault, ACR, connections, compute). <strong>Project</strong> = workspace where agents/flows/evals live, inherits hub connections.</div></div>

<div class="flashcard"><div class="fc-q">When use a Foundry "connection" vs deploying a separate resource?</div><div class="fc-a">Connection = reuse existing AOAI/Search/Storage. Avoids duplicate cost; centralizes credentials & RBAC.</div></div>

<div class="flashcard"><div class="fc-q">What identity should Foundry agents use to call AOAI/Search?</div><div class="fc-a"><strong>Managed identity on the hub/project</strong>. Grant <code>Cognitive Services User</code> / <code>Search Index Data Reader</code> on target.</div></div>

<div class="flashcard"><div class="fc-q">Foundry Models catalog — base vs fine-tuned vs serverless?</div><div class="fc-a">Base = MaaS deployment. Fine-tuned = customized weights. Serverless API = pay-per-token via MaaS endpoint, no quota mgmt.</div></div>

<div class="flashcard"><div class="fc-q">PTU vs PaYG (pay-as-you-go) deployment?</div><div class="fc-a">PTU = reserved throughput, predictable latency, monthly commit. PaYG = per-token, bursty, no commitment.</div></div>

<div class="flashcard"><div class="fc-q">Where do Foundry runs/traces/evals get stored?</div><div class="fc-a">Project linked <strong>storage</strong> + <strong>Application Insights</strong>. Tracing uses OpenTelemetry.</div></div>

<div class="flashcard"><div class="fc-q">Quick way to lock down a Foundry hub network?</div><div class="fc-a">Managed VNet + private endpoints on storage/keyvault/AOAI/Search; disable public network access on hub.</div></div>

<div class="flashcard"><div class="fc-q">Foundry vs Azure ML workspace — main difference?</div><div class="fc-a">Foundry = generative-AI-first (agents, prompt flow, model catalog). Azure ML = classical ML (training, MLflow, pipelines).</div></div>

</div>
</section>

<section class="fc-section" data-fc-title="Generative AI Models & Deployment">
<h2>2 · Generative AI Models & Deployment</h2>

<div class="flashcard-grid">

<div class="flashcard"><div class="fc-q">When choose RAG vs fine-tuning?</div><div class="fc-a">RAG: dynamic/proprietary knowledge, citations, low cost. Fine-tune: tone/format/structured output, narrow domain.</div></div>

<div class="flashcard"><div class="fc-q">Lowest-latency way to add company tone?</div><div class="fc-a"><strong>System prompt + few-shot examples</strong>. Reach for fine-tuning only when prompting plateaus.</div></div>

<div class="flashcard"><div class="fc-q">temperature vs top_p — which to vary?</div><div class="fc-a">Vary <strong>one, not both</strong>. Temperature = randomness, top_p = nucleus sampling. Default temp ≈ 0.7 for chat, 0 for deterministic.</div></div>

<div class="flashcard"><div class="fc-q">Reduce prompt cost on long chats?</div><div class="fc-a">Conversation summarization, prompt caching, shorter system prompt, smaller model for routing.</div></div>

<div class="flashcard"><div class="fc-q">Embeddings model dimensions matter how?</div><div class="fc-a">Higher = better recall, more storage/compute. <code>text-embedding-3-large</code> supports <strong>dimension truncation</strong>.</div></div>

<div class="flashcard"><div class="fc-q">Deploy an open-source Llama in Foundry — which option?</div><div class="fc-a"><strong>Serverless API (MaaS)</strong> for pay-per-token, or <strong>Managed Compute</strong> for dedicated GPU.</div></div>

<div class="flashcard"><div class="fc-q">Content filter levels in AOAI?</div><div class="fc-a">Hate, sexual, self-harm, violence — Off/Low/Med/High each. Plus jailbreak (Prompt Shields) + protected material.</div></div>

<div class="flashcard"><div class="fc-q">Stream tokens to UI — what do you set?</div><div class="fc-a"><code>stream=True</code> on the chat completion call; client iterates SSE chunks. Reduces perceived latency.</div></div>

</div>
</section>

<section class="fc-section" data-fc-title="Agents & Tool Calling">
<h2>3 · Agents & Tool Calling</h2>

<div class="flashcard-grid">

<div class="flashcard"><div class="fc-q">Foundry Agent Service — built-in tools?</div><div class="fc-a">File search, code interpreter, function calling, OpenAPI tools, Bing grounding, Azure AI Search, Logic Apps, Fabric data agent, connected agents.</div></div>

<div class="flashcard"><div class="fc-q">Connected agent vs handoff?</div><div class="fc-a">Connected agent = invoke another agent as a tool (orchestrator pattern). Handoff transfers conversation control.</div></div>

<div class="flashcard"><div class="fc-q">Add a private REST API as an agent tool?</div><div class="fc-a"><strong>OpenAPI 3.0 spec → tool</strong> in Foundry, with managed-identity or API-key connection.</div></div>

<div class="flashcard"><div class="fc-q">Agent SDK — thread vs run vs message?</div><div class="fc-a">Thread = conversation. Messages append to thread. Run = one execution of an agent on a thread.</div></div>

<div class="flashcard"><div class="fc-q">Stop an agent calling a tool in an infinite loop?</div><div class="fc-a">Set <strong>max steps / tool call limit</strong> on run; add tool-choice constraints; review traces.</div></div>

<div class="flashcard"><div class="fc-q">Make an agent grounded only in your docs?</div><div class="fc-a">Attach an <strong>Azure AI Search</strong> (or File Search) tool; system prompt refusing outside-source answers; groundedness eval.</div></div>

<div class="flashcard"><div class="fc-q">Multi-agent orchestration patterns?</div><div class="fc-a">Sequential, concurrent, group chat, handoff, magentic. Use <em>Microsoft Agent Framework</em>.</div></div>

<div class="flashcard"><div class="fc-q">Persist user-specific agent memory?</div><div class="fc-a">Thread per user + custom store (Cosmos DB) for long-term; pass summary back into system prompt.</div></div>

</div>
</section>

<section class="fc-section" data-fc-title="RAG & AI Search">
<h2>4 · RAG & AI Search</h2>

<div class="flashcard-grid">

<div class="flashcard"><div class="fc-q">Hybrid search = which signals?</div><div class="fc-a"><strong>BM25 keyword + vector similarity</strong> fused with RRF.</div></div>

<div class="flashcard"><div class="fc-q">Semantic ranker — what does it do?</div><div class="fc-a">L2 reranker over top results using deep model; produces <strong>captions</strong> and <strong>answers</strong>. Standard tier S1+.</div></div>

<div class="flashcard"><div class="fc-q">Best chunking strategy for prose docs?</div><div class="fc-a">Token-based (~512–1024) with <strong>10–25% overlap</strong>. Markdown/HTML use structural splits.</div></div>

<div class="flashcard"><div class="fc-q">Indexer vs push API?</div><div class="fc-a">Indexer = pull from data source on schedule + skillset. Push = your code uploads via REST/SDK.</div></div>

<div class="flashcard"><div class="fc-q">Skillset stages?</div><div class="fc-a">Cracking → enrichment skills (OCR, key phrases, embedding, custom WebApi) → projections to index/KB.</div></div>

<div class="flashcard"><div class="fc-q">Multilingual embeddings — model choice?</div><div class="fc-a"><code>text-embedding-3-large</code> works cross-lingually; alternative is Cohere Embed-Multilingual.</div></div>

<div class="flashcard"><div class="fc-q">Filter results by user permissions in RAG?</div><div class="fc-a">Add a <strong>security filter</strong> field (group IDs); query filter <code>group_ids/any(g: search.in(g, ''...''))</code>.</div></div>

<div class="flashcard"><div class="fc-q">Indexer auth to private storage?</div><div class="fc-a">Enable <strong>system-assigned identity</strong> on search service; assign <code>Storage Blob Data Reader</code>; use managed-identity datasource.</div></div>

</div>
</section>

<section class="fc-section" data-fc-title="Vision, Speech & Document Intelligence">
<h2>5 · Vision, Speech & Document Intelligence</h2>

<div class="flashcard-grid">

<div class="flashcard"><div class="fc-q">Read API vs general OCR?</div><div class="fc-a">Read = newer, async, better for printed + handwritten + multipage docs.</div></div>

<div class="flashcard"><div class="fc-q">Custom Vision vs Image Analysis 4.0?</div><div class="fc-a">Custom Vision = train your own classifier/object detector. Image Analysis 4.0 = pre-trained tags/captions/objects via Florence.</div></div>

<div class="flashcard"><div class="fc-q">Document Intelligence prebuilt vs custom?</div><div class="fc-a">Prebuilt = invoice/receipt/ID/W-2 etc. Custom = train on your forms (template or neural). Composed = combine multiple custom models.</div></div>

<div class="flashcard"><div class="fc-q">Speech-to-text — batch vs real-time?</div><div class="fc-a">Real-time = SDK live captions. Batch = REST submit URLs of audio for async transcription.</div></div>

<div class="flashcard"><div class="fc-q">Custom Neural Voice gating?</div><div class="fc-a">Limited Access — requires application + responsible-AI approval.</div></div>

<div class="flashcard"><div class="fc-q">Translator: document vs text?</div><div class="fc-a">Document translation preserves layout (Office, PDF). Text = string-by-string. Both support custom dictionaries.</div></div>

<div class="flashcard"><div class="fc-q">Content Understanding service?</div><div class="fc-a">Foundry service that extracts structured data from <strong>documents, images, audio, video</strong> via prompt-defined schema.</div></div>

<div class="flashcard"><div class="fc-q">Detect a face''s identity for verification?</div><div class="fc-a">Face API <strong>Verify</strong> (1:1) — Limited Access. Identification (1:N) also gated.</div></div>

</div>
</section>

<section class="fc-section" data-fc-title="Security, Evaluation & Monitoring">
<h2>6 · Security, Evaluation & Monitoring</h2>

<div class="flashcard-grid">

<div class="flashcard"><div class="fc-q">Prompt Shields — what does it block?</div><div class="fc-a"><strong>User attacks</strong> (jailbreaks) and <strong>indirect attacks</strong> (prompt injection in retrieved docs).</div></div>

<div class="flashcard"><div class="fc-q">Groundedness detection?</div><div class="fc-a">Content Safety check that flags responses not supported by provided context — RAG hallucination guard.</div></div>

<div class="flashcard"><div class="fc-q">Built-in Foundry RAG evaluators?</div><div class="fc-a">Groundedness, relevance, retrieval, fluency, coherence, similarity + risk evals (violence/self-harm/sexual/hate, jailbreak).</div></div>

<div class="flashcard"><div class="fc-q">Trace agent runs end-to-end?</div><div class="fc-a">Enable <strong>tracing</strong> on project → OpenTelemetry to App Insights. View in Foundry portal or Azure Monitor.</div></div>

<div class="flashcard"><div class="fc-q">Restrict AOAI to your VNet only?</div><div class="fc-a">Disable public network access; create <strong>private endpoint</strong>; private DNS zone <code>privatelink.openai.azure.com</code>.</div></div>

<div class="flashcard"><div class="fc-q">Manage AOAI quota across teams?</div><div class="fc-a">Use <strong>API Management</strong> as AI Gateway: token rate limit, semantic caching, load-balance backends.</div></div>

<div class="flashcard"><div class="fc-q">Customer-managed keys (CMK) on Foundry?</div><div class="fc-a">Configure CMK at hub creation against Key Vault; encrypts metadata, prompts, traces. Cannot enable post-creation.</div></div>

<div class="flashcard"><div class="fc-q">Red-teaming agent vs eval?</div><div class="fc-a">Eval = scored metrics on a dataset. Red-teaming agent = adversarial probing (PyRIT-based) for risky outputs.</div></div>

</div>
</section>
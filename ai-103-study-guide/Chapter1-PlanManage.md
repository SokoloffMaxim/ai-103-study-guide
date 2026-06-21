# Chapter 1 - Plan and Manage an Azure AI Solution

**AI-103 exam weighting: 25–30%**

This chapter covers the foundation of an Azure AI solution. Before building prompts, agents, search indexes, or document-processing pipelines, you need to understand how to choose the right services, plan the architecture, secure the environment, monitor usage, and apply Responsible AI controls.

For AI-103, this domain is important because many exam questions are scenario-based. You may be asked to choose between different services, models, deployment approaches, security controls, or monitoring options.

Official Microsoft reference:

* [AI-103 study guide](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-103)
* [Microsoft Foundry documentation](https://learn.microsoft.com/en-us/azure/foundry/)
* [What is Microsoft Foundry?](https://learn.microsoft.com/en-us/azure/foundry/what-is-foundry)

---

## 1.1 Choose the Right Azure AI and Foundry Services

The first design decision is choosing the correct service or pattern for the job.

A common exam trap is assuming that every AI requirement needs a large language model. In real projects, the better answer is often a dedicated Azure AI service, Azure AI Search, Document Intelligence, Speech, Vision, Content Safety, or a smaller model.

### What to Think About First

Before choosing a service, ask:

* What type of input does the app process?
* Does the solution need text, image, audio, video, or documents?
* Does the app need generative output or deterministic extraction?
* Does the model need private enterprise data?
* Does the app need real-time responses?
* Are there safety, compliance, or auditing requirements?
* Can a smaller, cheaper, faster model solve the task?

---

## Model Selection Notes

### Large Language Models

Use larger language models when the task requires strong reasoning, summarisation, planning, conversation, or open-ended generation.

Good fit for:

* Complex question answering
* Multi-step reasoning
* Summarising long content
* Drafting and rewriting
* Planning agent actions
* Handling ambiguous user requests

Trade-offs:

* Higher cost
* Higher latency
* More need for evaluation and safety controls
* Greater risk of hallucination if not grounded

Official reference:

* [Microsoft Foundry models](https://learn.microsoft.com/en-us/azure/foundry/foundry-models/)

---

### Small Language Models

Use smaller models when the task is narrow, repetitive, or latency-sensitive.

Good fit for:

* Classification
* Simple extraction
* Routing requests
* Basic summarisation
* Rewriting short text
* Simple conversational flows

Trade-offs:

* Weaker reasoning
* Less flexible with complex instructions
* May need clearer prompts and tighter evaluation

Exam reminder:

> Do not always choose the most powerful model. Choose the smallest model that meets the quality, latency, and cost requirements.

---

### Multimodal Models

Use multimodal models when the app needs to understand more than plain text.

Good fit for:

* Image question answering
* Screenshots
* Diagrams
* Receipts or visual documents
* Mixed text and image analysis

However, if the task is mainly extracting structured data from forms or invoices, **Azure AI Document Intelligence** may be a better fit than a general multimodal model.

Official references:

* [Azure AI Vision documentation](https://learn.microsoft.com/en-us/azure/ai-services/computer-vision/)
* [Azure AI Document Intelligence documentation](https://learn.microsoft.com/en-us/azure/ai-services/document-intelligence/)

---

### Dedicated Azure AI Services

Use a dedicated Azure AI service when the requirement is specific and well-defined.

Examples:

| Requirement                           | Better Service                 |
| ------------------------------------- | ------------------------------ |
| Translate text                        | Azure AI Translator            |
| Convert speech to text                | Azure AI Speech                |
| Convert text to speech                | Azure AI Speech                |
| Extract fields from invoices or forms | Azure AI Document Intelligence |
| Detect unsafe content                 | Azure AI Content Safety        |
| Search private data                   | Azure AI Search                |
| Analyse images                        | Azure AI Vision                |

Exam reminder:

> If a dedicated Azure AI service already solves the problem, it may be safer, cheaper, and more predictable than prompting a general-purpose LLM.

Official references:

* [Azure AI services documentation](https://learn.microsoft.com/en-us/azure/ai-services/)
* [Azure AI Speech documentation](https://learn.microsoft.com/en-us/azure/ai-services/speech-service/)
* [Azure AI Language documentation](https://learn.microsoft.com/en-us/azure/ai-services/language-service/)
* [Azure AI Content Safety documentation](https://learn.microsoft.com/en-us/azure/ai-services/content-safety/)

---

## Retrieval and Indexing Choices

Many AI-103 scenarios involve grounding a generative AI app with private data.

This usually means using **retrieval-augmented generation**, or RAG.

In a RAG pattern, the app:

1. Receives a user question
2. Searches trusted data sources
3. Retrieves relevant chunks
4. Adds those chunks to the model prompt
5. Generates a grounded response
6. Optionally returns citations or source references

Official reference:

* [Azure AI Search documentation](https://learn.microsoft.com/en-us/azure/search/)

---

### Keyword Search

Keyword search is useful when exact terms matter.

Good fit for:

* Product codes
* Policy names
* IDs
* Exact phrases
* Structured fields
* Filtering and faceted search

Weakness:

* It may miss relevant content when the user uses different wording.

---

### Vector Search

Vector search is useful when meaning matters more than exact words.

Good fit for:

* Conceptual similarity
* Natural language questions
* Semantic matching
* Cross-language or fuzzy matching
* Searching long-form content

Weakness:

* It may retrieve conceptually similar but imprecise results.
* It still needs good chunking, metadata, and evaluation.

Official reference:

* [Vector search in Azure AI Search](https://learn.microsoft.com/en-us/azure/search/vector-search-overview)

---

### Semantic Ranking

Semantic ranking improves relevance by reranking search results using language understanding models.

Good fit for:

* Improving top result quality
* Natural language queries
* RAG systems where top chunks matter
* Reducing weak retrieval results before generation

Official reference:

* [Semantic ranking in Azure AI Search](https://learn.microsoft.com/en-us/azure/search/semantic-search-overview)

---

### Hybrid Search

Hybrid search combines keyword search and vector search.

This is often a strong default for enterprise RAG because it balances:

* Exact matching
* Semantic similarity
* Better recall
* Better precision

Official references:

* [Hybrid search in Azure AI Search](https://learn.microsoft.com/en-us/azure/search/hybrid-search-overview)
* [Hybrid search ranking](https://learn.microsoft.com/en-us/azure/search/hybrid-search-ranking)

Exam reminder:

> For many RAG scenarios, hybrid search with semantic ranking is a strong design choice because it combines keyword precision with vector-based meaning.

---

## Agent Knowledge, Tools, and Memory

Agentic solutions need more than a model. They usually need:

* Instructions
* Tools
* Knowledge sources
* Memory
* Security boundaries
* Monitoring
* Human approval for sensitive actions

Official reference:

* [Microsoft Foundry Agent Service](https://learn.microsoft.com/en-us/azure/foundry/agents/overview)

---

### Foundry Agent Service

Microsoft Foundry Agent Service is used to build, deploy, and scale AI agents.

Agents can be designed to:

* Answer questions
* Use tools
* Retrieve knowledge
* Call APIs
* Run workflows
* Interact with users
* Perform multi-step tasks

Exam reminder:

> An agent is useful when the solution needs reasoning plus action. If the app only needs one predictable step, a normal workflow or function may be better.

---

### Foundry IQ

Foundry IQ provides a managed knowledge layer for agents.

It helps agents use enterprise or web content as grounded knowledge, with permission-aware access and citation-backed responses.

Official reference:

* [What is Foundry IQ?](https://learn.microsoft.com/en-us/azure/foundry/agents/concepts/what-is-foundry-iq)

---

### Tool Use

Agents can use tools to perform actions outside the model.

Examples:

* Search an index
* Query a database
* Call an API
* Trigger a workflow
* Read a document
* Create a ticket
* Send information to another system

Tool access should be restricted. Give the agent only the tools it needs.

Exam reminder:

> Tool access is a security boundary. A read-only support agent should not have tools that modify production systems.

---

## 1.2 Set Up AI Solutions in Microsoft Foundry

Microsoft Foundry is the central platform for building, managing, deploying, and monitoring AI apps, models, and agents.

Official references:

* [Microsoft Foundry documentation](https://learn.microsoft.com/en-us/azure/foundry/)
* [What is Microsoft Foundry?](https://learn.microsoft.com/en-us/azure/foundry/what-is-foundry)
* [Create a Microsoft Foundry project](https://learn.microsoft.com/en-us/azure/foundry/how-to/create-projects)

---

## Foundry Architecture Basics

A typical Foundry-based solution includes:

* A Foundry resource
* One or more projects
* Model deployments
* Agent definitions
* Connected data sources
* Tools
* Evaluation assets
* Monitoring and governance settings

A project is where you normally organize development work for a specific AI app, agent, or solution.

---

## Planning the Azure Infrastructure

When planning infrastructure for an AI solution, consider:

* Region availability
* Model availability
* Quotas and rate limits
* Networking requirements
* Identity and access control
* Data location and compliance
* Monitoring and logging
* Cost management
* CI/CD and environment separation

Common environments:

| Environment | Purpose                                                 |
| ----------- | ------------------------------------------------------- |
| Development | Build and experiment                                    |
| Test        | Validate functionality and safety                       |
| Production  | Run controlled workloads with monitoring and governance |

Exam reminder:

> Region choice matters because not every model, feature, or deployment option is available in every Azure region.

---

## Model Deployment Planning

When deploying a model, plan:

* Model family
* Model version
* Deployment name
* Region
* Throughput requirements
* Token limits
* Cost
* Upgrade policy
* Monitoring requirements

A model deployment is the named endpoint your application calls.

For exam scenarios, remember that the deployment name used by your app may be different from the underlying model name.

---

## Agent Deployment Planning

When deploying an agent, plan:

* Agent instructions
* Model deployment
* Tools
* Knowledge sources
* Memory needs
* Security permissions
* Human approval requirements
* Monitoring and tracing
* Evaluation process

Good agent design should be narrow and controlled.

Poor design:

> “This agent can do everything.”

Better design:

> “This support agent can answer product questions from approved documentation and create a support ticket only after user confirmation.”

---

## CI/CD for AI Solutions

AI solutions should be managed like normal software, but with extra controls for prompts, models, evaluations, and safety.

CI/CD may include:

* Infrastructure-as-code deployments
* Prompt and agent configuration versioning
* Model deployment configuration
* Search index deployment
* Automated test prompts
* Evaluation datasets
* Safety tests
* Smoke tests after deployment
* Rollback plans

Useful tools:

* Azure DevOps
* GitHub Actions
* Azure CLI
* Azure Developer CLI
* Bicep
* Terraform
* Foundry SDKs

Exam reminder:

> AI app deployment is not just code deployment. Prompts, indexes, model settings, safety settings, and evaluation assets also need lifecycle management.

---

## 1.3 Manage, Monitor, and Secure AI Systems

AI apps and agents need operational controls. The exam may test how you manage cost, quotas, performance, safety, data access, and reliability.

---

## Quotas, Rate Limits, and Cost

Model usage is usually limited by quotas such as:

* Tokens per minute
* Requests per minute
* Regional capacity
* Deployment capacity
* Model availability

Cost is affected by:

* Model choice
* Input tokens
* Output tokens
* Number of calls
* Retrieval volume
* Search index size
* Document processing volume
* Evaluation runs
* Logging and monitoring retention

Cost-control techniques:

* Use smaller models where possible
* Reduce unnecessary prompt size
* Chunk data carefully
* Cache repeated results
* Limit agent iterations
* Use retrieval filters
* Monitor token usage
* Use budgets and alerts

Exam reminder:

> A badly designed agent can become expensive if it repeatedly calls tools, loops through steps, or sends large prompts.

---

## Monitoring AI Applications

Monitor both the application and the AI-specific behaviour.

General app metrics:

* Availability
* Latency
* Error rate
* Request volume
* Dependency failures

AI-specific metrics:

* Token usage
* Model latency
* Prompt size
* Completion size
* Retrieval quality
* Citation quality
* User feedback
* Safety filter events
* Tool call failures
* Agent step count
* Evaluation scores

For RAG systems, also monitor:

* Indexer success and failures
* Document ingestion status
* Search latency
* Top search results
* Empty search results
* Relevance quality
* Chunking quality

Official references:

* [Monitor applications with Azure Monitor](https://learn.microsoft.com/en-us/azure/azure-monitor/app/app-insights-overview)
* [Azure AI Search monitoring](https://learn.microsoft.com/en-us/azure/search/search-monitor-logs)

---

## Drift and Quality Monitoring

AI quality can degrade over time even if the model does not change.

Possible causes:

* Source documents change
* User questions change
* Model versions change
* Prompts are modified
* Search indexes become stale
* New edge cases appear
* Tool behaviour changes

Use evaluation to check:

* Groundedness
* Relevance
* Coherence
* Safety
* Correctness
* Retrieval quality
* Tool-use accuracy

Exam reminder:

> Monitoring is not only about uptime. For AI systems, you also need to monitor answer quality, safety, and grounding.

---

## Security Configuration

Security for Azure AI solutions usually includes:

* Microsoft Entra ID
* Managed identities
* Azure RBAC
* Keyless authentication
* Private endpoints
* Network restrictions
* Secrets management
* Data access controls
* Audit logging

---

### Managed Identity

Managed identity allows Azure resources to authenticate to other Azure services without storing secrets in code.

Good fit for:

* Function App calling Azure AI Search
* App Service calling Foundry
* Agent tools accessing storage
* Backend APIs accessing Key Vault

Official reference:

* [Managed identities for Azure resources](https://learn.microsoft.com/en-us/entra/identity/managed-identities-azure-resources/overview)

---

### Keyless Authentication

For production workloads, avoid hard-coded API keys when possible.

Use Microsoft Entra ID authentication and managed identity where supported.

In Python development, `DefaultAzureCredential` is convenient because it can use local developer identity, Azure CLI login, Visual Studio Code login, or managed identity depending on environment.

Exam reminder:

> API keys are simple, but managed identity and RBAC are usually better for production security.

Official reference:

* [Azure Identity client library for Python](https://learn.microsoft.com/en-us/python/api/overview/azure/identity-readme)

---

### Private Networking

Private networking helps reduce public exposure.

Common controls:

* Private endpoints
* Virtual network integration
* Firewall rules
* Disable public network access where appropriate
* Restrict access to trusted networks

Official reference:

* [Azure Private Link documentation](https://learn.microsoft.com/en-us/azure/private-link/)

---

### RBAC for Microsoft Foundry

Microsoft Foundry uses Azure RBAC roles to control access.

Current role names include:

* Foundry User
* Foundry Owner
* Foundry Account Owner
* Foundry Project Manager

Some older Microsoft documentation or portal areas may still show older names such as Azure AI User or Azure AI Owner while the rename is rolling out.

Official reference:

* [Role-based access control for Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/concepts/rbac-foundry)

Exam reminder:

> Use least privilege. Do not give broad Owner or Contributor permissions when a narrower Foundry role is enough.

---

## 1.4 Apply Responsible AI Controls

Responsible AI appears across the whole AI-103 exam. It is not only a legal or governance topic. It affects design, implementation, deployment, and operations.

Official references:

* [Azure AI Content Safety documentation](https://learn.microsoft.com/en-us/azure/ai-services/content-safety/)
* [What is Azure AI Content Safety?](https://learn.microsoft.com/en-us/azure/ai-services/content-safety/overview)

---

## Azure AI Content Safety

Azure AI Content Safety helps detect unsafe or unwanted text and image content.

Common safety areas include:

* Hate
* Violence
* Sexual content
* Self-harm
* Prompt attacks
* Ungrounded responses
* Protected material
* Custom unsafe categories

---

### Prompt Shields

Prompt Shields help detect prompt injection and jailbreak-style attacks against language models.

This matters for apps that accept user input, retrieve external content, or allow agents to use tools.

Official reference:

* [Prompt Shields in Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/openai/concepts/content-filter-prompt-shields)

Exam reminder:

> Prompt injection is especially risky when a model can access tools, private data, or external systems.

---

### Groundedness Detection

Groundedness detection checks whether generated text is supported by the source material provided to the model.

This is especially important for RAG systems.

Official reference:

* [Groundedness detection quickstart](https://learn.microsoft.com/en-us/azure/ai-services/content-safety/quickstart-groundedness)

Exam reminder:

> Grounding helps reduce hallucination, but it does not remove the need for evaluation, monitoring, and human review in high-risk scenarios.

---

### Content Filtering

Content filters can be used to detect and block unsafe text or image content.

Typical uses:

* User input moderation
* Model output moderation
* Image moderation
* Safety reporting
* Abuse detection

Official reference:

* [Azure AI Content Safety overview](https://learn.microsoft.com/en-us/azure/ai-services/content-safety/overview)

---

## Auditing and Traceability

AI solutions should be auditable.

Useful information to log:

* User request
* Prompt version
* Model deployment
* Retrieved documents
* Search query
* Tool calls
* Agent steps
* Safety filter result
* Final response
* User feedback
* Errors and retries

For RAG systems, record source references so answers can be reviewed later.

For agents, trace tool calls and decisions so behaviour can be investigated.

Exam reminder:

> If an AI system makes or supports business decisions, traceability becomes part of the design, not an afterthought.

---

## Human Oversight

Human oversight is important when the AI system performs sensitive actions.

Examples where human approval may be needed:

* Sending an email externally
* Updating customer records
* Creating financial transactions
* Deleting data
* Approving claims
* Escalating legal or HR decisions
* Making high-impact recommendations

Design controls:

* Require confirmation before action
* Use approval workflows
* Restrict write tools
* Use read-only tools by default
* Log all actions
* Limit agent iterations
* Escalate uncertain cases to humans

Exam reminder:

> The more impact an agent action has, the more control, approval, and logging it needs.

---

## Agent Governance

Agents need stronger governance than normal chat apps because they can reason, call tools, and take actions.

Good governance includes:

* Clear agent purpose
* Narrow instructions
* Limited tools
* Least-privilege identity
* Human approval for sensitive actions
* Maximum iteration limits
* Output validation
* Content safety controls
* Monitoring and tracing
* Regular evaluation

Poor governance example:

> An agent has access to every company API and decides for itself what to do.

Better governance example:

> An agent can search approved documentation, summarise the result, and create a draft ticket only after the user confirms.

---

## Chapter 1 Exam Checklist

Before moving to Chapter 2, make sure you can explain:

* When to use Foundry, Azure AI Search, Document Intelligence, Vision, Speech, Language, and Content Safety
* When to use a large model versus a smaller model
* When to use RAG instead of fine-tuning
* Why hybrid search is useful for enterprise RAG
* How model deployments are planned
* How agents use tools and knowledge sources
* Why least privilege matters for agent tools
* How managed identity improves security
* How private endpoints reduce public exposure
* How to monitor token usage, latency, quality, and safety
* Why Responsible AI controls are part of the architecture
* How to apply human oversight for sensitive agent actions

---

## Quick Exam Memory Block

```text
Plan first.
Choose the smallest suitable model.
Use dedicated Azure AI services for specific tasks.
Use RAG when the answer must come from private or current data.
Use Azure AI Search for indexing and retrieval.
Use hybrid search when both keyword precision and semantic meaning matter.
Use managed identity and RBAC for production access.
Use Content Safety for unsafe content, prompt attacks, and groundedness checks.
Limit agent tools.
Add human approval for sensitive actions.
Monitor cost, latency, retrieval quality, safety, and grounding.
```

---

**Previous:** [README](../README.md)
**Next:** [Chapter 2 - Generative AI and Agentic Solutions](./Chapter2-GenerativeAgents.md)

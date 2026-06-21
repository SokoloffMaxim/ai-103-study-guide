# Chapter 2 — Implement Generative AI and Agentic Solutions

**AI-103 exam weighting: 30–35%**

This is the largest AI-103 exam domain. It covers generative AI applications, retrieval-augmented generation, agent design, tool use, orchestration, evaluation, and operational controls.

In simple terms, this chapter is about building AI systems that do more than return a basic chat response. The exam expects you to understand how to ground model responses, connect agents to tools, evaluate output quality, and choose the safest and most practical architecture for a scenario.

Official Microsoft references:

* [AI-103 study guide](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-103)
* [Microsoft Foundry documentation](https://learn.microsoft.com/en-us/azure/foundry/)
* [Microsoft Foundry Agent Service](https://learn.microsoft.com/en-us/azure/foundry/agents/overview)
* [Microsoft Agent Framework](https://learn.microsoft.com/en-us/agent-framework/overview/)
* [Develop AI agents on Azure](https://learn.microsoft.com/en-us/training/paths/develop-ai-agents-azure/)

---

## 2.1 Build Generative Applications with Microsoft Foundry

Generative AI applications use models to create, summarise, transform, classify, reason over, or explain content.

For AI-103, the important part is not just knowing how to call a model. You need to understand how to design the full application around the model.

A typical generative AI application may include:

* A user interface or API
* A model deployment
* Prompt instructions
* Retrieval from trusted data
* Tool or function calls
* Safety filters
* Evaluation
* Monitoring and logging

---

## Model Use in Generative Applications

Microsoft Foundry can be used to discover, deploy, and consume models for different types of workloads.

Common model choices include:

| Model Type            | When to Use                                                                  |
| --------------------- | ---------------------------------------------------------------------------- |
| Large language models | Complex reasoning, summarisation, planning, and open-ended generation        |
| Small language models | Faster, cheaper tasks such as routing, classification, and simple extraction |
| Code models           | Code generation, code explanation, and developer-assistant scenarios         |
| Multimodal models     | Inputs that include text, images, audio, or mixed content                    |
| Embedding models      | Vector search, similarity matching, and RAG pipelines                        |

Exam reminder:

> Do not automatically choose the largest model. Choose the smallest model that meets the quality, latency, and cost requirements.

Official reference:

* [Microsoft Foundry models](https://learn.microsoft.com/en-us/azure/foundry/foundry-models/)

---

## Retrieval-Augmented Generation

Retrieval-augmented generation, or RAG, is one of the most important AI-103 patterns.

Use RAG when the model needs to answer using information that is:

* Private
* Current
* Domain-specific
* Too large to fit in a prompt
* Required to be traceable to source documents

A simple RAG flow looks like this:

```text
User question
    ↓
Search trusted data
    ↓
Retrieve relevant chunks
    ↓
Add chunks to the prompt
    ↓
Generate grounded answer
    ↓
Return answer with sources
```

RAG is usually a better choice than fine-tuning when the main problem is missing knowledge.

Fine-tuning changes model behaviour.
RAG supplies external knowledge at runtime.

Official references:

* [Azure AI Search documentation](https://learn.microsoft.com/en-us/azure/search/)
* [Vector search in Azure AI Search](https://learn.microsoft.com/en-us/azure/search/vector-search-overview)
* [Hybrid search in Azure AI Search](https://learn.microsoft.com/en-us/azure/search/hybrid-search-overview)

---

## RAG Design Decisions

When designing a RAG solution, think about:

* What content needs to be indexed?
* How should documents be split into chunks?
* What metadata should be stored?
* Should retrieval use keyword search, vector search, semantic ranking, or hybrid search?
* How many chunks should be passed to the model?
* Should the answer include citations?
* How should irrelevant or low-confidence results be handled?
* How will retrieval quality be evaluated?

Common mistakes:

* Indexing everything without cleaning the data
* Creating chunks that are too large or too small
* Forgetting metadata filters
* Passing too much context to the model
* Trusting generated answers without checking retrieved sources

Exam reminder:

> RAG quality depends heavily on ingestion, chunking, indexing, retrieval, and prompt construction. It is not only a model problem.

---

## Tool-Augmented Generation

Some tasks need more than text generation. The model may need to call a tool.

Tools can be used to:

* Search a knowledge base
* Query a database
* Call an API
* Run code
* Retrieve a document
* Create a ticket
* Check live information
* Perform structured validation

A tool-augmented flow usually looks like this:

```text
User request
    ↓
Model decides a tool is needed
    ↓
Tool is called with structured arguments
    ↓
Tool result is returned to the model
    ↓
Model produces final response
```

Official reference:

* [Agent tools overview for Microsoft Foundry Agent Service](https://learn.microsoft.com/en-us/azure/foundry/agents/concepts/tool-catalog)

Exam reminder:

> Use tools for actions, live data, deterministic checks, and external systems. Do not ask the model to guess information that should come from a tool or database.

---

## Connecting Applications to Foundry

Applications can connect to Microsoft Foundry through SDKs and endpoints depending on the scenario.

Common options include:

| Option             | Use Case                                                             |
| ------------------ | -------------------------------------------------------------------- |
| Foundry SDK        | Foundry projects, agents, evaluations, and platform features         |
| Agent Framework    | Code-based agents and multi-agent workflows                          |
| OpenAI SDK         | Model inference with OpenAI-compatible APIs                          |
| Foundry Tools SDKs | Specific Azure AI services such as Vision, Speech, or Content Safety |

Use Microsoft Entra ID and managed identity where possible instead of hard-coded keys.

Official reference:

* [Microsoft Foundry SDKs and endpoints](https://learn.microsoft.com/en-us/azure/foundry/how-to/develop/sdk-overview)

---

## Evaluation of Generative AI Apps

Evaluation is part of the application lifecycle. It should not be left until production.

Evaluate for:

* Correctness
* Relevance
* Groundedness
* Coherence
* Safety
* Retrieval quality
* Citation quality
* Latency
* Cost
* User satisfaction

For RAG solutions, evaluation should check both:

1. Whether the right content was retrieved
2. Whether the generated answer used that content correctly

Exam reminder:

> A response can sound fluent and still be wrong. AI-103 expects you to think about evaluation, not just generation.

---

## 2.2 Build Agents with Foundry

An agent is more than a chatbot.

A simple chatbot usually responds with text.
An agent can reason over a task, use tools, retrieve knowledge, track context, and perform multi-step actions.

In Microsoft Foundry, an agent usually combines:

* A model
* Instructions
* Tools
* Optional knowledge sources
* Optional memory or conversation state
* Monitoring and tracing
* Safety controls

Official reference:

* [Microsoft Foundry Agent Service](https://learn.microsoft.com/en-us/azure/foundry/agents/overview)

---

## Agent Design Basics

Before building an agent, define:

| Design Area | Question                             |
| ----------- | ------------------------------------ |
| Role        | What is this agent responsible for?  |
| Goal        | What outcome should it produce?      |
| Boundaries  | What must it not do?                 |
| Tools       | What systems can it access?          |
| Knowledge   | What data should ground its answers? |
| Memory      | What context should be retained?     |
| Safety      | When should it ask for approval?     |
| Monitoring  | What behaviour should be logged?     |

Weak design:

```text
You are an AI assistant. Help with anything.
```

Better design:

```text
You are a support triage agent. You answer questions using approved product documentation.
If the user asks for an account change, create a draft ticket but do not submit it without confirmation.
```

Exam reminder:

> Agents should be narrow, controlled, and auditable. Broad agents with too many tools are harder to secure and test.

---

## Agent Tools

Agents can use tools to access data or perform actions.

Examples:

* Azure AI Search indexes
* Foundry IQ knowledge sources
* OpenAPI tools
* Custom functions
* MCP servers
* Code execution
* Web search
* File search
* Content understanding tools

Tool access should follow least privilege.

A read-only information agent should not have tools that modify production systems. A ticketing agent should not be able to delete customer records. A finance agent should not execute a transaction without approval.

Official references:

* [Agent tools overview](https://learn.microsoft.com/en-us/azure/foundry/agents/concepts/tool-catalog)
* [Use function calling with Microsoft Foundry agents](https://learn.microsoft.com/en-us/azure/foundry/agents/how-to/tools/function-calling)

---

## Knowledge and Memory

Agents often need knowledge and context.

Knowledge helps the agent answer using trusted information.
Memory or conversation state helps the agent keep track of previous turns or task progress.

Knowledge sources may include:

* Azure AI Search
* Foundry IQ
* Files
* SharePoint or Microsoft 365 data
* Databases
* APIs
* Domain-specific content stores

Official reference:

* [Build knowledge-enhanced AI agents with Foundry IQ](https://learn.microsoft.com/en-us/training/modules/build-knowledge-enhanced-ai-agents-foundry-iq/)

Exam reminder:

> Use knowledge sources when the agent needs trusted information. Use memory when the agent needs continuity across steps or conversations.

---

## Autonomous and Semiautonomous Agents

Agents can operate with different levels of autonomy.

| Type           | Description                                                                  |
| -------------- | ---------------------------------------------------------------------------- |
| Human-led      | The user controls each action                                                |
| Semiautonomous | The agent can plan and suggest actions, but sensitive steps require approval |
| Autonomous     | The agent can act with minimal human intervention within defined limits      |

For most business scenarios, semiautonomous is safer than fully autonomous.

Safeguards include:

* Tool approval
* Human-in-the-loop review
* Maximum iteration limits
* Read-only tools by default
* Clear escalation rules
* Restricted identities
* Audit logging
* Safety filtering

Exam reminder:

> The more impact an agent action has, the more approval, logging, and restriction it needs.

---

## 2.3 Optimize and Operationalize Generative AI Systems

After a generative app or agent works, the next task is improving it and running it safely.

Operational concerns include:

* Prompt quality
* Model parameters
* Retrieval quality
* Latency
* Token usage
* Safety
* Monitoring
* Evaluation
* Cost
* Reliability

---

## Prompt Engineering and Parameters

Prompt engineering is still important, even when using RAG or agents.

Good prompts usually define:

* Role
* Task
* Context
* Constraints
* Output format
* Safety rules
* What to do when information is missing

Common model parameters:

| Parameter                       | Purpose                           |
| ------------------------------- | --------------------------------- |
| Temperature                     | Controls randomness               |
| Max output tokens               | Limits response length            |
| Top-p                           | Controls token sampling diversity |
| Stop sequences                  | Stops generation at defined text  |
| Frequency or presence penalties | Reduce repetition where supported |

Typical guidance:

* Use lower temperature for factual or compliance-heavy answers.
* Use higher temperature for brainstorming or creative writing.
* Limit max output tokens to control cost and verbosity.
* Use structured output when the next system expects JSON or fields.

Exam reminder:

> Parameter tuning can improve output style and consistency, but it does not replace grounding, evaluation, or safety controls.

---

## Fine-Tuning

Fine-tuning adapts a model to a specific task or style using training examples.

Use fine-tuning when you need:

* Consistent response format
* Domain-specific behaviour
* Better performance on repeated task types
* Shorter prompts
* Lower latency with smaller models
* Behaviour that cannot be reliably achieved with prompting alone

Do not use fine-tuning as the first solution when the problem is missing or changing knowledge. Use RAG for that.

Good fine-tuning use case:

```text
The model must consistently classify support tickets into company-specific categories.
```

Poor fine-tuning use case:

```text
The model needs to know the latest company policy documents.
```

For the second case, use RAG or a knowledge source.

Official references:

* [Customize a model with fine-tuning](https://learn.microsoft.com/en-us/azure/foundry/openai/how-to/fine-tuning)
* [Microsoft Foundry fine-tuning considerations](https://learn.microsoft.com/en-us/azure/foundry/openai/concepts/fine-tuning-considerations)

Exam reminder:

> Fine-tuning changes behaviour. RAG adds knowledge. Many real solutions use prompt engineering, RAG, and evaluation before considering fine-tuning.

---

## Reflection, Self-Check, and Evaluator Patterns

Generative systems can use extra steps to improve quality.

Examples:

| Pattern            | Use Case                                                             |
| ------------------ | -------------------------------------------------------------------- |
| Self-check         | Ask the model to review its own answer before final output           |
| Evaluator model    | Use a separate model call to score or critique the answer            |
| Groundedness check | Check whether the answer is supported by source content              |
| Rules validation   | Use deterministic code to validate required fields or business rules |
| Human review       | Require approval before high-impact actions                          |

These patterns can improve reliability, but they also increase cost and latency.

Exam reminder:

> Use extra evaluation steps when accuracy, safety, or compliance matters more than speed.

---

## Observability

AI observability means tracking both system performance and AI behaviour.

Monitor:

* Latency
* Errors
* Token usage
* Model deployment usage
* Retrieval queries
* Retrieved documents
* Safety filter events
* Tool calls
* Agent steps
* User feedback
* Failed or repeated tool calls
* Cost trends

For agents, tracing is especially important because the final answer may hide many internal steps.

Exam reminder:

> If an agent can call tools, you need to know what it called, why it called it, and what happened.

---

## Hybrid AI Systems

Not every task should be solved by an LLM.

A strong AI application often combines:

* LLMs for language understanding and generation
* Azure AI Search for retrieval
* APIs for live data
* Code for calculations
* Rules engines for deterministic decisions
* Databases for source-of-truth records
* Human approval for sensitive actions

Example:

```text
Use the model to understand the user's request.
Use Azure AI Search to retrieve the policy.
Use code to calculate the refund amount.
Use a workflow to request approval.
Use the model to explain the result clearly.
```

Exam reminder:

> Use the model where language and reasoning are useful. Use code, tools, and workflows where precision and control are required.

---

## 2.4 Agent Orchestration Patterns

Multi-agent orchestration is used when one agent is not enough or when the task benefits from multiple roles.

Microsoft Agent Framework includes several orchestration patterns:

* Sequential
* Concurrent
* Handoff
* Group Chat
* Magentic

Official references:

* [Workflow orchestrations in Agent Framework](https://learn.microsoft.com/en-us/agent-framework/workflows/orchestrations/)
* [AI agent orchestration patterns](https://learn.microsoft.com/en-us/azure/architecture/ai-ml/guide/ai-agent-design-patterns)

---

## Sequential Orchestration

Sequential orchestration runs agents one after another in a defined order.

Use it when the process has clear stages.

Examples:

* Research → Draft → Review
* Extract → Validate → Summarise
* Classify → Route → Respond
* Translate → Check → Publish

Exam keywords:

```text
pipeline, step-by-step, ordered stages, each stage builds on the previous one
```

---

## Concurrent Orchestration

Concurrent orchestration runs agents in parallel.

Use it when several agents should work independently on the same input and then combine their results.

Examples:

* Multiple reviewers checking the same document
* Several translation agents working at once
* Independent risk, security, and compliance reviews
* Brainstorming multiple solution options

Exam keywords:

```text
parallel, fan-out/fan-in, independent perspectives, compare results
```

---

## Handoff Orchestration

Handoff orchestration lets one agent transfer the conversation or task to another agent.

Use it when different specialists should take ownership depending on the situation.

Examples:

* General support agent → billing specialist
* Triage agent → technical support agent
* Sales assistant → licensing specialist
* First-line agent → escalation agent

Exam keywords:

```text
handoff, transfer, specialist, escalation, routing
```

---

## Group Chat Orchestration

Group chat orchestration allows multiple agents to collaborate in a shared conversation.

Use it when the task needs discussion, review, debate, or iterative refinement.

Examples:

* Architect, security engineer, and cost analyst reviewing a design
* Researcher and writer improving a report
* Multiple agents refining a project plan
* Design review with several expert roles

Exam keywords:

```text
collaboration, roundtable, shared discussion, iterative refinement
```

---

## Magentic Orchestration

Magentic orchestration uses a manager agent to dynamically coordinate specialist agents.

Use it for complex, open-ended tasks where the steps are not known in advance.

Examples:

* Investigating a broad technical problem
* Planning a complex project
* Breaking down a vague business request
* Coordinating multiple specialist agents dynamically

Exam keywords:

```text
dynamic planning, manager agent, open-ended task, task decomposition
```

---

## Orchestration Pattern Comparison

| Pattern    | Best For                  | Simple Memory Hook            |
| ---------- | ------------------------- | ----------------------------- |
| Sequential | Ordered stages            | Assembly line                 |
| Concurrent | Parallel independent work | Team working at the same time |
| Handoff    | Specialist routing        | Call transfer                 |
| Group Chat | Collaborative refinement  | Roundtable discussion         |
| Magentic   | Complex open-ended work   | Dynamic project manager       |

Exam reminder:

> Choose orchestration based on control flow. Ordered stages suggest sequential. Parallel work suggests concurrent. Specialist transfer suggests handoff. Shared discussion suggests group chat. Dynamic planning suggests magentic.

---

## Chapter 2 Exam Checklist

Before moving to Chapter 3, make sure you can explain:

* How a generative AI application connects to a model deployment
* When to use RAG
* When to use fine-tuning
* How Azure AI Search supports grounding
* Why hybrid search is useful for RAG
* How tool calling extends a model or agent
* What makes an agent different from a chatbot
* How to define an agent role, goal, tools, and boundaries
* Why agents need least-privilege tool access
* How human approval reduces risk
* How to monitor token usage, latency, safety, retrieval, and tool calls
* How sequential, concurrent, handoff, group chat, and magentic orchestration differ
* When not to use an agent

---

## Quick Exam Memory Block

```text
Use RAG when the model needs trusted or current knowledge.
Use fine-tuning when the model needs specialised behaviour or format.
Use tools when the model needs data or actions outside itself.
Use agents when the solution needs reasoning plus tool use across steps.
Keep agents narrow, secured, monitored, and auditable.
Use human approval for sensitive actions.
Use evaluation to check quality, relevance, groundedness, and safety.
Choose orchestration by control flow:
sequential = ordered stages
concurrent = parallel work
handoff = specialist transfer
group chat = shared collaboration
magentic = dynamic manager
```

---

**Previous:** [Chapter 1 — Plan and Manage an Azure AI Solution](./Chapter1-PlanManage.md)
**Home:** [README](../README.md)
**Next:** [Chapter 3 — Computer Vision Solutions](./Chapter3-ComputerVision.md)
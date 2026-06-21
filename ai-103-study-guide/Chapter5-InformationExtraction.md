# Chapter 5 — Implement Information Extraction Solutions

**AI-103 exam weighting: 10–15%**

This chapter covers how AI applications and agents extract useful information from documents, images, audio, video, and other unstructured content.

For AI-103, this domain connects directly to RAG and agents. A model or agent can only produce grounded answers if the source content is properly extracted, cleaned, chunked, indexed, retrieved, and passed into the workflow.

Official Microsoft references:

* [AI-103 study guide](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-103)
* [Azure AI Search documentation](https://learn.microsoft.com/en-us/azure/search/)
* [Vector search in Azure AI Search](https://learn.microsoft.com/en-us/azure/search/vector-search-overview)
* [Hybrid search in Azure AI Search](https://learn.microsoft.com/en-us/azure/search/hybrid-search-overview)
* [Semantic ranking in Azure AI Search](https://learn.microsoft.com/en-us/azure/search/semantic-search-overview)
* [AI enrichment in Azure AI Search](https://learn.microsoft.com/en-us/azure/search/cognitive-search-concept-intro)
* [Azure AI Document Intelligence](https://learn.microsoft.com/en-us/azure/ai-services/document-intelligence/overview)
* [Azure Content Understanding](https://learn.microsoft.com/en-us/azure/ai-services/content-understanding/overview)

---

## 5.1 Build Retrieval and Grounding Pipelines

A retrieval and grounding pipeline prepares content so an AI application or agent can search it and use it as evidence.

A typical pipeline looks like this:

```text
Raw content
    ↓
Extract text, structure, and metadata
    ↓
Clean and normalize the content
    ↓
Split content into useful chunks
    ↓
Generate embeddings where needed
    ↓
Index content in Azure AI Search
    ↓
Retrieve relevant results
    ↓
Ground the model or agent response
```

The goal is not just to make content searchable. The goal is to return the right evidence at the right time so the model can produce a grounded, traceable answer.

Exam reminder:

> RAG quality depends on the whole pipeline, not only the model.

---

## Azure AI Search

Azure AI Search is the main Azure service for indexing, retrieval, and grounding.

Use it for:

* Full-text search
* Vector search
* Hybrid search
* Semantic ranking
* Filtering and faceting
* AI enrichment
* RAG pipelines
* Agent knowledge retrieval
* Multimodal search scenarios where supported

Official reference:

* [What is Azure AI Search?](https://learn.microsoft.com/en-us/azure/search/search-what-is-azure-search)

Exam reminder:

> In AI solutions, Azure AI Search often becomes the retrieval layer that grounds generative AI and agent responses.

---

## Content Ingestion

Ingestion is the process of bringing content into the system and preparing it for extraction, indexing, and retrieval.

Common source content includes:

* PDF files
* Word documents
* HTML pages
* Scanned documents
* Images
* Audio transcripts
* Video transcripts
* Azure Blob Storage content
* Database records
* SharePoint or enterprise documents
* Web content

During ingestion, the pipeline may:

* Extract text
* Run OCR
* Detect language
* Extract tables
* Extract key-value pairs
* Identify entities
* Generate captions or descriptions
* Split content into chunks
* Add metadata
* Generate embeddings
* Store content in an index

Exam reminder:

> Retrieval quality starts during ingestion. Poor extraction, missing metadata, bad OCR, or weak chunking can make a RAG solution fail even when the model is strong.

---

## OCR in RAG Ingestion

OCR is required when the content is image-based or scanned.

Use OCR when:

* PDFs contain scanned pages
* Images contain text
* Forms are uploaded as photos
* Screenshots need to be searched
* Video frames or visual content include useful text

A simple OCR-based ingestion flow:

```text
Scanned document
    ↓
OCR extracts text
    ↓
Layout analysis identifies structure
    ↓
Text is cleaned and chunked
    ↓
Metadata is added
    ↓
Content is indexed for search
```

Exam reminder:

> If a document is scanned, normal text extraction may not work. OCR is needed before indexing or grounding.

---

## Chunking

Chunking splits long content into smaller sections that can be indexed and retrieved.

Good chunks should be:

* Small enough to fit into model context
* Large enough to preserve meaning
* Linked back to the source document
* Stored with useful metadata
* Suitable for citations
* Suitable for filtering

Useful metadata examples:

* Source file name
* Source URL
* Page number
* Section heading
* Document type
* Created or updated date
* Business owner
* Department
* Security classification
* Access permissions

Bad chunking example:

```text
A chunk cuts a policy rule in half, so the retrieved result includes the exception but not the rule.
```

Better chunking example:

```text
A chunk keeps the full policy section together and stores the page number, heading, and source document.
```

Exam reminder:

> Chunking is a design decision. The best chunk size depends on the document type, retrieval quality, and model context limits.

---

## Search Methods for Grounding

Azure AI Search supports several retrieval approaches.

| Search Type      | Best For                                                  |
| ---------------- | --------------------------------------------------------- |
| Keyword search   | Exact terms, product codes, IDs, names, policy references |
| Vector search    | Meaning-based retrieval and natural language questions    |
| Semantic ranking | Improving the order and relevance of top results          |
| Hybrid search    | Combining keyword precision with vector similarity        |

---

## Vector Search

Vector search uses embeddings to find content based on meaning instead of exact words.

Use vector search when:

* Users ask natural language questions
* Exact keywords may not match
* Content uses different wording
* Conceptual similarity matters
* The app needs multilingual or multimodal retrieval
* The search experience should understand meaning

Official reference:

* [Vector search in Azure AI Search](https://learn.microsoft.com/en-us/azure/search/vector-search-overview)

Exam reminder:

> Vector search is strong for meaning-based retrieval, but it still needs good metadata, filtering, chunking, and evaluation.

---

## Hybrid Search

Hybrid search combines full-text search and vector search.

This is often a strong default for enterprise RAG because it combines:

* Exact keyword matching
* Semantic similarity
* Better recall
* Better precision
* Metadata filtering
* Stronger grounding results

Official reference:

* [Hybrid search in Azure AI Search](https://learn.microsoft.com/en-us/azure/search/hybrid-search-overview)

Exam reminder:

> Use hybrid search when the solution needs both exact matching and meaning-based retrieval.

---

## Semantic Ranking

Semantic ranking improves the ordering of search results using language understanding.

Use semantic ranking when:

* The top results matter
* Queries are natural language
* The solution needs better relevance
* The RAG answer depends on the best few chunks
* The first retrieval results are too noisy

Official reference:

* [Semantic ranking in Azure AI Search](https://learn.microsoft.com/en-us/azure/search/semantic-search-overview)

Exam reminder:

> In RAG, the model can only answer well if the retrieved context is relevant. Ranking quality matters.

---

## AI Enrichment

AI enrichment adds useful information to content during indexing.

Built-in enrichment skills can help with:

* OCR
* Language detection
* Key phrase extraction
* Entity recognition
* Image analysis
* Text splitting
* Embedding generation
* Layout-aware extraction where supported

Custom skills can be used when the built-in skills are not enough.

Custom skills may call:

* Azure Functions
* Custom APIs
* Custom ML models
* Business rules
* External processing services

Official reference:

* [AI enrichment in Azure AI Search](https://learn.microsoft.com/en-us/azure/search/cognitive-search-concept-intro)

Example enrichment flow:

```text
Scanned PDF in Azure Blob Storage
    ↓
OCR extracts page text
    ↓
Language detection identifies document language
    ↓
Entity extraction finds people and organizations
    ↓
Custom skill adds business-specific metadata
    ↓
Chunks and embeddings are indexed
    ↓
RAG app retrieves relevant passages
```

Exam reminder:

> Use built-in skills for common enrichment. Use custom skills when the pipeline needs business-specific processing.

---

## Connecting Retrieval to Workflows and Agents

Retrieval can be connected to applications, workflows, and agents in different ways.

### Option 1 — Retrieval Before Generation

The application searches first, then passes retrieved content into the model prompt.

Good for:

* Simple RAG chat
* Predictable question-answering
* Always-grounded responses
* Scenarios where every answer should use source content

### Option 2 — Retrieval as an Agent Tool

The agent decides when to search and what query to use.

Good for:

* Multi-step tasks
* Agentic workflows
* Scenarios where search is only sometimes needed
* Workflows that combine search, APIs, and actions

### Option 3 — Shared Knowledge Layer

A shared knowledge layer can allow multiple agents to use the same grounded content source.

Good for:

* Multiple agents using the same approved knowledge
* Enterprise knowledge reuse
* Consistent citations
* Centralised knowledge management

Official reference:

* [Build knowledge-enhanced AI agents with Foundry IQ](https://learn.microsoft.com/en-us/training/modules/build-knowledge-enhanced-ai-agents-foundry-iq/)

Exam reminder:

> Simple RAG usually retrieves before the model responds. Agents may use retrieval as a tool only when needed.

---

## Agentic Retrieval

Agentic retrieval means retrieval is part of a reasoning workflow rather than a single fixed search call.

An agentic retrieval flow may:

* Break a question into subqueries
* Search multiple times
* Compare results
* Use metadata filters
* Ask follow-up questions
* Retrieve from different sources
* Combine search results with tool outputs

Use it when the user question is complex and one search query is not enough.

Exam reminder:

> For simple Q&A, normal RAG may be enough. For complex multi-step information needs, agentic retrieval can be more suitable.

---

## 5.2 Extract Content from Documents

Document extraction converts documents into structured or semi-structured information that downstream systems can use.

Common outputs include:

* Plain text
* Markdown
* Tables
* Key-value pairs
* Field values
* Layout information
* Confidence scores
* Structured JSON
* Searchable chunks
* Source references

---

## Azure AI Document Intelligence

Azure AI Document Intelligence is used for intelligent document processing.

It can extract:

* Text
* Tables
* Key-value pairs
* Selection marks
* Layout information
* Fields from prebuilt document types
* Fields from custom document models

Use Document Intelligence when the input is document-first and the goal is to extract structured information.

Good fit:

* Invoices
* Receipts
* Tax forms
* ID documents
* Contracts
* Application forms
* Tables
* Key-value pairs
* Scanned forms

Official reference:

* [Azure AI Document Intelligence](https://learn.microsoft.com/en-us/azure/ai-services/document-intelligence/overview)

Exam reminder:

> Use Document Intelligence when the problem is document extraction, not general conversation or broad summarisation.

---

## Prebuilt and Custom Document Models

Document Intelligence supports different model types.

| Model Type       | Use Case                                                                        |
| ---------------- | ------------------------------------------------------------------------------- |
| Layout           | Extract text, tables, selection marks, and layout structure                     |
| General document | Extract common key-value pairs and document structure                           |
| Prebuilt models  | Extract known document types such as invoices, receipts, IDs, and tax documents |
| Custom models    | Extract fields from business-specific forms or documents                        |

Use prebuilt models when Microsoft already provides a model for the document type.

Use custom models when the document structure, terminology, or fields are specific to your organization.

Exam reminder:

> Prebuilt models reduce development effort for common documents. Custom models are used when the fields or formats are specific to the business.

---

## Multimodal Document Extraction

Some extraction workflows combine multiple techniques:

* OCR for scanned text
* Layout analysis for document structure
* Field extraction for specific values
* Table extraction for rows and columns
* Image analysis for visual features
* LLM reasoning for summarisation or transformation
* Search indexing for retrieval

Example:

```text
Contract PDF
    ↓
OCR extracts text
    ↓
Layout analysis identifies headings and tables
    ↓
Field extraction finds parties, dates, and obligations
    ↓
Content is converted to markdown
    ↓
Chunks are indexed in Azure AI Search
    ↓
Agent answers questions using grounded passages
```

Exam reminder:

> Multimodal extraction is useful when a document contains text, layout, tables, images, and business fields that all matter.

---

## Azure Content Understanding

Azure Content Understanding is a Foundry Tool for extracting and structuring information from multiple content types.

It can process:

* Documents
* Images
* Audio
* Video

It is useful when content is unstructured or multimodal and needs to be converted into a defined output format.

Official reference:

* [Azure Content Understanding overview](https://learn.microsoft.com/en-us/azure/ai-services/content-understanding/overview)

Use Content Understanding when:

* The input is mixed media
* The workflow needs structured outputs from unstructured content
* The solution needs extraction plus classification
* The app needs confidence scores and grounding
* The output needs to be used by agents, automation, analytics, or RAG

Exam reminder:

> Content Understanding is useful when the app needs structured outputs from documents, images, audio, or video, not just raw OCR text.

---

## Content Understanding Analyzers

Content Understanding uses analyzers to process content and return structured results.

An analyzer can be used to:

* Classify content
* Extract fields
* Generate structured outputs
* Generate markdown outputs
* Return confidence scores
* Provide grounded values for downstream reasoning

Example analyzer output target:

```json
{
  "documentType": "supplier_invoice",
  "supplierName": "Example Pty Ltd",
  "invoiceDate": "2026-05-14",
  "totalAmount": 1240.50,
  "currency": "AUD",
  "requiresReview": false
}
```

Downstream systems can use this output for:

* Search indexing
* Workflow automation
* Agent reasoning
* Reporting
* Human review queues

Exam reminder:

> Analyzers help turn unstructured content into structured or markdown outputs that agents and workflows can reason over.

---

## Clean Representations for Agents and RAG

A clean representation is the processed version of source content that is safe and useful for retrieval, generation, and auditing.

For RAG, raw extraction is often not enough.

A clean representation should include:

* Main text
* Headings
* Tables converted into readable format
* Source file name
* Page numbers
* Section names
* Metadata
* Confidence values where useful
* Security or access labels
* Links back to the original source

Example:

```text
Source: HR Leave Policy.pdf
Page: 4
Section: Carer's Leave
Content:
Employees may request carer's leave when they need to support an immediate family member...
```

Exam reminder:

> RAG works better when the model receives clean, source-linked content instead of messy OCR output.

---

## Document Intelligence vs Content Understanding vs Azure AI Search

These services often work together, but they solve different problems.

| Need                                                              | Best Fit                                |
| ----------------------------------------------------------------- | --------------------------------------- |
| Extract fields from invoices or forms                             | Document Intelligence                   |
| Extract text, tables, layout, and selection marks                 | Document Intelligence                   |
| Process mixed content such as documents, images, audio, and video | Content Understanding                   |
| Convert unstructured content into a defined schema                | Content Understanding                   |
| Generate structured or markdown outputs for reasoning             | Content Understanding                   |
| Index and retrieve content for RAG                                | Azure AI Search                         |
| Search with keyword, vector, hybrid, and semantic ranking         | Azure AI Search                         |
| Ground an LLM or agent response                                   | Azure AI Search plus RAG or agent tools |

Exam reminder:

> Document Intelligence extracts from documents. Content Understanding structures mixed content. Azure AI Search retrieves content for grounding.

---

## End-to-End Information Extraction Workflow

A complete workflow might look like this:

```text
1. Store raw files in Azure Storage.
2. Extract text, layout, fields, and structure with Document Intelligence or Content Understanding.
3. Clean and normalize the extracted content.
4. Convert complex content into readable text, markdown, or structured JSON.
5. Split content into useful chunks.
6. Add metadata, source links, and security labels.
7. Generate embeddings where vector search is required.
8. Index content in Azure AI Search.
9. Retrieve content using keyword, vector, semantic, or hybrid search.
10. Ground the model or agent response with retrieved chunks.
11. Log citations, tool calls, and user feedback.
12. Monitor ingestion quality and retrieval relevance.
```

---

## Monitoring Extraction and Retrieval Quality

Information extraction pipelines need monitoring.

Check:

* Failed files
* OCR quality
* Extraction confidence
* Missing fields
* Bad tables
* Chunking problems
* Indexer failures
* Document counts
* Search latency
* Empty search results
* Irrelevant top results
* Citation quality
* User feedback
* Retrieval drift over time

Exam reminder:

> For RAG systems, monitor both ingestion quality and retrieval quality. A healthy app with bad retrieval still produces poor answers.

---

## Security and Access Control

Information extraction often processes sensitive business data.

Important controls:

* Use managed identity where possible
* Restrict access with RBAC
* Apply document-level permissions where required
* Preserve source metadata
* Avoid indexing sensitive fields unnecessarily
* Use private endpoints for protected environments
* Log access and tool calls
* Protect raw files and extracted outputs
* Redact PII where appropriate
* Use metadata filters for security trimming

Exam reminder:

> A search index can expose sensitive information if access controls, metadata filters, and permissions are not designed properly.

---

## Chapter 5 Exam Checklist

Before moving to the appendix, make sure you can explain:

* How ingestion, extraction, indexing, retrieval, and grounding fit together
* How to ingest and index documents, images, audio, and video
* When to use Azure AI Search
* When to use vector search
* When to use hybrid search
* When to use semantic ranking
* Why AI enrichment improves raw content
* When to use built-in enrichment skills
* When to use custom enrichment skills
* When OCR is required
* How to configure a RAG ingestion flow
* How retrieval connects to workflows and agent tools
* When to use Document Intelligence
* When to use Content Understanding
* How OCR, layout analysis, and field extraction work together
* Why clean structured or markdown outputs help agents reason
* Why metadata and source links matter
* How to monitor ingestion and search quality
* How to protect sensitive indexed content

---

## Quick Exam Memory Block

```text
Extract first, then index, then retrieve, then ground.
Use OCR when content is scanned or image-based.
Use Document Intelligence for document extraction.
Use Content Understanding for structured outputs from documents, images, audio, or video.
Use Azure AI Search for retrieval and grounding.
Use vector search for meaning.
Use keyword search for exact terms.
Use hybrid search when both matter.
Use semantic ranking to improve top results.
Use built-in skills for common enrichment.
Use custom skills for business-specific enrichment.
Keep source metadata for citations and auditing.
Connect retrieval to agents as a tool when the agent needs to decide when to search.
Secure the index because indexed content can expose sensitive data.
```

---

**Previous:** [Chapter 4 — Text Analysis Solutions](./Chapter4-TextAnalysis.md)
**Home:** [README](../README.md)
**Next:** [Appendix — Resources and Exam Tips](./Appendix-Resources.md)

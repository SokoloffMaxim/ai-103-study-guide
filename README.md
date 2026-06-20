# AI-103 Study Notes

## Developing AI Apps and Agents on Azure

This repository contains my personal study notes for **Exam AI-103: Developing AI Apps and Agents on Azure**.

The goal is not to copy Microsoft Learn or create another generic certification dump. Instead, this repo is my structured way of breaking down the AI-103 objectives into practical notes, examples, design patterns, and exam-focused reminders.

AI-103 focuses on building modern AI applications and agents on Azure, especially around **Microsoft Foundry**, generative AI, retrieval-augmented generation, agentic workflows, multimodal AI, language services, search, and Responsible AI.

---

## Official Exam and Certification Links

Use these Microsoft pages as the source of truth:

* [Exam AI-103 study guide](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-103)
* [Microsoft Certified: Azure AI Apps and Agents Developer Associate](https://learn.microsoft.com/en-us/credentials/certifications/azure-ai-apps-and-agents-developer-associate/)
* [AI-103T00-A: Develop AI apps and agents on Azure](https://learn.microsoft.com/en-us/training/courses/ai-103t00)
* [Microsoft Foundry documentation](https://learn.microsoft.com/en-us/azure/foundry/)
* [What is Microsoft Foundry?](https://learn.microsoft.com/en-us/azure/foundry/what-is-foundry)
* [Develop AI agents on Azure learning path](https://learn.microsoft.com/en-us/training/paths/develop-ai-agents-azure/)

Microsoft updates Azure AI services and exam objectives regularly, so always check the official study guide before relying on any third-party notes, including this repo.

---

## Why I Created This Repo

I created this repository while preparing for AI-103 because the exam covers a wide set of topics:

* Microsoft Foundry
* Generative AI applications
* Azure AI agents
* Retrieval-augmented generation
* Azure AI Search
* Computer vision
* Language and speech services
* Document intelligence and information extraction
* Responsible AI, security, monitoring, and governance

The exam is not just about remembering service names. Many questions are likely to be scenario-based, where the important skill is choosing the right design pattern or Azure service for a specific requirement.

This repo is built around that idea.

---

## Study Map

The notes are grouped around the official AI-103 skill areas.

![AI-103 Study Guide Preview](./AI-103-StudyGuide/images/HTML-preview.png)

---

## Repository Structure

```text
AI-103-StudyGuide/
├── Chapter1-PlanManage.md
├── Chapter2-GenerativeAgents.md
├── Chapter3-ComputerVision.md
├── Chapter4-TextAnalysis.md
├── Chapter5-InformationExtraction.md
├── Appendix-Resources.md
└── images/
    └── HTML-preview.png
```

---

## Chapters

### 1. Plan and Manage an Azure AI Solution

Official exam weighting: **25–30%**

This section covers the foundation of an Azure AI solution:

* Choosing the right Azure AI service
* Planning Microsoft Foundry resources
* Selecting models and deployment options
* Managing quotas, capacity, and cost
* Applying identity and access control
* Monitoring AI applications
* Planning CI/CD for AI solutions
* Applying Responsible AI controls

File: [Chapter1-PlanManage.md](./AI-103-StudyGuide/Chapter1-PlanManage.md)

---

### 2. Implement Generative AI and Agentic Solutions

Official exam weighting: **30–35%**

This is the largest part of the exam and probably the most important area to understand deeply.

Topics include:

* Prompt engineering
* Generative AI app design
* Retrieval-augmented generation
* Grounding with enterprise data
* Function calling and tool use
* Agent design
* Multi-agent patterns
* Knowledge connections
* Fine-tuning considerations
* Evaluating and improving AI responses

File: [Chapter2-GenerativeAgents.md](./AI-103-StudyGuide/Chapter2-GenerativeAgents.md)

---

### 3. Implement Computer Vision Solutions

Official exam weighting: **10–15%**

This section covers vision and multimodal scenarios, including:

* Image analysis
* Video and visual content processing
* Multimodal model use cases
* Image generation concepts
* Visual content safety
* When to use vision models versus traditional OCR or document processing

File: [Chapter3-ComputerVision.md](./AI-103-StudyGuide/Chapter3-ComputerVision.md)

---

### 4. Implement Text Analysis Solutions

Official exam weighting: **10–15%**

This section focuses on language and speech capabilities:

* Language detection
* Sentiment analysis
* Key phrase extraction
* PII detection
* Translation
* Speech-to-text
* Text-to-speech
* Speech-enabled AI applications and agents

File: [Chapter4-TextAnalysis.md](./AI-103-StudyGuide/Chapter4-TextAnalysis.md)

---

### 5. Implement Information Extraction Solutions

Official exam weighting: **10–15%**

This section covers extracting and retrieving knowledge from documents and data sources:

* Azure AI Document Intelligence
* OCR
* Structured document extraction
* Azure AI Search
* Indexing and enrichment pipelines
* Vector search
* Hybrid search
* Semantic ranking
* Using search results to ground generative AI responses

File: [Chapter5-InformationExtraction.md](./AI-103-StudyGuide/Chapter5-InformationExtraction.md)

---

## How I Use These Notes

My suggested study order is:

1. Start with **Chapter 1** to understand the platform, management, security, and planning topics.
2. Spend the most time on **Chapter 2**, because generative AI and agents have the highest weighting.
3. Study Chapters 3, 4, and 5 as capability areas that can be added to AI apps and agents.
4. Use the appendix for final revision, links, and quick checks.
5. Revisit the official Microsoft study guide before the exam.

---

## Key AI-103 Design Questions

These are the types of questions I try to answer while studying:

* Should this solution use RAG, fine-tuning, prompt engineering, or a combination?
* Is an agent actually needed, or would a normal workflow be safer and simpler?
* What data should be indexed, chunked, embedded, and retrieved?
* How should the AI application authenticate to Azure services?
* How should tool access be restricted for an agent?
* How do I reduce hallucinations and improve grounding?
* How should I evaluate response quality and safety?
* How do cost, latency, reliability, and security affect the design?
* What Responsible AI controls are required?

---

## Responsible AI Notes

Responsible AI appears across the whole exam, not just as a separate topic.

Important areas include:

* Content filtering and safety systems
* Prompt injection protection
* Grounded responses
* Source citation and traceability
* Human review for high-risk outputs
* Secure use of tools and plugins
* Privacy and data protection
* Monitoring and auditing AI behaviour
* Evaluation of quality, safety, and reliability

For AI-103, it is important to understand not only how to build AI apps, but also how to build them safely.

---

## Useful Microsoft Learn Resources

* [Microsoft Foundry documentation](https://learn.microsoft.com/en-us/azure/foundry/)
* [Microsoft Foundry models](https://learn.microsoft.com/en-us/azure/foundry-classic/concepts/foundry-models-overview)
* [Develop AI agents on Azure](https://learn.microsoft.com/en-us/training/paths/develop-ai-agents-azure/)
* [Azure AI Search documentation](https://learn.microsoft.com/en-us/azure/search/)
* [Azure AI Document Intelligence documentation](https://learn.microsoft.com/en-us/azure/ai-services/document-intelligence/)
* [Azure AI Language documentation](https://learn.microsoft.com/en-us/azure/ai-services/language-service/)
* [Azure AI Vision documentation](https://learn.microsoft.com/en-us/azure/ai-services/computer-vision/)
* [Azure AI Speech documentation](https://learn.microsoft.com/en-us/azure/ai-services/speech-service/)
* [Azure AI Content Safety documentation](https://learn.microsoft.com/en-us/azure/ai-services/content-safety/)

---

## Disclaimer

This is an unofficial study repository.

It is based on my own notes, public Microsoft documentation, Microsoft Learn materials, and hands-on study. It is not endorsed by Microsoft and does not replace the official AI-103 exam guide.

Do not treat this repository as the only preparation source. Use it together with Microsoft Learn, official documentation, hands-on labs, and real Azure practice.

---

## Contributions

This is mainly a personal study repo, but suggestions and corrections are welcome.

If you notice outdated information, broken links, or unclear explanations, feel free to open an issue or submit a pull request.

---

## Final Note

AI-103 is not only about knowing Azure AI services. It is about understanding how to design useful, secure, grounded, and responsible AI applications.

Build small examples, compare design options, and focus on the reasoning behind each Azure AI pattern.

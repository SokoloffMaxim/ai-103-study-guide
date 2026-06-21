# Chapter 4 — Implement Text Analysis Solutions

**AI-103 exam weighting: 10–15%**

This chapter covers text, language, translation, and speech capabilities in Azure AI.

For AI-103, the main skill is choosing the right approach for the task. Some language problems are best solved with prebuilt Azure AI services. Others need generative prompting, structured outputs, agents, or a combination of tools.

Official Microsoft references:

* [AI-103 study guide](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-103)
* [Azure Language in Foundry Tools](https://learn.microsoft.com/en-us/azure/ai-services/language-service/overview)
* [Azure Translator in Foundry Tools](https://learn.microsoft.com/en-us/azure/ai-services/translator/overview)
* [Azure Speech in Foundry Tools](https://learn.microsoft.com/en-us/azure/ai-services/speech-service/overview)
* [Azure AI Content Safety](https://learn.microsoft.com/en-us/azure/ai-services/content-safety/overview)

---

## 4.1 Apply Language Model Text Analysis

Text analysis solutions process natural language to extract meaning, structure, intent, sentiment, or sensitive information.

Common tasks include:

* Detecting language
* Extracting entities
* Detecting personally identifiable information
* Analysing sentiment
* Extracting key phrases
* Summarising text
* Translating text
* Producing structured JSON
* Creating domain-specific summaries or classifications

The exam may ask whether to use a prebuilt Azure AI Language capability, Azure Translator, Azure AI Content Safety, or a generative model.

---

## Azure Language in Foundry Tools

Azure Language provides prebuilt natural language processing features for common text-analysis tasks.

Useful capabilities include:

| Capability                | Use Case                                                                       |
| ------------------------- | ------------------------------------------------------------------------------ |
| Language detection        | Identify the language of input text                                            |
| Named entity recognition  | Detect people, organizations, locations, dates, quantities, and other entities |
| PII detection             | Detect and optionally redact personally identifiable information               |
| Key phrase extraction     | Identify the main concepts in text                                             |
| Sentiment analysis        | Detect positive, neutral, negative, or mixed sentiment                         |
| Summarization             | Produce shorter versions of long text                                          |
| Text analytics for health | Extract healthcare-related entities from clinical text                         |

Exam reminder:

> If the task matches a prebuilt Azure Language feature, the prebuilt service is usually faster, cheaper, and more predictable than a general-purpose LLM.

---

## Prebuilt APIs vs LLM Prompting

A common design decision is whether to use a dedicated Azure AI service or a generative model.

| Requirement                                    | Better Option                                               |
| ---------------------------------------------- | ----------------------------------------------------------- |
| Detect language                                | Azure Language                                              |
| Detect PII                                     | Azure Language or Content Safety, depending on the scenario |
| Extract standard entities                      | Azure Language                                              |
| Translate text directly                        | Azure Translator                                            |
| Produce a flexible domain-specific summary     | LLM prompting                                               |
| Extract custom structured JSON from messy text | LLM prompting, possibly with validation                     |
| Apply strict repeatable classification         | Azure Language custom classification or fine-tuned model    |
| Combine multiple reasoning steps               | LLM or agent with tools                                     |

Simple rule:

```text id="3p1z7m"
Use prebuilt services for standard NLP tasks.
Use LLMs when the task needs flexible reasoning, custom formatting, or domain-specific interpretation.
```

---

## Entity Extraction

Entity extraction identifies important objects or concepts in text.

Examples:

```text id="g9d8m3"
Input:
Microsoft opened a new office in Sydney in 2026.

Entities:
- Microsoft: Organization
- Sydney: Location
- 2026: Date
```

Use prebuilt NER when the entity types are standard.

Use custom NER or an LLM when the entity types are domain-specific.

Examples of domain-specific entities:

* Contract clauses
* Product codes
* Chemical names
* Internal project names
* Compliance obligations
* Medical or legal terminology

Exam reminder:

> Prebuilt NER is good for common entity types. Custom extraction is needed when the schema belongs to your business domain.

---

## PII Detection and Redaction

PII detection finds personal information in text.

Examples:

* Names
* Email addresses
* Phone numbers
* Addresses
* Account numbers
* Identity numbers

A typical PII workflow:

```text id="xzvk10"
User text
    ↓
Detect PII
    ↓
Redact or mask sensitive values
    ↓
Send safe text to downstream app, model, or agent
```

Use PII detection before sending sensitive text to a generative model when the app does not need the raw values.

Exam reminder:

> PII detection is not only for compliance. It also reduces unnecessary exposure of personal data to models, logs, tools, and downstream systems.

---

## Sentiment, Tone, and Safety

Sentiment analysis helps detect whether text is positive, neutral, negative, or mixed.

Content Safety is used when the issue is not sentiment but harm, risk, or policy violation.

| Need                                                  | Use                     |
| ----------------------------------------------------- | ----------------------- |
| Is the customer happy or unhappy?                     | Sentiment analysis      |
| Does this text contain harmful or unsafe content?     | Azure AI Content Safety |
| Is the user trying to manipulate the model?           | Prompt Shields          |
| Is the generated answer grounded in sources?          | Groundedness detection  |
| Does the text contain sensitive personal information? | PII detection           |

Exam reminder:

> Sentiment and safety are different. Negative sentiment is not automatically unsafe content.

---

## Structured JSON Outputs

Generative models are useful when text needs to be converted into a custom structure.

Example:

```json id="5sneec"
{
  "customerIntent": "cancel_subscription",
  "urgency": "high",
  "mentionedProducts": ["mobile plan"],
  "requiresHumanReview": true
}
```

Good practices:

* Clearly define the JSON schema
* Ask for only JSON when the next system expects JSON
* Validate the output in code
* Retry or reject invalid output
* Avoid trusting generated fields without validation

Exam reminder:

> LLM-generated JSON should still be validated. The model can produce malformed or incomplete output.

---

## Translation

Azure Translator is the best default choice for direct translation tasks.

Use Azure Translator when you need:

* Fast translation
* Broad language support
* Document translation
* Consistent production translation
* Translation APIs with predictable behaviour

Use an LLM-powered translation flow when translation is part of a larger reasoning task.

Examples:

* Translate and summarise
* Translate and rewrite for tone
* Translate and extract action items
* Translate and explain cultural context
* Translate and classify

Exam reminder:

> Use Azure Translator for direct translation. Use an LLM when translation must be combined with reasoning, rewriting, or summarisation.

Official reference:

* [Azure Translator in Foundry Tools](https://learn.microsoft.com/en-us/azure/ai-services/translator/overview)

---

## Domain-Specific Text Analysis

Some text analysis tasks require domain knowledge.

Examples:

* Compliance summary
* Legal clause extraction
* Incident report classification
* Medical note extraction
* Technical support triage
* Financial document summarisation

Possible approaches:

| Approach                    | Use When                                               |
| --------------------------- | ------------------------------------------------------ |
| Few-shot prompting          | You need quick domain adaptation with examples         |
| Structured output prompting | You need JSON or field extraction                      |
| Custom NER                  | You need consistent domain entity extraction           |
| Custom text classification  | You need repeatable classification labels              |
| Fine-tuning                 | You need repeated domain behaviour at scale            |
| RAG                         | The model needs current or private reference knowledge |

Exam reminder:

> If the model lacks knowledge, use RAG. If the model behaves inconsistently on a repeated task, consider fine-tuning or custom classification.

---

## MCP and Agent Integration

Azure Language capabilities can be exposed to agents through tools, including MCP-based integrations.

This allows an agent to call language capabilities such as:

* Detect language
* Extract entities
* Detect PII
* Redact sensitive text
* Analyse sentiment
* Process text before another action

Example agent pattern:

```text id="s3u6sw"
User submits support message
    ↓
Agent detects language
    ↓
Agent redacts PII
    ↓
Agent classifies intent
    ↓
Agent creates a draft support ticket
```

Exam reminder:

> Agents should not do every task themselves. Let agents call reliable tools for language detection, PII, translation, and other repeatable NLP work.

---

## 4.2 Implement Speech Solutions

Speech solutions allow AI applications and agents to work with spoken language.

The exam includes:

* Speech-to-text
* Text-to-speech
* Speech translation
* Speech-enabled agents
* Custom speech models
* Multimodal reasoning from audio inputs

---

## Speech-to-Text

Speech-to-text converts audio into text.

Use cases:

* Voice commands
* Meeting transcription
* Call center analytics
* Audio search
* Voice-enabled agents
* Captions and subtitles
* Compliance recording analysis

Common modes:

| Mode                    | Use Case                                        |
| ----------------------- | ----------------------------------------------- |
| Real-time transcription | Live audio or streaming conversations           |
| Fast transcription      | Pre-recorded files where speed matters          |
| Batch transcription     | Large volumes of audio processed asynchronously |

Exam reminder:

> Use real-time transcription for live apps. Use batch transcription for large offline workloads.

Official reference:

* [Azure Speech in Foundry Tools](https://learn.microsoft.com/en-us/azure/ai-services/speech-service/overview)

---

## Text-to-Speech

Text-to-speech converts written text into spoken audio.

Use cases:

* Voice agents
* Accessibility
* Narration
* IVR systems
* Training content
* Multilingual customer experiences

Text-to-speech can use neural voices and SSML.

SSML can control:

* Pronunciation
* Speaking rate
* Pitch
* Volume
* Pauses
* Emphasis

Exam reminder:

> Use SSML when the app needs more control over how speech sounds.

---

## Speech Translation

Speech translation converts spoken language into another language.

Common patterns:

```text id="exdzg8"
Speech input
    ↓
Speech-to-text
    ↓
Translate text
    ↓
Optional text-to-speech output
```

Azure Speech also provides speech translation capabilities for real-time multilingual scenarios.

Use cases:

* Live multilingual meetings
* Customer support
* Travel and accessibility apps
* Multilingual voice agents
* Subtitles and captions

Exam reminder:

> For multilingual voice workflows, think in stages: transcribe, translate, then optionally synthesize speech.

---

## LLM Speech

Azure Speech also includes LLM-enhanced speech capabilities in preview.

These can support tasks such as:

* Transcribing pre-recorded audio
* Translating pre-recorded audio into target-language text
* Improving contextual understanding
* Producing better captions or meeting notes

Because preview features can change, check current Microsoft documentation before relying on them in a production design.

Exam reminder:

> Preview features may appear in exam scenarios if they are commonly used, but for production architecture always check availability, region support, and support status.

---

## Custom Speech Models

Custom speech models improve recognition accuracy for specific audio conditions or vocabulary.

Use custom speech when the base model struggles with:

* Company names
* Product names
* Technical jargon
* Industry terms
* Accents
* Noisy audio
* Specialized vocabulary

Examples:

```text id="br5g8n"
The model keeps mishearing internal project names during meeting transcription.
```

```text id="dw3fh9"
A healthcare call center needs better transcription of medical terms.
```

Exam reminder:

> Use custom speech when transcription accuracy is poor because of domain vocabulary, pronunciation, noise, or accent patterns.

---

## Speech as an Agent Modality

Speech can be used as an input and output channel for agents.

A speech-enabled agent may use this flow:

```text id="b2m42n"
User speaks
    ↓
Speech-to-text
    ↓
Agent interprets intent
    ↓
Agent retrieves knowledge or calls tools
    ↓
Agent produces response
    ↓
Text-to-speech
    ↓
User hears the answer
```

Design considerations:

* Latency
* Interruptions
* Turn-taking
* Background noise
* Language identification
* Safety filtering
* Logging and transcripts
* Human handoff
* Privacy and consent

Exam reminder:

> Speech agents are not just chatbots with audio. They need careful handling of latency, turn-taking, transcription errors, and user confirmation.

---

## Voice Live

Voice Live is designed for natural, real-time conversational experiences between users and agent implementations.

Use it when the application needs:

* Low-latency voice interaction
* Natural spoken conversation
* Agent-like dialogue
* Real-time response behaviour

Official reference:

* [Azure Speech in Foundry Tools](https://learn.microsoft.com/en-us/azure/ai-services/speech-service/overview)

Exam reminder:

> Use real-time voice capabilities for live conversational agents. Use batch transcription for offline audio processing.

---

## Responsible AI for Text and Speech

Text and speech systems can process sensitive personal data, private conversations, and harmful content.

Important controls:

* Detect and redact PII
* Moderate unsafe content
* Protect against prompt injection
* Log only what is necessary
* Avoid storing raw audio unless required
* Inform users when conversations are recorded or transcribed
* Require consent where appropriate
* Protect transcripts with access control
* Use human review for high-risk decisions

For voice agents, add extra controls:

* Confirm sensitive actions before execution
* Repeat critical details back to the user
* Handle transcription uncertainty
* Provide fallback to a human
* Avoid making irreversible changes from a misunderstood voice command

Exam reminder:

> Speech adds extra risk because transcription errors can become action errors.

---

## Chapter 4 Exam Checklist

Before moving to Chapter 5, make sure you can explain:

* When to use Azure Language instead of an LLM
* When to use LLM prompting for structured text analysis
* How entity extraction, PII detection, sentiment, and safety detection differ
* When to use Azure Translator
* When LLM-powered translation is useful
* How to generate and validate structured JSON output
* How agents can use language tools through tool integration
* How speech-to-text and text-to-speech fit into an agent workflow
* When to use real-time, fast, or batch transcription
* When custom speech models are useful
* How speech translation works
* What risks apply to text and speech data

---

## Quick Exam Memory Block

```text id="y8bjzt"
Use Azure Language for standard NLP tasks.
Use PII detection before exposing sensitive text to models, logs, or tools.
Use Azure Translator for direct translation.
Use LLMs when translation or extraction needs reasoning or custom formatting.
Validate structured JSON generated by models.
Use speech-to-text for voice input.
Use text-to-speech for voice output.
Use batch transcription for large offline audio workloads.
Use custom speech for domain vocabulary, accents, or noisy audio.
For speech agents, confirm sensitive actions before execution.
```

---

**Previous:** [Chapter 3 — Computer Vision Solutions](./Chapter3-ComputerVision.md)
**Home:** [README](../README.md)
**Next:** [Chapter 5 — Information Extraction Solutions](./Chapter5-InformationExtraction.md)
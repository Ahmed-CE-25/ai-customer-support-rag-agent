# AI Customer Support RAG Agent

An intelligent, context-aware AI Support Agent that securely retrieves company-specific knowledge to answer incoming customer questions with 100% factual accuracy. Built using Retrieval-Augmented Generation (RAG) to eliminate AI hallucinations.

## 💡 The Problem
Standard AI models are prone to "hallucinations"—making up fake pricing, policies, or information when talking to users. Businesses need the automation power of AI, but require a strict guarantee that the model will only speak on verified company facts without leaking internal general knowledge.

## ⚙️ Architecture & How It Works

This system is built using a highly scalable, split-workflow architecture separated into an independent **Ingestion Pipeline** and a **Retrieval Engine**.

### 1. The Knowledge Ingestion Pipeline (`RAG-Loader.json`)
- **Document Chunking:** This background workflow ingests corporate unstructured data documents (PDFs/Markdown/Text).
- **Embedding Generation:** It processes text segments sequentially, converting raw strings into high-dimensional vector embeddings using an embedding model.
- **Upserting to Vector Database:** The final multi-dimensional vector embeddings are systematically indexed into a `Pinecone Vector Store` with customized metadata payloads.

### 2. The Live Retrieval Engine (`RAG-Answerer.json`)
- **User Inquiry:** A user submits a question through a custom web interface (`n8n Form Trigger`).
- **Semantic Search:** The system vectorizes the text prompt and executes a top-k similarity query against the compiled Pinecone database index.
- **Context Injection & Inference:** The closest text chunks are extracted and passed as a verified context envelope alongside the prompt to `Gemini 2.5 Flash`.
- **Response Delivery:** Gemini synthesizes a factual, brand-safe output which is immediately piped back onto the user's interface using an `n8n Form Ending` node.

## 🛠️ Tech Stack
- **Orchestration:** n8n Workflow Automation
- **LLM Engine:** Google Gemini 2.5 Flash API
- **Vector Database:** Pinecone
- **Data Format:** JSON

## 📋 What I'd Customize for a Real Client
- **Ingestion Pipelines:** Build an automated background sync workflow that updates the Pinecone index whenever new PDFs or pages are updated in a client's Google Drive or Notion workspace.
- **Omnichannel UI:** Swap the web form trigger for direct API webhooks into WhatsApp Business, Slack, or an on-site chat widget (e.g., Typebot).
- **Human Hand-off:** Insert an conditional evaluation router. If the AI responds with its fallback *"I don't have that info"* phrase, automatically escalate the session to a live agent queue or open a ticket inside a CRM like Hubspot.

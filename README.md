# AI Customer Support RAG Agent

An intelligent, context-aware AI Support Agent that securely retrieves company-specific knowledge to answer incoming customer questions with 100% factual accuracy. Built using Retrieval-Augmented Generation (RAG) to eliminate AI hallucinations.

## 💡 The Problem
Standard AI models are prone to "hallucinations"—making up fake pricing, policies, or information when talking to users. Businesses need the automation power of AI, but require a strict guarantee that the model will only speak on verified company facts without leaking internal general knowledge.

## ⚙️ How It Works
1. **User Inquiry:** A customer submits a question through a custom web interface (`n8n Form Trigger`).
2. **Knowledge Retrieval:** The system vectorizes the question and queries a `Pinecone Vector Store` index hosting the company's official knowledge base documents.
3. **Contextual Injection:** The most relevant document chunks are extracted along with their original source metadata.
4. **Brand-Safe Processing:** A targeted prompt template passes both the user's question and the raw documentation context to `Gemini 2.5 Flash`.
5. **Dynamic Response:** Gemini synthesizes the exact answer instantly. If the required information isn't present in the documents, a fallback rule forces the agent to safely state: *"I don't have that information."*
6. **Interface Resolution:** The generated text is passed back sequentially via an `n8n Form Ending` node to display seamlessly onto the customer's screen.

## 🛠️ Tech Stack
- **Orchestration:** n8n Workflow Automation
- **LLM Engine:** Google Gemini 2.5 Flash API
- **Vector Database:** Pinecone
- **Data Format:** JSON

## 📋 What I'd Customize for a Real Client
- **Ingestion Pipelines:** Build an automated background sync workflow that updates the Pinecone index whenever new PDFs or pages are updated in a client's Google Drive or Notion workspace.
- **Omnichannel UI:** Swap the web form trigger for direct API webhooks into WhatsApp Business, Slack, or an on-site chat widget (e.g., Typebot).
- **Human Hand-off:** Insert an conditional evaluation router. If the AI responds with its fallback *"I don't have that info"* phrase, automatically escalate the session to a live agent queue or open a ticket inside a CRM like Hubspot.

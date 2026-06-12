Typesense v29.0+ provides built-in orchestration for Retrieval-Augmented Generation (RAG), automatically managing context windows, retrieving documents, and communicating with external LLMs.

You need to write a setup script that initializes a system `conversation_store` collection and maps a primary document collection to an external OpenAI LLM configuration. Once configured, execute a conversational search asking "What are the return policies?" and output the generated natural language response and conversation ID to `rag_output.json`.

**Constraints:**
- Must utilize the built-in Conversational Search API (`/collections/{collection}/documents/search`).
- Provide the OpenAI API key securely via an environment variable; do NOT store it in the Typesense schema long-term if possible.
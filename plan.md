# Typesense Benchmark Research Plan

This research plan covers Typesense (v29.0+) and Typesense Cloud, focusing on its core search capabilities, AI-driven features (Vector Search, RAG), and managed infrastructure.

## 1. Library Overview

*   **Description**: Typesense is a fast, open-source, in-memory search engine designed for performance and developer productivity. It serves as a modern alternative to Algolia and Elasticsearch, focusing on "sane defaults" and a simplified API.
*   **Ecosystem Role**: It is commonly used for site search, e-commerce product discovery, and increasingly as a vector database for RAG (Retrieval-Augmented Generation) applications.
*   **Project Setup**:
    *   **Docker (Local)**:
        ```bash
        docker run -p 8108:8108 -v/tmp/data:/data typesense/typesense:29.0 \
          --data-dir /data --api-key=xyz --enable-cors
        ```
    *   **Typesense Cloud**: Managed clusters with High Availability (HA) and a Search Delivery Network (SDN). Setup involves provisioning a cluster via the UI/API and generating Scoped or Admin API Keys.
    *   **Clients**: Official libraries for JavaScript, Python, PHP, Ruby, Go, and more.

## 2. Core Primitives & APIs

*   **Collections**: Schematized (or auto-detected) containers for documents.
    *   [Documentation](https://typesense.org/docs/latest/api/collections.html)
    *   ```javascript
        const schema = {
          'name': 'products',
          'fields': [
            {'name': 'name', 'type': 'string'},
            {'name': 'price', 'type': 'float'},
            {'name': 'embedding', 'type': 'float[]', 'num_dim': 384} // For Vector Search
          ]
        };
        await client.collections().create(schema);
        ```
*   **Documents**: CRUD operations and high-performance bulk imports.
    *   [Documentation](https://typesense.org/docs/latest/api/documents.html)
*   **Search**: Core engine supporting keyword, prefix, typo-tolerance, and filtering.
    *   [Documentation](https://typesense.org/docs/latest/api/search.html)
    *   ```javascript
        client.collections('products').documents().search({
          'q': 'red shirts',
          'query_by': 'name,description',
          'filter_by': 'price:<50',
          'sort_by': 'price:asc'
        });
        ```
*   **Vector Search & Hybrid Search**: Nearest-neighbor search with optional keyword boosting.
    *   [Documentation](https://typesense.org/docs/latest/api/vector-search.html)
*   **Conversational Search (RAG)**: Built-in orchestration for LLM-based Q&A using indexed data.
    *   [Documentation](https://typesense.org/docs/latest/api/conversational-search-rag.html)
*   **Natural Language Search**: Uses LLMs to parse natural language queries into structured Typesense filters.
    *   [Documentation](https://typesense.org/docs/latest/api/natural-language-search.html)

## 3. Real-World Use Cases & Templates

*   **E-commerce**: Instant search with facets (categories, price ranges) and sorting.
*   **SaaS Search**: Multi-tenant search using **Scoped API Keys** to isolate user data.
*   **AI Knowledge Base**: Using Typesense as a vector store for RAG pipelines (e.g., with OpenAI or Gemini).
*   **Integrations**:
    *   [Firebase Extension](https://firebase.google.com/products/extensions/typesense-firestore-typesense-search): Automatic sync from Firestore.
    *   [Laravel Scout](https://laravel.com/docs/11.x/scout#typesense): First-class driver for PHP/Laravel.

## 4. Developer Friction Points

*   **Memory Management**: Since Typesense is in-memory, users often struggle with `OUT_OF_MEMORY` errors if the dataset size exceeds 2x-3x the available RAM. [Issue #705](https://github.com/typesense/typesense/issues/705).
*   **Schema Evolution**: Changing a field type requires creating a new collection and re-indexing data. The recommended pattern is using **Aliases** to avoid downtime. [Docs](https://typesense.org/docs/guide/running-in-production.html#schema-management).
*   **Bulk Import Tuning**: Large imports can cause write amplification or disk bottlenecks if batch sizes aren't tuned to CPU cores. [Issue #2312](https://github.com/typesense/typesense/issues/2312).

## 5. Evaluation Ideas

1.  **Basic Search Implementation**: Create a collection, import a JSON dataset, and implement a search with typo-tolerance and facets.
2.  **Zero-Downtime Schema Update**: Implement a script that re-indexes data into a new collection and swaps an alias without dropping requests.
3.  **Multi-Tenant Security**: Generate Scoped API Keys that restrict a user's search results to only documents they own.
4.  **Hybrid AI Search**: Configure a collection with vector embeddings and perform a search that combines semantic similarity with keyword filtering.
5.  **Built-in RAG Pipeline**: Set up a conversational search endpoint using the `conversation_store` schema and an LLM configuration.
6.  **Natural Language to Filter**: Implement a search that uses `nl_query: true` to transform "show me blue items under $20" into structured filters.
7.  **Geosearch & Sorting**: Implement a "find nearby" feature using latitude/longitude fields and sorting by distance.

## 6. Sources

1.  [Typesense Official Documentation](https://typesense.org/docs/): Primary source for API and Guide.
2.  [Typesense Cloud Dashboard & Features](https://cloud.typesense.org/): Details on SDN, HA, and managed services.
3.  [Typesense GitHub Repository](https://github.com/typesense/typesense): Source for issues, memory discussions, and roadmap.
4.  [Vector Search API Reference](https://typesense.org/docs/latest/api/vector-search.html): Technical details on HNSW indexing.
5.  [RAG & Conversational Search Guide](https://typesense.org/docs/latest/api/conversational-search-rag.html): Built-in LLM orchestration details.
6.  [System Requirements & RAM Estimation](https://typesense.org/docs/guide/system-requirements.html): Memory usage guidelines.
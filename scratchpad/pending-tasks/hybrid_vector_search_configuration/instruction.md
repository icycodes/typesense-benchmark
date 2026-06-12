Combining traditional keyword search with nearest-neighbor vector search improves result relevance by factoring in both exact matches and semantic similarity.

You need to create a Typesense collection schema that includes an `embedding` field configured for a 384-dimensional vector. After importing the provided mock dataset containing pre-computed vectors, execute a hybrid search request for "ergonomic chair" that utilizes both keyword boosting and vector similarity, outputting the top 5 document IDs to `hybrid_results.json`.

**Constraints:**
- The schema MUST explicitly define the `embedding` field with `type: float[]` and `num_dim: 384`.
- The search request MUST include both the `q` (keyword) and `vector_query` parameters simultaneously.
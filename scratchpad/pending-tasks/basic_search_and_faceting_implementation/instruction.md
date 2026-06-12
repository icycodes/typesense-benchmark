E-commerce product discovery requires robust search with typo tolerance, sorting, and faceting capabilities.

You need to write a Node.js script that initializes a Typesense client, creates a `products` collection, bulk imports a provided `products.json` file, and executes a search query for "red shirts". The search must be typo-tolerant, filter by `price:<50`, sort by `price:asc`, and output the resulting payload to `results.json`.

**Constraints:**
- Must use the official Typesense JavaScript client.
- Do NOT hardcode the API key; it must be read from the `TYPESENSE_API_KEY` environment variable.
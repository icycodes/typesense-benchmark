SaaS applications must securely restrict search results to documents owned by the active user without creating a separate collection for every single tenant.

You need to generate a Scoped API Key for a user with the tenant identifier `123`. Your script must take a highly restricted Parent Search Key and embed a mandatory `filter_by: tenant_id:=123` constraint into the new Scoped API Key, saving the generated token string to `scoped_key.txt`.

**Constraints:**
- Do NOT modify the collection schema or create a new collection.
- The Scoped API Key must be generated entirely client-side using the parent key without making a network request to the Typesense server.
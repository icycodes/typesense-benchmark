Typesense can utilize LLMs to dynamically parse natural language inputs from users and translate them into structured search filters.

You need to execute a search query using the Typesense client for the phrase "show me blue items under $20". You must enable the natural language parsing feature in the API request so the engine automatically translates the string into structured Typesense filters (e.g., `price:<20` and `color:blue`) before fetching the documents.

**Constraints:**
- The `nl_query: true` parameter MUST be passed in the search request.
- Save the raw Typesense JSON response, which includes the automatically applied filters in the metadata, to `nl_response.json`.
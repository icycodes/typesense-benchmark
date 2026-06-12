Because Typesense is an in-memory datastore, changing an existing field type requires creating a new collection and re-indexing the data. 

You need to write a Python script that implements a zero-downtime schema update using Collection Aliases. The script must create a `users_v2` collection with an updated schema, copy all documents from `users_v1`, update the `users` alias to point to `users_v2` atomically, and safely drop `users_v1`.

**Constraints:**
- The alias `users` MUST be updated without dropping incoming search requests.
- Must use the official `typesense` Python SDK.
---
description: Generate a standard Neo4j database connection module for the current project's language/stack
---

# Neo4j Connector Skill

When the user wants to connect a project to Neo4j:

1. Detect the project's language (check package.json, requirements.txt, pom.xml, etc.)
2. Check ./templates/ for the matching language's connection pattern
3. Generate a connection module with:
   - Driver initialization using env vars (NEO4J_URI, NEO4J_USER, NEO4J_PASSWORD)
   - Connection pooling / session management
   - A basic `run_query(query, params)` wrapper
   - Graceful close/teardown handling
4. Never hardcode credentials — always read from environment or config
5. Add the new dependency (neo4j driver package) to the project's dependency file if missing
6. Confirm the file location with the user before writing, matching existing project structure
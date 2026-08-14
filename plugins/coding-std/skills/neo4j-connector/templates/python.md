import os
from neo4j import GraphDatabase

class Neo4jConnection:
    def __init__(self):
        self._driver = GraphDatabase.driver(
            os.environ["NEO4J_URI"],
            auth=(os.environ["NEO4J_USER"], os.environ["NEO4J_PASSWORD"]),
        )

    def close(self):
        self._driver.close()

    def run_query(self, query: str, params: dict = None):
        with self._driver.session() as session:
            return list(session.run(query, params or {}))
            
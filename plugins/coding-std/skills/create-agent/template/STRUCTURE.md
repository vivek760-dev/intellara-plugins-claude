agentic-platform/
app/
├── api/
│   └── v1/
│       ├── chat.py
│       ├── agent.py
│       ├── workflow.py
│       ├── documents.py
│       ├── health.py
│       └── auth.py
├── middleware/
├── dependencies/
├── routers.py
│
├── core/
│   ├── config.py
│   ├── logging.py
│   ├── security.py
│   ├── constants.py
│   ├── exceptions.py
│   ├── metrics.py
│   ├── tracing.py
│   └── startup.py
│
├── orchestrator/
│   ├── planner.py
│   ├── executor.py
│   ├── router.py
│   ├── workflow_engine.py
│   ├── state_manager.py
│   └── execution_context.py
│
├── agents/
│   ├── base/
│   │   ├── base_agent.py
│   │   ├── agent_context.py
│   │   └── agent_result.py
│   ├── finance/
│   ├── hr/
│   ├── risk/
│   ├── coding/
│   ├── retrieval/
│   └── planning/
│
├── tools/
│   ├── base/
│   ├── database/
│   ├── search/
│   ├── rag/
│   ├── email/
│   ├── filesystem/
│   ├── browser/
│   ├── calculator/
│   └── custom/
│
├── memory/
│   ├── short_term/
│   ├── long_term/
│   ├── episodic/
│   ├── semantic/
│   ├── user_profile/
│   ├── retrieval.py
│   └── manager.py
│
├── knowledge/
│   ├── ingestion/
│   ├── chunking/
│   ├── embeddings/
│   ├── vectorstores/
│   ├── rerankers/
│   ├── retrievers/
│   └── citation.py
│
├── llm/
│   ├── providers/
│   │   ├── openai.py
│   │   ├── anthropic.py
│   │   ├── bedrock.py
│   │   ├── groq.py
│   │   └── ollama.py
│   ├── model_router.py
│   ├── prompt_builder.py
│   ├── prompt_templates/
│   ├── structured_output.py
│   ├── tokenizer.py
│   └── cache.py
│
├── workflows/
│   ├── onboarding/
│   ├── claim_processing/
│   ├── approval/
│   ├── document_analysis/
│   └── research/
│
├── services/
│   ├── conversation_service.py
│   ├── document_service.py
│   ├── memory_service.py
│   ├── agent_service.py
│   └── workflow_service.py
│
├── repositories/
│   ├── mongo/
│   ├── postgres/
│   ├── vector_db/
│   └── redis/
│
├── integrations/
│   ├── jira/
│   ├── slack/
│   ├── teams/
│   ├── sharepoint/
│   ├── confluence/
│   └── sap/
│
├── events/
│   ├── publisher.py
│   ├── subscriber.py
│   └── schemas.py
│
├── schemas/
├── utils/
└── main.py


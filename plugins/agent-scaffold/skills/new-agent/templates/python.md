# Reference shape — domain agent (Python)

> This is a **reference for the general shape only**. Always read the project's
> real `app/agents/base/base_agent.py`, `agent_context.py`, and `agent_result.py`
> and match those. If this template and the project disagree, the project wins.

```python
from app.agents.base.base_agent import BaseAgent
from app.agents.base.agent_context import AgentContext
from app.agents.base.agent_result import AgentResult
from app.core.constants import AgentName
from app.core.logging import get_logger

logger = get_logger(__name__)


class LegalAgent(BaseAgent):
    """Handles contract review and clause extraction questions."""

    name = AgentName.LEGAL

    def __init__(self, llm, tools=None):
        super().__init__(llm=llm, tools=tools or [])

    async def run(self, context: AgentContext) -> AgentResult:
        try:
            response = await self.llm.complete(
                self.build_prompt(context),
                context=context,
            )
            return AgentResult.ok(
                agent=self.name,
                output=response.text,
                citations=response.citations,
            )
        except Exception as exc:
            logger.exception("legal agent failed")
            return AgentResult.error(agent=self.name, message=str(exc))
```

## Registration — the step people forget

```python
# app/orchestrator/router.py
from app.agents.legal.legal_agent import LegalAgent

AGENT_REGISTRY = {
    AgentName.FINANCE: FinanceAgent,
    AgentName.HR:      HRAgent,
    AgentName.LEGAL:   LegalAgent,   # <-- new
}
```

```python
# app/core/constants.py
class AgentName(str, Enum):
    FINANCE = "finance"
    HR      = "hr"
    LEGAL   = "legal"   # <-- new
```

## Checklist

- [ ] Inherits the project's base agent
- [ ] Accepts the project's context type, returns the project's result type
- [ ] Errors return a result — they do not escape into the orchestrator
- [ ] Registered in `orchestrator/router.py`
- [ ] Identifier added to `core/constants.py`
- [ ] Tools wired from `app/tools/`, not reimplemented
- [ ] No credentials or model names hardcoded
- [ ] Test covering one success and one failure path

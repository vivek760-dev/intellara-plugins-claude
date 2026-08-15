# Reference shape — agent tool (Python)

> This is a **reference for the general shape only**. Always read the project's
> real `app/tools/base/` and match it. If this template and the project
> disagree, the project wins.

```python
import asyncio

from app.tools.base.base_tool import BaseTool, ToolResult
from app.core.config import settings
from app.core.logging import get_logger

logger = get_logger(__name__)


class SearchInvoicesTool(BaseTool):
    name = "search_invoices"

    # Read by the model at runtime. This IS a prompt — say when to call it,
    # and when not to.
    description = (
        "Search the invoice database by vendor, date range, or amount. "
        "Call this when the user asks about specific invoices, payment status, "
        "or vendor spend. Do NOT call this for budget forecasts or general "
        "financial policy questions — use search_documents for those."
    )

    parameters = {
        "type": "object",
        "properties": {
            "vendor": {
                "type": "string",
                "description": "Vendor name, exact or partial match.",
            },
            "start_date": {
                "type": "string",
                "description": "Inclusive start date, ISO 8601 (YYYY-MM-DD).",
            },
            "end_date": {
                "type": "string",
                "description": "Inclusive end date, ISO 8601 (YYYY-MM-DD).",
            },
            "max_results": {
                "type": "integer",
                "description": "Maximum rows to return. Defaults to 20.",
            },
        },
        "required": ["vendor"],
    }

    async def run(self, vendor: str, start_date=None, end_date=None,
                  max_results: int = 20) -> ToolResult:
        try:
            rows = await asyncio.wait_for(
                self.repo.find_invoices(vendor, start_date, end_date, max_results),
                timeout=settings.TOOL_TIMEOUT_SECONDS,
            )
            return ToolResult.ok(data=rows)

        except asyncio.TimeoutError:
            # Returned, not raised — the agent needs to read this and adapt.
            return ToolResult.error(
                message="Invoice search timed out. Try a narrower date range."
            )
        except Exception as exc:
            logger.exception("search_invoices failed")
            return ToolResult.error(message=str(exc))
```

## Registration — the step people forget

```python
# app/tools/base/registry.py
from app.tools.database.search_invoices import SearchInvoicesTool

TOOL_REGISTRY = {
    SearchInvoicesTool.name: SearchInvoicesTool,   # <-- new
}
```

## Writing the description — the part that actually decides if it works

| ❌ Weak | ✅ Strong |
|---|---|
| "Searches invoices." | "Search the invoice database by vendor, date range, or amount." |
| No trigger condition | "Call this when the user asks about specific invoices, payment status, or vendor spend." |
| No boundary | "Do NOT call this for budget forecasts — use search_documents for those." |

A weak description fails silently. The tool is never called and nothing errors.

## Checklist

- [ ] Lives in the right category folder, not `custom/` by default
- [ ] Name is `verb_noun` and unambiguous against existing tools
- [ ] Description says what, when to call, and when not to
- [ ] Every parameter has a type, description, and required/optional marker
- [ ] Failures return a result — they never raise into the agent loop
- [ ] Timeout on every network / database / browser call
- [ ] Registered in the tool registry
- [ ] Narrowest credentials that do the job
- [ ] Test covering success **and** failure

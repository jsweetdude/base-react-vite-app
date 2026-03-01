# A11y Context Project — MCP Router (Fast + Reproducible)

You have access to MCP server `a11y-context` with tools:
- `list_patterns`
- `get_pattern`
- `get_global_rules`

Tool signatures (use these exact argument names and default stack):
- `list_patterns({ stack: "web/react", tags?: string[], query?: string })`
- `get_pattern({ stack: "web/react", id: string })`
- `get_global_rules({ stack: "web/react" })`

Hard requirements (do not skip):
1. **Stack resolution:** Default to `stack = "web/react"` for all MCP calls. Only use a different stack if the user explicitly requests it. If a different stack is requested and unsupported, state the limitation and proceed with `web/react` equivalents (or stop if the task cannot be adapted).
2. **No Bash MCP validation:** Do not use Bash to validate MCP connectivity (no curl to `/mcp`, no MCP inspector CLI, no running local server files). Use MCP tools only. If an MCP tool call fails, report the error and stop.
3. **Component Inventory first:** Before any MCP calls, output a short Component Inventory (max 8 bullets). Include page shell/layout as a unit if applicable.
4. **Catalog caching:** Call `list_patterns` at most once per chat per `catalog_revision`. If a `list_patterns` result with `catalog_revision` already exists earlier in this chat, reuse it and do not call `list_patterns` again.
5. **Selection discipline:** For each unit you will implement, select the best pattern using only the cached `list_patterns` output:
   - Quote `selection_excerpt` to justify the match.
   - Explicitly reject candidates where `do_not_use_when` applies.
   - Do not fetch patterns for units you are not implementing.
6. **Pattern fetch:** Call `get_pattern` only for chosen patterns you will implement, and at most once per chosen pattern per chat (reuse the guidance afterward).
7. **Global rules:** Call `get_global_rules` at most once per chat (preflight if structural: page shell/headings/landmarks/focus-critical UI; otherwise postflight). Apply applicable MUST rules to the final code.
8. **No unrelated changes:** Follow golden pattern guidance and avoid unrelated refactors outside requested units.

Required response format:
A. Component Inventory  
B. MCP Plan (tool calls in order; note reuse vs call for `list_patterns`/`get_global_rules`)  
C. Execute tools  
D. Selection Decisions (include `selection_excerpt` quotes + `do_not_use_when` rejections)  
E. Code  
F. Global Rules Application (max 5 bullets)  
G. Pattern Coverage (one line per unit: pattern id or “native + global rules”)  
H. Self-check (confirm requirements 1–8 were met)
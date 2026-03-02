---
description: A11y Context MCP Router. Enforce MCP-first best-practice retrieval, deterministic tool usage, caching, styling constraints, and global-rules compliance.
applyTo: "**/*.{ts,tsx,js,jsx,html,css,scss}"
---

# A11y Context Project — MCP Router (Fast + Reproducible)

## Core Mandate

Before generating final front-end code, retrieve and apply relevant accessibility best practices from MCP (patterns + global rules).

## MCP Tools

You have access to MCP server `a11y-context` with tools:
- `list_patterns({ stack: "web/react", tags?: string[], query?: string })`
- `get_pattern({ stack: "web/react", id: string })`
- `get_global_rules({ stack: "web/react" })`

## Platform Parameters

- Default to `stack = "web/react"` for all MCP calls.
- Only use a different stack if the user explicitly requests it.
- If a different stack is requested and unsupported, state the limitation and proceed with `web/react` equivalents (or stop if the task cannot be adapted).

## Order of Operations

1. Ensure a cached `list_patterns` catalog exists for this chat (call once if needed).
2. For each component you will implement: select via `selection_excerpt`, reject `do_not_use_when`, then call `get_pattern` once and cache.
3. If the task includes page/layout/landmarks/headings or focus-critical UI: call `get_global_rules` once and cache.
4. Apply retrieved guidance, then produce final code.

## Guardrails

### MCP-First Enforcement
- Do not generate final code until relevant MCP guidance has been retrieved (or you explicitly state that no pattern/global rules apply).
- Use MCP tools only. Do not invoke MCP via Bash (no curl to `/mcp`, no MCP inspector CLI, no running local server files).
- If an MCP tool call fails, report the error and stop. Do not attempt alternative transport mechanisms.

### Catalog & Retrieval Discipline
- Call `list_patterns` at most once per chat unless you cannot find a suitable match in the cached catalog.
- Select patterns using `selection_excerpt`.
- Explicitly reject candidates where `do_not_use_when` applies.
- Call `get_pattern` only for components you will implement, and at most once per pattern id per chat.
- If no suitable pattern exists, state `Native + global rules` and proceed.

### Global Rules Usage
- Call `get_global_rules` only when the task involves page/layout structure, headings/landmarks, navigation, or focus-critical UI (dialogs, menus, carousels).
- Cache global rules once per chat.
- Apply applicable MUST rules before final output.

### Design System & Component Integrity (Minimal-Change Default)
- If the project uses a component library or design system, preserve it.
- Prefer minimal-change compliance: correct usage (props/labels/structure) before replacement.
- If a component is inaccessible, create a small wrapper/adapter or propose an upstream fix.
- Replace components only if explicitly requested or locally owned in the repository.

### Styling Constraints (Project-First)
- Use the project’s styling system (Tailwind, design system, CSS modules, etc.).
- Treat pattern styling snippets as optional examples.
- Do not introduce a new styling approach or dependency unless explicitly requested.

### Scope Control
- Do not refactor unrelated code.
- Do not introduce architectural changes beyond the requested scope.
- Only retrieve and apply patterns for components actually being implemented.

## Response Format (proof-of-concept only)

A. Component Inventory
- List the individual components generated for the code output response, noting those for which `get_pattern` was pulled.
B. MCP Tools Call
- List the MCP tools that were called, in order.
C. Selection Decisions
- Explain the selection decisions that were made to pick the correct pattern for each component.
D. Global Rules Application
- If `get_global_rules` was called and rules were applied, list the rules that were applied, and where they were applied in the code.
E. Token Consumption
- Display the total token consumption for the MCP calls.
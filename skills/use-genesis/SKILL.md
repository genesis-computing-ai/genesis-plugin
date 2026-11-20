---
name: use-genesis
description: Work with the person's own Genesis through its `genesis` MCP server. Use when a Genesis MCP server is connected and the person asks Genesis or a Genesis data agent (such as Eve) to find data, explain lineage, run queries, build or check pipelines, run Missions, or do other data work, or asks which Genesis agents or tools they have.
---

# Work with Genesis

The person's own Genesis is reached through an MCP server they added, usually named `genesis` (Genesis Desktop registers `genesis-desktop`). If no such server is connected, use the `setup-genesis` skill first. If it is listed but not connected, tell the person to run `/mcp` and pick it (Authenticate, for a Genesis that signs in through the browser).

## Discover before you call

Genesis's tools change between versions, and Claude Code prefixes each tool with the server name, so do not assume a fixed list.

1. Look at the tools the `genesis` server offers in this session (`/mcp` shows them to the person).
2. Call `help` through its `call_tool` tool for a short map of what Genesis offers: which thread id goes where, and every action in one line. For one action's parameters and examples, call `describe_action` with its name.
3. `version` through `call_tool` says which Genesis build answers.

## Work with Genesis agents

- `list_agents` through `call_tool` lists the agents the person can use. Eve is Genesis's system manager agent: she builds and monitors other data agents, manages Missions, and runs discovery and analysis. Genesis picks a default agent (usually Eve) when you do not name one.
- Hand a task to an agent with `ask_agent`, described as the outcome the person wants. Pass an agent id only when the person named an agent.
- Keep the `genesis_thread_id` a call returns and reuse it for follow-ups on the same work instead of starting new threads. Long work keeps running on the Genesis server; check on it on the same thread rather than repeating the request.
- When an agent comes back with a plan, a question, or a change that writes to the person's systems, show it to the person and pass their answer back on the same thread. Do not approve plans or writes on the person's behalf.
- Report what Genesis returned, including links it gives, rather than restating results you computed yourself.

## Limits

- Genesis acts with the person's own access; a refusal for permissions is for the person or their Genesis admin to resolve, not something to work around.
- The Try Me MCP token from setup expires after 30 days and can be revoked under API Keys in Genesis. When it stops working, run the `setup-genesis` flow again (`connect_tryme`) to get a new one.
- Product docs: https://docs.genesiscomputing.com (Getting Started > Connect coding agents over MCP).

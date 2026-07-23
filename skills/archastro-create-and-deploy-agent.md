---
name: Create and deploy an ArchAstro agent
description: Authenticate, create an agent, attach a tool, and verify it is healthy.
api: openapi/archastro-platform-openapi.json
operations: [get_api_v1_users_me, get_api_v1_agents, post_api_v1_agents, get_api_v1_agents__agent, post_api_v1_agents__agent_agent_tools, get_api_v1_agents__agent_health]
---

# Create and deploy an ArchAstro agent

Use this to stand up a new agent on the ArchAstro Platform API.

## Auth
Send `Authorization: Bearer $ARCHASTRO_ACCESS_TOKEN` (or `x-archastro-api-key: $ARCHASTRO_PUBLISHABLE_KEY` for client-side). Use a **sandbox** key first — sandbox credentials only touch sandbox data. Base URL: `https://developers.archastro.ai`.

## Steps
1. **Confirm identity** — `GET /api/v1/users/me` (`get_api_v1_users_me`) to verify the token and read the caller's org.
2. **Check for existing agents** — `GET /api/v1/agents` (`get_api_v1_agents`). This list is page-numbered (`page`, `page_size`; response has `has_next`, `total_pages`).
3. **Create the agent** — `POST /api/v1/agents` (`post_api_v1_agents`).
4. **Attach a tool** — `POST /api/v1/agents/{agent}/agent_tools` (`post_api_v1_agents__agent_agent_tools`) to give the agent a capability; list with `get_api_v1_agents__agent_agent_tools`.
5. **Verify health** — `GET /api/v1/agents/{agent}/health` (`get_api_v1_agents__agent_health`) and re-fetch the agent with `get_api_v1_agents__agent`.

## Error handling
- `401` unauthenticated → refresh the token. `403` → the caller lacks permission. `422` → fix invalid body fields. `429` → back off and retry. Errors return a plain `{"error": ...}` envelope.

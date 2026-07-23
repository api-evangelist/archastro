---
name: Start a thread with an agent and follow the conversation
description: Open a thread against an agent, read its messages, and mark them read.
api: openapi/archastro-platform-openapi.json
operations: [post_api_v1_agents__agent_threads, get_api_v1_threads__thread, get_api_v1_threads__thread_messages, post_api_v1_threads__thread_mark_read, get_api_v1_threads__thread_members]
---

# Start a thread with an agent and follow the conversation

Use this to begin and track a conversation with a deployed agent.

## Auth
`Authorization: Bearer $ARCHASTRO_ACCESS_TOKEN`. Base URL `https://developers.archastro.ai`. Realtime channels also use bearer auth.

## Steps
1. **Open a thread with the agent** — `POST /api/v1/agents/{agent}/threads` (`post_api_v1_agents__agent_threads`). Capture the returned `thread` id.
2. **Read the thread** — `GET /api/v1/threads/{thread}` (`get_api_v1_threads__thread`) and its members `get_api_v1_threads__thread_members`.
3. **Poll messages** — `GET /api/v1/threads/{thread}/messages` (`get_api_v1_threads__thread_messages`). This surface is cursor-paginated (`before_cursor`, `after_cursor`). Messages carry an `idempotency_key` so replays are de-duplicated.
4. **Mark read** — `POST /api/v1/threads/{thread}/mark_read` (`post_api_v1_threads__thread_mark_read`) once you have processed the batch.

## Idempotency & errors
- Message sends are idempotent via the `idempotency_key` field — reuse the same key on retry to avoid duplicates.
- `404` → the thread/agent is not visible to the caller. `429` → back off. Error envelope is `{"error": ...}`.

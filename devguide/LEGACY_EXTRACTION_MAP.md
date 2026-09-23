# Legacy Extraction Map

This document classifies legacy code currently living in `molsys-ai-server/client/`. It is a migration map, not an instruction to delete working code immediately.

## Protected first capability

The documentation chatbot is the first operational MolSys-AI capability. Migration must preserve equivalent or better grounded documentation assistance, but does **not** freeze its current endpoint, RAG implementation, schemas, or widget internals.

See the server's `devguide/CHATBOT_PRESERVATION.md`.

## File classification

| Current file | Responsibility | Destination | Migration note |
|---|---|---|---|
| `client/agent/core.py` | agent loop/orchestration | `molsys-ai-agent` | migrate/refactor |
| `client/agent/executor.py` | local tool registry/execution | `molsys-ai-agent` | strengthen validation/authorization |
| `client/agent/planner.py` | specialist planning/tool selection | `molsys-ai-agent` | remove direct ownership of server RAG |
| `client/agent/notebook.py` | notebook agent helpers | `molsys-ai-agent` | migrate after API decision |
| `client/agent/tools/core_tools.py` | local tools | `molsys-ai-agent` | strict approval/security policy |
| `client/agent/tools/molsysmt_tools.py` | MolSysMT adapter prototype | `molsys-ai-agent` | first MolSysSuite adapter candidate |
| `client/agent/model_client.py` | model abstraction + direct HTTP | split/replace | transport → client; backend abstraction may remain agent-side |
| `client/cli/config.py` | endpoint/API-key config | `molsys-ai-client` | SDK configuration/profile layer |
| `client/cli/http_api.py` | HTTP transport | `molsys-ai-client` | typed client |
| `client/cli/main.py` | mixed CLI | split | remote UX vs agent/tool UX |

## Special cases

### planner.py

The current planner imports RAG directly. Target Agent code should consume Software Knowledge through a stable interface when needed; it should not own/import server retrieval internals.

### model_client.py

The current direct HTTP implementation should evolve toward MolSys-AI Client for server access. An agent-side abstract backend interface may remain so local/alternative backends are possible.

### cli/main.py

The file mixes remote-service UX and local specialist-agent UX and must be decomposed rather than moved wholesale.

## Tests

Before deleting legacy modules:

1. establish a server-owned chatbot/Software-Knowledge capability gate;
2. migrate agent tests to `molsys-ai-agent`;
3. migrate client tests to `molsys-ai-client`;
4. retain compatibility tests until legacy imports disappear.

## Safe migration order

1. Baseline the working chatbot capability.
2. Implement minimal typed Client without requiring server endpoint redesign.
3. Establish Agent primitives in `molsys-ai-agent`; server access, when used, goes through stable Client contracts.
4. Reproduce Agent tests.
5. Split CLI ownership.
6. Mark legacy modules deprecated.
7. Verify chatbot capability plus client/agent compatibility.
8. Remove legacy code only then.

The goal is a cleaner architecture **without sacrificing the first working MolSys-AI capability**.

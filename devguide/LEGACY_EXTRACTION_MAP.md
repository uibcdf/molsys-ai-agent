# Legacy Extraction Map

This document classifies legacy code currently living in `molsys-ai-server/client/`. It is a migration map, not an instruction to delete working code immediately.

## Protected first functionality: documentation chatbot

The documentation chatbot is the first operational MolSys-AI functionality and must remain working throughout migration.

Protected server behavior includes:

- `POST /v1/chat`;
- multi-turn `messages` support and legacy `query` compatibility while supported;
- RAG corpus/index construction and retrieval;
- citations and `sources`;
- anchors/deep links;
- symbol verification and re-reading;
- documentation widget integration;
- model-server integration;
- current authentication/CORS/deployment path;
- chatbot benchmarks and smoke tests.

No agent/client extraction is complete if these regress.

## File classification

| Current file | Responsibility | Destination | Migration note |
|---|---|---|---|
| `client/agent/core.py` | agent loop/orchestration | `molsys-ai-agent` | migrate/refactor |
| `client/agent/executor.py` | local tool registry/execution | `molsys-ai-agent` | migrate; strengthen validation/authorization |
| `client/agent/planner.py` | specialist planning/tool selection | `molsys-ai-agent` | migrate; remove direct legacy RAG ownership |
| `client/agent/notebook.py` | notebook-facing agent helpers/workflow generation | `molsys-ai-agent` | migrate after public API decision |
| `client/agent/tools/core_tools.py` | local machine/shell tools | `molsys-ai-agent` | migrate with strict approval/security policy |
| `client/agent/tools/molsysmt_tools.py` | MolSysMT adapter prototype | `molsys-ai-agent` | migrate as first MolSysSuite adapter |
| `client/agent/model_client.py` | direct HTTP/model abstraction | split/replace | remote transport belongs to `molsys-ai-client`; agent-facing model interface may remain in agent |
| `client/cli/config.py` | endpoint/API-key configuration | `molsys-ai-client` | migrate into SDK configuration/profile layer |
| `client/cli/http_api.py` | HTTP transport for chat/engine APIs | `molsys-ai-client` | migrate/refactor into typed client |
| `client/cli/main.py` | mixed CLI surface | split | chat/docs/login/config → client-facing CLI decision; agent/tools → `molsys-ai-agent` |
| `client/agent/__init__.py` | package marker/exports | `molsys-ai-agent` | recreate under new package namespace |
| `client/cli/__init__.py` | package marker | depends on CLI packaging | do not copy blindly |

## Special cases

### planner.py

The current planner performs documentation-RAG retrieval directly. In the target architecture the Agent should not own the server RAG implementation. Software-knowledge queries should go through MolSys-AI Client → MolSys-AI Server contracts.

### model_client.py

`HTTPModelClient` currently calls `/v1/engine/chat` directly. Target code should prefer the typed MolSys-AI Client. Keep an agent-side abstract model/reasoning interface only if it remains useful after the SDK exists.

### cli/main.py

This file contains two products:

1. remote-service UX: login/config/chat/docs;
2. local specialist-agent UX: agent/tools.

It must be decomposed by responsibility. Moving the whole file to either repository would reproduce the current boundary problem.

## Tests

Current `tests/test_smoke.py` mixes server, RAG, CLI and agent imports. Before deleting legacy modules:

1. establish a **server/chatbot protected test gate** containing only server-owned imports and chatbot behavior;
2. migrate agent tests to `molsys-ai-agent`;
3. migrate client transport/config tests to `molsys-ai-client`;
4. retain temporary compatibility tests in the server until legacy imports are removed.

## Safe migration order

1. Freeze and run chatbot baseline tests/benchmarks.
2. Implement minimal typed client transport without changing server endpoints.
3. Copy/refactor agent primitives into `molsys-ai-agent`, initially against the existing server API through the client.
4. Reproduce agent tests in the new repo.
5. Split CLI ownership.
6. Mark server legacy modules deprecated.
7. Verify chatbot and agent/client compatibility.
8. Only then remove legacy client/agent packaging from the server.

The server chatbot is a protected operational capability throughout all phases.

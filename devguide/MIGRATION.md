# Migration from molsys-ai-server

The current `molsys-ai-server` repository contains legacy `client/agent` and `client/cli` code created before the four-repository MolSys-AI structure was frozen.

## Destination rules

Move concepts by responsibility, not by directory:

- planner/executor/tool-inspection and MolSysSuite execution → `molsys-ai-agent`;
- HTTP transport, endpoint configuration, authentication, typed remote schemas → `molsys-ai-client`;
- inference, software-knowledge/RAG, documentation assistants, remote API → remain in `molsys-ai-server`.

## Migration policy

1. Preserve current working server behavior.
2. Classify legacy modules before copying or moving them.
3. Add replacement tests/contracts in the destination repository.
4. Migrate incrementally.
5. Mark legacy modules deprecated only after replacement paths work.
6. Remove legacy code from the server only after compatibility is verified.

No destructive migration is implied by creating this repository.

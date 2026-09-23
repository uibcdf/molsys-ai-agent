# MolSys-AI Agent Architecture

## Mission

Operate MolSysSuite as a specialized agent while keeping scientific execution close to molecular data/tool environments and allowing inference/software knowledge to be local or remote.

## Main responsibilities

- planning and tool selection;
- MolSysSuite API introspection;
- execution and recovery;
- local molecular/session interaction where required;
- verification of symbols, signatures, defaults, and documentation before execution;
- optional consumption of MolSys-AI Software Knowledge and inference services.

## Backend independence

The Agent should not require a single fixed topology.

Possible paths include:

```text
Agent → MolSysSuite directly
Agent → MolSys-AI Client → MolSys-AI Server
Agent → local model/knowledge backend
Agent → other authorized backend
```

MolSys-AI Client is the preferred typed interface to MolSys-AI Server when remote services are used, but Server availability is not an architectural prerequisite for every agent operation.

## Software Knowledge

The Agent should consume software knowledge through stable contracts rather than importing the server's RAG implementation. RAG, symbol cards, recipes, and retrieval internals remain server-side knowledge-service concerns when the server is used.

## Environment boundary

MolSysSuite execution should remain separable from server-side model inference. This supports local, HPC, remote, or hybrid execution without coupling scientific toolchains to the inference environment.

## Relationship with MOLI

MOLI Agent may delegate modeling-specialist tasks to MolSys-AI Agent. MolSys-AI Agent returns outputs/observations; it does not own Nextia Evidence or scientific Decisions.

# MolSys-AI Agent Architecture

## Mission

Operate MolSysSuite as a specialized agent while keeping inference/software knowledge remote-capable and scientific execution close to the molecular data and tool environment.

## Main responsibilities

- planning and tool selection;
- MolSysSuite API introspection;
- execution and recovery;
- local molecular/session interaction where required;
- verification of symbols, signatures, defaults, and documentation before execution;
- communication with MolSys-AI Server through MolSys-AI Client.

## Environment boundary

The MolSysSuite execution environment should remain separable from the server-side model-inference environment. This preserves dependency isolation and allows local, HPC, or other scientific execution backends.

## Relationship with MOLI

MOLI Agent may delegate modeling-specialist tasks to MolSys-AI Agent. MolSys-AI Agent returns outputs/observations; it does not own Nextia Evidence or scientific Decisions.

## Non-goals

- owning remote inference infrastructure;
- owning the MolSysSuite software-knowledge corpus;
- becoming the lightweight transport SDK;
- becoming MOLI Agent;
- becoming the authoritative DiscoveryProject store.

# MolSys-AI Agent

**MolSys-AI Agent is the specialist agent for understanding and operating MolSysSuite.**

It is part of the **MolSys-AI** subsystem and is distinct from **MOLI Agent**, the scientific agent of the wider MOLI Platform.

## Role

MolSys-AI Agent may:

- plan MolSysSuite operations;
- inspect installed MolSysSuite APIs and signatures;
- select and invoke tools;
- execute authorized workflows in the scientific environment;
- inspect outputs and recover from tool-level errors;
- use MolSys-AI Client to access remote inference and software-knowledge services;
- report modeling outputs and observations.

Project-level Evidence, Hypotheses, Strategies, and Decisions remain represented and owned by Nextia.

## Architecture

```text
MOLI Agent (optional delegator)
            │
            ▼
     MolSys-AI Agent
        ╱         ╲
       ▼           ▼
MolSys-AI Client  MolSysSuite
       │          local APIs
       ▼
MolSys-AI Server
inference + software knowledge
```

MolSys-AI Agent is not a mandatory gateway to MolSysSuite.

## Migration status

Legacy agent and CLI prototypes currently live under `molsys-ai-server/client/`. They are migration sources, not the final ownership location. Code should move here incrementally only after classification and compatibility tests; the working server must not be broken during extraction.

Project-level architecture is maintained in [uibcdf/molsys-ai](https://github.com/uibcdf/molsys-ai).

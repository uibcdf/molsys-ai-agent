# MolSys-AI Agent

**MolSys-AI Agent is the specialist agent for understanding and operating MolSysSuite.**

It is part of MolSys-AI and is distinct from MOLI Agent.

## Role

MolSys-AI Agent may plan MolSysSuite operations, inspect installed APIs/signatures, select and invoke tools, execute authorized workflows, inspect outputs, recover from tool-level errors, and report modeling outputs/observations.

It may use MolSys-AI Client to consume remote inference and Software Knowledge services, but this is **not a mandatory dependency path**. The agent is local-first and may use local or alternative model/knowledge backends where appropriate.

Project-level Evidence, Hypotheses, Strategies, and Decisions remain represented and owned by Nextia.

## Architecture

```text
              MOLI Agent
          optional delegation
                  │
                  ▼
           MolSys-AI Agent
          /       |        \
         ▼        ▼         ▼
   MolSysSuite  Client   local/other
    local APIs    │       backends
                  ▼
                Server
       inference + Software Knowledge
```

MolSys-AI Agent is not a mandatory gateway to MolSysSuite, and MolSys-AI Server is not a mandatory gateway to every agent operation.

## Migration status

Legacy agent/CLI prototypes remain under `molsys-ai-server/client/` as migration sources. Extraction is additive and must preserve the operational documentation-chatbot capability.

Project-level architecture is maintained in [uibcdf/molsys-ai](https://github.com/uibcdf/molsys-ai).

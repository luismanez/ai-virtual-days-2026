# Tasks: Copilot tool and agent integration

Commands run from `src/copilot-expert-finder`. File paths below are relative to `src/copilotComponents/expertFinder` unless they start with config, copilot, docs, README, or Specs. Completion status lives only in [tasks/todo.md](../tasks/todo.md).

## COP-01

**Describe the tool and agent behavior**

Update the component tool description, plugin/agent descriptions, and instructions for canonical supported skills and authoritative visual results.

- Acceptance: The agent should map known terms such as SharePoint Framework to SPFx, clarify missing skill, and explain unsupported skills without inventing experts. Preserve the existing generated discovery conventions.
- Verification: Run npm run build and inspect the generated skill-only contract and agent package.
- Dependencies: INL-01.
- Likely files: ExpertFinderCopilotComponent.manifest.json; config/copilot-agent.json; copilot/ai-plugin.json; copilot/instruction.txt.

## COP-02

**Add starters and invocation examples**

Add useful supported-skill starters and a short manual invocation checklist.

- Acceptance: Include natural SPFx/Copilot/database prompts, Spanish empty results, and an unsupported Azure Functions request; clearly separate Workbench argument injection from real Copilot tool choice.
- Verification: Review generated metadata now; run the prompts in actual Copilot once the agent is deployed.
- Dependencies: COP-01.
- Likely files: copilot/declarativeAgent.json; docs/copilot-invocation.md.

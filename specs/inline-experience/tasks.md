# Tasks: Inline experience and host shell

Commands run from `src/copilot-expert-finder`. File paths below are relative to `src/copilotComponents/expertFinder` unless they start with config, copilot, docs, README, or Specs. Completion status lives only in [tasks/todo.md](../tasks/todo.md).

## INL-01

**Connect skill input to the inline view**

Replace message with the minimal validated skill schema, wire a stable provider once (mock during the initial UI increment; automatic root-list selection after SP-03), adapt existing props/root, and show count/first three experts/contact/expand.

- Acceptance: Preserve root/context/targetDocument/teardown and existing IDs. Remove unrelated starter Graph/site calls. Invalid arguments make no expert-items query; the independent root-list availability check is an initialization step.
- Verification: Run npm run build; fire {"skill":"SPFx"}, DB, Spanish, and an invalid argument in Workbench when available.
- Dependencies: DAT-02, ACT-01.
- Likely files: ExpertFinderCopilotComponentProperties.ts; ExpertFinderCopilotComponent.tsx; components/IExpertFinderProps.ts; components/ExpertFinder.tsx; components/ExpertFinderInline.tsx.

## INL-02

**Polish inline states and lifetime**

Add the shared avatar fallback, readable loading/empty/error/retry states, theme tokens, and stable async/mode handling.

- Acceptance: Inline fits a narrow host, indicates more than three results, handles denied expansion, and ordinary host changes do not repeat retrieval or discard app state.
- Verification: Use both themes, broken/missing images, slow/failed mock retrieval during development, and mode changes; inspect requests in the available browser tools.
- Dependencies: INL-01, DAT-03.
- Likely files: components/ExpertFinder.tsx; components/ExpertFinderInline.tsx; components/ExpertAvatar.tsx; localized resources if needed.

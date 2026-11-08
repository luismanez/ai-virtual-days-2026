# Tasks: Contact actions

Commands run from `src/copilot-expert-finder`. File paths below are relative to `src/copilotComponents/expertFinder` unless they start with config, copilot, docs, README, or Specs. Completion status lives only in [tasks/todo.md](../tasks/todo.md).

## ACT-01

**Implement honest contact actions**

Build safe encoded Teams links for verified live UPNs and Fluent controls for both views. In mock mode contact previews are labeled; Schedule is a labeled demo dialog.

- Acceptance: The fixed Teams destination cannot become an arbitrary URL; unavailable identity and rejected host actions are explained. Dialogs close correctly and return focus.
- Verification: Inspect generated links, use the mock dialogs in the UI, and verify real navigation only with a configured tenant user.
- Dependencies: DAT-01.
- Likely files: utils/expertLinks.ts; components/ExpertActions.tsx.

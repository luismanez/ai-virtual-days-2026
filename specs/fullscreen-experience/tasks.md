# Tasks: Fullscreen exploration

Commands run from `src/copilot-expert-finder`. File paths below are relative to `src/copilotComponents/expertFinder` unless they start with config, copilot, docs, README, or Specs. Completion status lives only in [tasks/todo.md](../tasks/todo.md).

## FUL-01

**Add local filter controls**

Implement a simple filter helper and toolbar for name startsWith, Department, Role, and Clear filters.

- Acceptance: Normalize spaces/casing, combine with AND, derive options from fetched results, and never change Skill or trigger retrieval.
- Verification: Try a name prefix, a substring that is not a prefix, combined department/role, no local matches, and Clear filters in the completed view.
- Dependencies: DAT-01, INL-01.
- Likely files: utils/expertFilters.ts; components/ExpertSearch.tsx.

## FUL-02

**Compose and polish fullscreen cards**

Build the mockup-inspired skill header and responsive photo/name/title/summary/department/action cards; connect them to the shared state.

- Acceptance: No Skill selector, location, extra tags, dead menu, or separate details drawer. Filters/results survive mode changes and adapt to both host themes.
- Verification: Run npm run build and manually check both themes, narrow/medium/wide hosts, keyboard/dialog focus, counts, Clear filters, and zero/one/many results.
- Dependencies: FUL-01, INL-02, ACT-01.
- Likely files: components/ExpertFinderFullscreen.tsx; components/ExpertCard.tsx; components/ExpertFinder.tsx; localized resources if needed.

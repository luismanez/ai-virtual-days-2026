# Tasks: Package and conference runbook

Commands run from `src/copilot-expert-finder`. File paths below are relative to `src/copilotComponents/expertFinder` unless they start with config, copilot, docs, README, or Specs. Completion status lives only in [tasks/todo.md](../tasks/todo.md).

## DEM-01

**Build and document installation**

Run the existing production build, verify the sppkg/agent ZIP and local assets, and write the steps to install/sync the baseline agent.

- Acceptance: Artifacts are concrete and usable for review; real publisher values still needed are named, not fabricated. Upload/sync remains separately authorized.
- Verification: Run npm run build; inspect the packages and start the local server to confirm the debug manifests are served.
- Dependencies: FUL-02, COP-02, SP-03, DAT-03.
- Likely files: docs/deployment.md; README.md.

## DEM-02

**Write and rehearse the demo**

Write a short flow for many/one/zero results, expansion, combined filters/clear, both themes, Teams, and Schedule. Include the fallback when discovery propagation is delayed.

- Acceptance: Workbench injection is presented as a fallback, not successful natural discovery. Record actual manual outcomes and remaining host/tenant blockers in the canonical checklist.
- Verification: Rehearse with the available mock/live environment. A final browser/tenant checklist stays open if access is unavailable.
- Dependencies: DEM-01.
- Likely files: docs/demo-runbook.md; specs/tasks/todo.md.

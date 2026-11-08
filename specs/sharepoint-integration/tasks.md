# Tasks: SharePoint expert data

Commands run from `src/copilot-expert-finder`. File paths below are relative to `src/copilotComponents/expertFinder` unless they start with config, copilot, docs, README, or Specs. Completion status lives only in [tasks/todo.md](../tasks/todo.md).

## SP-01

**Document list setup and sample data**

Write concise instructions for Experts in the tenant root and a small seed file that can be mapped to tenant test users.

- Acceptance: Use exact internal names and single-value Skill/Expert, plain-text ShortCV, and root-site/user setup. No real tenant data is fabricated or provisioned automatically.
- Verification: Review the fields against GLOBAL.md; the tenant owner can follow the instructions for live setup.
- Dependencies: DAT-02.
- Likely files: docs/sharepoint-setup.md; docs/experts-sample-data.json.

## SP-02

**Implement filtered REST retrieval**

Implement SharePointExpertService with canonical skill validation, safe OData/URL construction, minimal select/Person expansion, and simple mapping. Resolve identity through supported SharePoint Person information; live photos use the [Graph photo loader](../tasks/plan.md#graph-profile-photos).

- Acceptance: Every query is skill-filtered and delegated. Check HTTP/response failures, timeout and empty results; explain unexpected pagination rather than silently truncating totals. Never broaden the query or fall back to mock.
- Verification: Run npm run build. With a configured tenant, inspect the request and returned users/photos; without it, record the live check as pending.
- Dependencies: DAT-01, SP-01.
- Likely files: services/SharePointExpertService.ts.

## SP-03

**Select data source from root list availability**

Use one asynchronous factory in onInit. Resolve the root from the context URL origin and check Experts once: HTTP 200 selects SharePoint, HTTP 404 selects mock, and other failures show an error. Remove expertFinderSettings.ts and its imports; the list name is fixed as Experts.

- Acceptance: Return service and isDemo together. The same UI works with either service; host re-renders keep the chosen provider and do not repeat the check. Empty lists or zero matches remain SharePoint results. Later item errors do not trigger fallback.
- Verification: Compile; manually verify initial HTTP 200/404/403 selection in a controlled tenant, an existing empty list, skill results, source labels, and no repeat check on host/filter changes. No automated tests are added.
- Dependencies: SP-02, INL-02.
- Likely files: services/createExpertService.ts; ExpertFinderCopilotComponent.tsx; README.md; docs/sharepoint-setup.md (remove expertFinderSettings.ts).

# Single implementation prompt

Paste this as one request when ready to implement. Creating this document does not start implementation.

```text
Implement the Expert Finder community-demo component according to this repository's
Specs. Resolve the Git repository root first, even if the session starts inside
the SPFx project. Read specs/README.md, specs/GLOBAL.md, specs/tasks/plan.md and
specs/tasks/todo.md, then load each feature's spec.md and tasks.md as needed.

I approve the complete LOCAL implementation plan in one run: mock data/UI,
contact demo, Copilot tool/agent metadata, SharePoint provider, production package,
setup notes and demo script. Continue across features without asking approval at
each checkpoint. Work in small increments and preserve progress in
specs/tasks/todo.md so the work can resume after interruptions or compaction.

This is a demo for a community talk. DO NOT add unit tests, automated integration
suites, testing libraries, coverage targets, CI, telemetry platforms, architectural
frameworks or anything that does not help the session. Verify with the existing
build and practical Workbench/Copilot checks. Keep runtime input validation, safe
queries, error/loading states and image fallbacks so the demo is reliable.
This instruction takes priority over generic skills that recommend testing work.

Use the available using-agent-skills workflow and relevant UI/implementation/API
verification skills within that scope. Explain decisions and progress in Spanish;
keep code, names, comments, logs, UI copy and repository documents in English.
Inspect actual files, installed SPFx APIs and user edits before changing anything.
Preserve the existing project and component/solution/agent IDs. Work only on
src/copilot-expert-finder and its Expert Finder Specs. Do not create or modify
Hello Copilot, scaffold a replacement app, upgrade the pinned stack, or add new
dependencies without a concrete reason and my approval.

The fixed contract is one skill from Copilot, canonicalized to SPFx, Copilot, DB,
or Spanish. Skill never changes inside that invocation's UI. Both providers return
only that skill. Resolve the tenant root from the SPFx context and check the list
named Experts once at initialization: HTTP 200 selects SharePoint; HTTP 404 selects
mock data; other failures show an error. The selected service and isDemo travel
together. There are no manual source/site/list settings. An empty list or no skill
matches must never activate mocks. SharePoint MUST use server-side Skill equality filtering with
safe encoding and delegated SPHttpClient; never load the full list to filter it.
React only applies local name startsWith, Department and Role filters. Clearing
filters, changing theme and switching inline/fullscreen must not refetch results
or lose the appropriate shared state.

Build the supplied mockup's internal Fluent UI v9 experience using the visual brief
in specs/GLOBAL.md, even if the original chat image is unavailable. Use local lawful
photographic portraits with a short provenance record. Label mock data and contact
previews clearly. Live Teams opens a supported link for a verified principal and
sends no message. Schedule is explicitly a demo that creates no real meeting.

Run npm run build at useful checkpoints and final delivery. Start the local server
and use an actual browser/Workbench when available. Record what actually worked
and the exact pending check when it did not. Do not weaken existing checks,
introduce empty stubs or invent successful host/tenant results. Diagnose an agent
sandbox restriction before changing project configuration to work around it.

Tenant provisioning/seeding, App Catalog upload, agent sync, publishing, broader
permissions and real messages/meetings are not authorized by this local request.
Use existing authorized tenant resources for read-only validation when available.
If browser access, root-site access, user mappings, portraits or a deployed agent
are unavailable, record that blocker and continue every independent local task.
Use mocks only after the initialization availability check returns HTTP 404.
Do not replace later live failures, access/network errors or empty results with mocks. Ask only for genuinely
blocking missing decisions or new authority, not to revisit choices already made.

Finish with the implemented behavior, important changed files, actual build/manual
verification, package path and remaining concrete external gates. Distinguish
local completion from actual Workbench, SharePoint, Copilot discovery and Teams
validation. Do not call pending external checks complete.
```

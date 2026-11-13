# Expert Finder demo implementation plan

Planning deliverable, ready for review. No application implementation is included in this change.

This is a community conference demo. Keep the implementation easy to explain, attractive, and reliable. Verification consists of the existing build and practical Workbench/Copilot checks. Do not add unit tests, test infrastructure, coverage targets, CI, or production architecture that does not serve the session.

## Start here

1. [GLOBAL.md](GLOBAL.md): requirements, shared contracts, visual brief, and scope limits.
2. [tasks/plan.md](tasks/plan.md): build order and checkpoints.
3. [tasks/todo.md](tasks/todo.md): the single progress checklist, including manual checks.
4. [IMPLEMENTATION-PROMPT.md](IMPLEMENTATION-PROMPT.md): one prompt to execute the local implementation without pausing between features.

## Feature map

| Feature | Scope | Depends on | Files |
|---|---|---|---|
| expert-data | Skill, model, mock service, fictional data and local portraits | none | [Spec](expert-data/spec.md), [Tasks](expert-data/tasks.md) |
| contact-actions | Teams contact with fictional-profile preview and link error feedback | expert-data | [Spec](contact-actions/spec.md), [Tasks](contact-actions/tasks.md) |
| inline-experience | Input seam, host shell, compact results and states | expert-data, contact-actions | [Spec](inline-experience/spec.md), [Tasks](inline-experience/tasks.md) |
| fullscreen-experience | Search, filters and responsive expert cards | inline-experience | [Spec](fullscreen-experience/spec.md), [Tasks](fullscreen-experience/tasks.md) |
| copilot-integration | Tool descriptions, agent instructions and starters | inline-experience | [Spec](copilot-integration/spec.md), [Tasks](copilot-integration/tasks.md) |
| sharepoint-integration | Experts list guide, filtered REST provider and identity/photos | expert-data, inline-experience | [Spec](sharepoint-integration/spec.md), [Tasks](sharepoint-integration/tasks.md) |
| expert-availability | On-demand fullscreen calendar free/busy panel for one selected expert | sharepoint-integration, fullscreen-experience, existing Graph infrastructure | [Spec](expert-availability/spec.md), [Tasks](expert-availability/tasks.md) |
| introduce-me | Fullscreen selected-profile context and a user-triggered introduction draft in Copilot chat (implemented locally; host acceptance pending) | fullscreen-experience, copilot-integration, public Copilot Bridge | [Spec](introduce-me/spec.md), [Tasks](introduce-me/tasks.md) |
| meeting-agenda | Copilot-generated agenda reviewed in an independent component and explicitly saved to the existing meeting (local build passed; user validation pending) | expert-availability booking, copilot-integration, public Copilot Bridge | [Spec](meeting-agenda/spec.md), [Tasks](meeting-agenda/tasks.md) |
| demo-readiness | Package, installation notes, demo script and fallback | all above | [Spec](demo-readiness/spec.md), [Tasks](demo-readiness/tasks.md) |

Introduce me is implemented locally; actual host checks remain pending. Its [feature-specific implementation prompt](introduce-me/tasks.md#single-implementation-prompt) selects this work without reopening the baseline implementation.

Meeting agenda follows [ten tasks](meeting-agenda/tasks.md) and its [feature-specific implementation prompt](meeting-agenda/tasks.md#single-implementation-prompt). The user requested local implementation on 2026-11-13 with verification stopping at a successful build; runtime/tenant checks are left to the user. Existing unfinished tasks and their evidence remain intact.

All detailed task definitions live with their feature. Completion status and observed results live only in tasks/todo.md. The latest written requirements take priority over earlier requirements and over extra details in the promotional mockup. These specs apply only to `src/copilot-expert-finder`; Hello Copilot is outside scope.

Repository documentation, code, comments, logs, and UI copy use English. Explanations to the user use Spanish.

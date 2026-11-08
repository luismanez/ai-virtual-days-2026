# Demo implementation order

Use the feature map in [README.md](../README.md) and the shared contract in [GLOBAL.md](../GLOBAL.md). Detailed tasks live with their feature; [todo.md](todo.md) is the only status checklist. This is a planning deliverable, not an instruction to start coding now.

## Build order and checkpoints

| Stage | Tasks | Practical checkpoint |
|---|---|---|
| Domain and mock data | DAT-01..DAT-03 | Canonical skills, fictional scenarios and local portraits ready |
| Contact controls | ACT-01 | Teams preview and Schedule demo are honest and usable |
| Phase 1: inline | INL-01..INL-02 | A skill fired in Workbench renders a complete compact experience |
| Phase 1: fullscreen | FUL-01..FUL-02 | Expansion, local filters and cards work over the same results |
| Phase 2: agent | COP-01..COP-02 | Tool/agent schema and natural invocation guidance agree |
| Phase 3: SharePoint | SP-01..SP-03 | Root-list availability automatically selects the delegated skill-filtered provider or mock data |
| Phase 4: demo | DEM-01..DEM-02 | Package, installation instructions, script and fallback ready |

Checkpoints mean run the build and manually exercise the new user flow when the host is available. They are not mandatory approval pauses when the user later invokes the single implementation prompt. Keep edits small and fix build failures before dependent work. Do not generate automated test suites or add quality infrastructure for this demo.

Portrait sourcing can proceed after mock identities exist. Pure filters and list setup notes can be prepared before their consuming UI/provider. Default to one sequential executor; the shared root, props and entry files do not need parallel editing.

## Risks that matter for the session

- A bad LLM argument must produce a clear correction, not an unfiltered query.
- Mode/theme/filter changes must preserve state and avoid duplicate retrieval.
- Person email and Teams UPN may differ; verify identity instead of guessing.
- Photos may fail inside the host; retain a graceful fallback.
- Agent discovery can propagate slowly; keep a new-conversation/Workbench fallback.
- A browser, root-site access, mapped tenant users, or deployed agent may be unavailable. Complete independent local work and leave those checks explicitly pending.

## Single-prompt execution

[IMPLEMENTATION-PROMPT.md](../IMPLEMENTATION-PROMPT.md) authorizes the future local implementation once. The executor resumes unchecked tasks, records actual build/manual results, and continues across features. Local source/package work does not imply authority to provision the tenant, seed people data, upload/sync an agent, or publish. No unfinished task or external check is marked complete without evidence.

## Graph profile photos

Small follow-up: SharePoint still supplies experts and verified UPNs; only live photos move to Graph. The user confirmed Graph Explorer returns the photo for the unlicensed demo account; component access remains to be verified.

Execute GP-01 → GP-02 → GP-03. Reuse the existing delegated `User.ReadBasic.All` consent and the [Graph permission guide](../../src/copilot-expert-finder/docs/graph-permissions.md). No new dependencies, unit tests, feature folder or package permission requests.

### GP-01: Retrieve Graph photos

Add one small loader using SPFx's `msGraphClientFactory.getClient('3')` and Graph v1.0 `GET /users/{encodedUPN}/photo/$value`. Lazily share one client, convert binary responses to Blob URLs, and cache pending/completed/unavailable results per normalized UPN for the component lifetime. Release Blob URLs on teardown and ignore late completions. Stop returning live `userphoto.aspx` URLs; retain local mock portraits.

**Acceptance:** missing UPN, 404, access/network errors or a short timeout yield an unavailable photo without failing the expert list. A UPN is requested at most once per instance.

**Files:** entry component, main props, SharePoint provider and one loader file. Verify the installed SDK's binary-response types. No dependency on another task.

### GP-02: Show progressive avatars

Connect the loader to the existing UI with minimal prop changes. Render cards as soon as SharePoint returns; start photos in parallel for the small returned set, including experts beyond the first three. Show a circular Fluent Skeleton at the existing 48/72 px size until each image loads; fall back to initials.

**Acceptance:** no blocked cards, layout shift or endless shimmer. Loaded photos remain visible without repeat requests across inline/fullscreen and filters, in both themes. Photo errors do not activate mock data.

**Files:** existing app root, avatar and necessary view/card props. Depends on GP-01.

### GP-03: Build and check the demo

Run `npm run build`. Manually check Workbench and Copilot with licensed/unlicensed demo users, missing-photo fallback, fullscreen and filters. Aim for the first three photos within about 1–2 seconds after cards appear; record actual cold/warm behavior. If latency consistently hurts the demo, retain the current demo version rather than expand the implementation.

**Acceptance:** build passes, available manual results are recorded, unavailable checks stay pending, and README/setup/deployment/permission docs reflect the implemented photo source. Graph Explorer success alone does not verify component access. No deployment or tenant permission changes are needed. Depends on GP-01 and GP-02.

**References:** [Graph photo endpoint and permissions](https://learn.microsoft.com/en-us/graph/api/profilephoto-get?view=graph-rest-1.0), [SPFx MSGraphClientV3](https://learn.microsoft.com/en-us/sharepoint/dev/spfx/use-msgraph).

## Expert availability panel

Planned extension: follow [the feature spec](../expert-availability/spec.md) and [four tasks](../expert-availability/tasks.md) in order: AV-01 calendar read → AV-02 compact panel → AV-03 fullscreen selection → AV-04 demo validation/docs. The feature tasks include a single implementation prompt. Existing unfinished host checks remain open; this extension does not reimplement them or change their status. No application code or tenant changes are part of this planning update.


## Introduce me

Locally implemented extension; host acceptance pending: [feature spec](../introduce-me/spec.md), [four technical tasks](../introduce-me/tasks.md). Keep all previous unfinished tasks and host checks; this execution makes no tenant changes. No separate tracker or feature framework is introduced.

| Order | Task | Scope | Checkpoint |
| --- | --- | --- | --- |
| 1 | IM-01 | Whitelisted context/prompt helper, public Bridge callback and typed props (3 files) | Installed API shapes, byte guard and ordered failure behavior; build |
| 2 | IM-02 | Fullscreen action and shared React pending/error state (5 files, including the shared props contract) | Build and available UI checks; no host simulator |
| 3 | IM-03 | Agent drafting branch and demo/invocation/walkthrough docs (4 files) | Preserve search routing; real chat/tool diagnostics when deployed |
| 4 | IM-04 | Local release/package and honest validation record (4 authored files) | One current agent ZIP, baseline re-sync documented; unavailable host checks pending |

Use one sequential executor: the callback, root state and card props share files. The entry owns actual host calls and teardown cancellation; React owns a synchronous instance-wide guard plus visible pending/error state. One small helper formats the authorized five-field snapshot and constant follow-up, with a 9,000-byte UTF-8 context budget. The task dependency chain is IM-01 -> IM-02 -> IM-03 -> IM-04.

Risks worth checking for this demo: a stale/mixed profile (await the complete update before the follow-up and serialize clicks); oversized or instruction-like CV text (bounded serialized data, no silent shortening, explicit trust boundary); unsupported/rejected Workbench handoff (local error, no substitute generator); model question count/language or repeat search (drafting instructions plus deployed Copilot diagnostics). Bridge acknowledgement is not a returned draft or proof that the model finished. The existing instruction change requires a baseline re-sync after the separately directed deployment.

The [single feature implementation prompt](../introduce-me/tasks.md#single-implementation-prompt) was executed for all authorized local tasks without per-task approval pauses. Implementation and production build/package checks are complete; Workbench/Copilot checks remain pending.

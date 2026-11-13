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

## Meeting agenda

Locally implemented extension (2026-11-13), build passed and user validation pending: [feature spec](../meeting-agenda/spec.md), [ten detailed tasks](../meeting-agenda/tasks.md) and the [implementation prompt](../meeting-agenda/tasks.md#single-implementation-prompt). Progress lives in [todo.md](todo.md#meeting-agenda--planned); earlier unfinished tasks/results remain intact. The user's latest instruction overrides broader checkpoint/manual verification: execution stopped after the existing build passed, leaving host/calendar checks to the user. No deployment or real-calendar operation was performed.

### Architecture and dependency order

Keep the original booking flow and stamp the event in its existing create POST. Reuse one creation/correlation UUID and the delegated GraphMeetingService for independent event authorization, latest-body reads, section replacement and confirmation. The second component owns its editor/save state and receives only strict `{ eventId, bookingId, agenda }` properties; it cannot depend on the original React/service instance. The original entry owns the ordered Bridge handoff, with a shared introduction/agenda guard. Agent rules separate search, introduction drafting and the agenda-tool route.

Execute numerically with one sequential executor. The dependency column records the graph; shared Graph service, helper, root/props and registration files make concurrent editing unnecessary. AG-02/AG-03 and AG-04/AG-05 divide their service/consumer changes to keep each task within five authored files; together they produce the real review/save interactions. They are internal increments of one complete delivery, with no stubs or alternate demo path.

| Order | Task | Depends on | Scope and authored size | Verification point |
| --- | --- | --- | --- | --- |
| 1 | [AG-01](../meeting-agenda/tasks.md#ag-01) | Existing confirmed booking | Event provenance and returned correlation, 3 files | Preserve one creation/transactionId and Busy/unknown guards |
| 2 | [AG-02](../meeting-agenda/tasks.md#ag-02) | AG-01 | Authorized event read and strict tool schema, 4 files | Build; reject invalid/unmarked/wrong-organizer targets |
| 3 | [AG-03](../meeting-agenda/tasks.md#ag-03) | AG-02 | Registered independent editable review card, 5 files | Invocation only reads; authoritative summary and teardown |
| 4 | [AG-04](../meeting-agenda/tasks.md#ag-04) | AG-02 | Safe same-event save/status service, 3 files | Fresh body merge, escaping, version/conflict/unknown outcomes |
| 5 | [AG-05](../meeting-agenda/tasks.md#ag-05) | AG-03, AG-04 | Explicit review-card save/recovery UI, 2 files | Build/package inspection; available card UI checks |
| 6 | [AG-06](../meeting-agenda/tasks.md#ag-06) | AG-01, AG-02 | Minimal context, ordered Bridge and shared guard contract, 4 files | Exact IDs, byte budget, no generation/save claims |
| 7 | [AG-07](../meeting-agenda/tasks.md#ag-07) | AG-05, AG-06 | Notes/confirmed-booking action through existing React hierarchy, 5 files | Eligibility, shared UI lock and request-only acknowledgement |
| 8 | [AG-08](../meeting-agenda/tasks.md#ag-08) | AG-03, AG-07 | Scoped agent routing and descriptions, up to 3 files | Build; complete source path/schema/routing; actual host behavior still unverified |
| 9 | [AG-09](../meeting-agenda/tasks.md#ag-09) | AG-05, AG-07, AG-08 | Usage/invocation/runbook/walkthrough docs, 4 files | Full final matrix and truthful known limitations |
| 10 | [AG-10](../meeting-agenda/tasks.md#ag-10) | AG-01 through AG-09 | Normal local release, deployment notes and evidence, 4 authored files plus generated outputs | Final build/package and complete manual acceptance where authorized |

Checkpoints after AG-02, AG-05, AG-08 and AG-10 cover source/build/package correctness and available observations. They do not require human approval or a working Copilot orchestration demonstration before continuing local development. Fix actual build failures before dependent edits; record missing host/tenant authority separately and finish independent local work. No new automated tests, libraries, coverage or CI are added.

### Final acceptance and risks

The final matrix in [the spec](../meeting-agenda/spec.md#verification-criteria-after-implementation) evaluates the real complete journey: original meeting creation, exact second-tool identifiers plus generated content, independent review/edit, confirmed save in the same event, preserved Notes/Teams connection, conversation continuity and no duplicate invitations. `AG-*` tasks are distinct from the spec's `MA-*` acceptance IDs; the task file maps coverage. Checkpoint/build success alone does not satisfy conversational or calendar acceptance.

| Risk | Planned handling |
| --- | --- |
| Copilot may refuse, choose the wrong/no tool, mix context or block subsequent chat | Ordered minimal context, explicit correlation/routing, shared handoff lock and final deployed-conversation checks. Preserve the unresolved Introduce me evidence/unknown cause. Accepted risk; no feasibility phase or alternative transport. |
| A model supplies an unrelated event ID or forged correlation | Service reads `/me/events` with fixed queries and validates actual organizer, persisted application provenance, expert and interval; no title-only or model-only trust. |
| A body update removes Notes/Teams data or duplicates agenda | Read current Text/HTML body, replace one bounded escaped section, preserve unrelated content, validate result and use read-only unknown-status recovery. |
| Concurrent edits or lost write responses | Re-read/merge, conditional ETag write, conflict handling and explicit status/review actions; no automatic ambiguous PATCH retry or exactly-once guarantee. `If-Match` enforcement remains the spec's service assumption to observe after implementation. |
| Old agent/package or unavailable permissions/host | Existing version/history/single-ZIP conventions and baseline-sync guidance; real tenant update and meeting operations need separate direction. Effective `Calendars.ReadWrite`, privacy configuration and host outcomes remain unverified here. |

No product decision is missing. The accepted spec supplies behavior/limits; implementation-time service/host observations are recorded honestly without reopening the decision to implement the full feature. Revert through Git remains the agreed fallback for an unsuccessful final result, not an added deployment mechanism.

# Tasks: Meeting agenda with Copilot

**Execution update (2026-11-13):** source, UI, agent routing and usage/release documentation are implemented locally; `npm run build` passed with both components and the normal agent ZIP/sppkg. Execution stopped there as requested; runtime/tenant checks are left to the user. Original task descriptions remain the full acceptance reference; no automated tests, new dependencies or live calendar operations were added/executed.

Ten implementation tasks for the complete community-demo feature. Follow [spec.md](spec.md), the [shared plan](../tasks/plan.md#meeting-agenda) and the single [progress checklist](../tasks/todo.md#meeting-agenda--planned). This document plans development; it does not start implementation, deploy an agent or authorize calendar operations. `AG-*` identifies implementation tasks; `MA-*` below refers to acceptance criteria in the feature spec.

Use one sequential executor in numerical order. Dependencies name the actual prerequisites; related service/card work is split to keep changes small. These increments are internal implementation boundaries, not partial deliveries or decisions about whether to continue. No spike, feasibility task, preliminary orchestration proof or approval pause is included. Unavailable tenant checks remain pending while independent local work continues.

Run commands from `src/copilot-expert-finder`. Abbreviated `components/`, `models/`, `services/` and `utils/` paths are under its existing `src/copilotComponents/expertFinder`; paths starting `src/`, `copilot/` or `config/` are project-relative. `docs/` and `specs/` paths are repository-relative. File counts cover authored implementation changes; generated build/version outputs and routine shared-checklist evidence are not separate feature tasks. Keep actual completion state only in the shared checklist.

Verification uses source/API/schema inspection, the existing `npm run build` at the checkpoints below, and the spec's manual matrix after implementation in authorized hosts. `npm start -- --nobrowser` serves Workbench when useful; stop it before the production build because they share staging folders. Do not add tests, test libraries, coverage, CI, dependencies, permissions, backend, alternate transport, polling, storage outside the event or deployment flags. No build or live rehearsal is run for this planning deliverable.

## AG-01

**Retain verifiable provenance on confirmed bookings**

Extend the existing creation path so each new ExpertFinder event contains the spec's fixed single-value extended property in its original POST. Reuse the creation UUID for `transactionId` and `bookingId`, and return correlation with the confirmed meeting result. Preserve the existing booking behavior.

- Acceptance: The strict metadata contains only version, booking UUID, normalized expert SMTP and original UTC interval; validate the 2,048-byte bound and 30-minute interval before writing. Notes remain in the original body; no profile, objective copy or agenda is added to metadata.
- Acceptance: The existing explicit confirmation, eligibility/calendar checks, pending/uncertain duplicate protection, link retrieval and local Busy marks retain their behavior. A missing Teams URL never causes another POST; legacy events are not silently marked.
- Verification: Inspect the create payload, result mapping, retry/transaction handling and installed Graph client call shapes; include compilation/lint/packaging at the AG-02 checkpoint. Real invitation delivery belongs to final authorized verification, not a planning experiment.
- Dependencies: Existing confirmed Teams booking flow; no pending host checkbox from another feature is a development prerequisite.
- Likely files (3, medium): `models/ExpertMeeting.ts`, `services/GraphMeetingService.ts`, new `utils/meetingAgenda.ts` for shared agenda/provenance constants and validation.
- Spec coverage: MA-01, MA-08, MA-09.

## AG-02

**Read only an authorized ExpertFinder event for agenda review**

Add the normalized agenda read contract and strict second-tool properties. Reuse the existing delegated Graph factory and fixed select/filtered expansion; keep raw bodies, provenance parsing and version state inside the service.

- Acceptance: Validate exact opaque `eventId`, UUID `bookingId` and bounded nonempty plain-text `agenda`, rejecting extra properties, invalid control characters and reserved delimiters before Graph access. Export draft-7 JSON Schema through the installed Zod convention; add no schema library.
- Acceptance: Authenticate through `/me`; verify organizer identity, marker/correlation, required expert attendee, uncancelled single 30-minute Teams event and unchanged recorded interval. Return only normalized summary/current agenda/review version/validated links. Unmarked, wrong-organizer, changed, malformed or inaccessible events fail closed without a write.
- Acceptance: Reads have bounded timeout/disposal/error handling; HTTP failures and missing version/body/connection data remain truthful errors. The contract works in a new service instance without directory selection or the first component's memory.
- Verification: Inspect invalid-input-before-read and eligibility branches against the spec; run the AG-02 checkpoint build and inspect the compiled properties module's JSON Schema export. Manifest registration follows in AG-03. Direct argument injection in Workbench after card integration checks only inputs/UI, not natural Copilot routing.
- Dependencies: AG-01.
- Likely files (4, medium): `utils/meetingAgenda.ts`, new `models/MeetingAgenda.ts`, `services/GraphMeetingService.ts`, new `src/copilotComponents/prepareMeetingAgenda/PrepareMeetingAgendaCopilotComponentProperties.ts`.
- Spec coverage: MA-04, MA-09, MA-10, MA-11.

## Checkpoint after AG-02

Run `npm run build`; inspect strict schema, provenance/read boundaries and preserved booking code. Record actual results under the feature in the shared checklist. This is a source/build checkpoint, not a live-calendar exercise or a decision about developing the rest of the feature.

## AG-03

**Render an independent event-backed agenda review card**

Register the second component/tool in the existing solution and render the draft passed through its validated properties. Give it its own React root, Fluent provider/target document, GraphMeetingService and teardown. This increment reads and edits locally; the explicit save interaction is completed by AG-04/AG-05, without a fake Save handler.

- Acceptance: The new manifest exposes `PrepareMeetingAgendaTool` with the strict properties module and a distinct component GUID. Existing component/solution/agent IDs remain stable; the normal bundle/agent configuration includes both components.
- Acceptance: Loading, invalid input, read error/retry and editable draft states show authoritative title, expert and zoned time from Graph. The card works inline, shows the unsaved/possible-update-notification copy, validates text, and neither invocation nor rendering writes to the calendar.
- Acceptance: Ordinary re-renders/theme/mode changes preserve edits; changed invocation identity resets incompatible state and ignores stale completions. The card remains independent when the first component is disposed; no live HTML rendering, browser storage or shared callback is introduced.
- Verification: Inspect registration and component lifecycle; verify them in the AG-05 checkpoint build. When authorized Workbench is available after integration, check direct valid/invalid arguments, independent loading/retry and both themes. Do not use direct injection as evidence of agent choice.
- Dependencies: AG-02.
- Likely files (5, medium): new `src/copilotComponents/prepareMeetingAgenda/PrepareMeetingAgendaCopilotComponent.tsx`, new matching `.manifest.json`, new `src/copilotComponents/prepareMeetingAgenda/components/MeetingAgendaReview.tsx`, `config/config.json`, `config/copilot-agent.json`.
- Spec coverage: MA-04, MA-09, MA-13, MA-14.

## AG-04

**Update only the managed agenda section in the same event**

Implement the real Graph save/status operations and bounded body composition used by the review card. Handle Text and HTML bodies using the exact readable section boundaries from the spec; preserve the rest of the latest event body and Teams connection.

- Acceptance: A save re-reads/authorizes the event, detects review conflicts, skips an already identical section and otherwise adds/replaces one safe section. Escape generated text, preserve original Notes/unrelated markup and the online-meeting blob, enforce 128 KiB, and reject partial/duplicate/ambiguous boundaries. Never reconstruct the body from old Notes or accept model HTML.
- Acceptance: PATCH targets the same encoded event ID and contains only `body`, with the freshly fetched ETag in `If-Match`. Missing version/eligibility or conflicts stop the write; no unconditional fallback. Confirm ID, marker, intended section, preserved content and Teams connection from the result/one bounded read before returning saved.
- Acceptance: Deduplicate in-flight saves; ambiguous PATCH/confirmation outcomes retain attempted content and an unknown-write guard. Status checking only reads, an absent/different section does not prove failure, and later writes require the spec's explicit inspection/review decision. Timeout/teardown never implies rollback; no recovery creates another event.
- Verification: Inspect merge/escaping and the saved/alreadySaved/conflict/unknown/error paths, including 409/412, 429 and disposal; compile at AG-05. Exercise real Text/HTML preservation and service conditional-write behavior only in final authorized verification. Keep `If-Match` enforcement labeled as the spec's unverified service assumption, not a prior gate.
- Dependencies: AG-02.
- Likely files (3, medium): new `utils/meetingAgendaBody.ts`, `services/GraphMeetingService.ts`, `models/MeetingAgenda.ts`.
- Spec coverage: MA-06, MA-08, MA-09, MA-10, MA-11.

## AG-05

**Save a reviewed draft with truthful result and recovery states**

Connect the review editor to the real service operations. Complete the card's explicit save journey, rather than treating tool rendering or model output as consent to update an invitation.

- Acceptance: **Save agenda to meeting** is enabled only for a valid editable draft and eligible event; one synchronous guard locks the editor/button through read, write and confirmation. Show success/already-saved only on confirmed content, with **Open in Outlook** from a validated Graph link and **Edit agenda** for another deliberate save.
- Acceptance: Retain edits on failures/conflicts; implement **Reload meeting**, read-only **Check save status**, unknown-write locking and explicit re-enable/review behavior from the spec. Link failures retain saved status, and retries/duplicate cards never create invitations or automatically overwrite another agenda.
- Acceptance: Progress, errors, focus and keyboard labels work inline in both themes, narrow widths and 200% zoom. No generation result is sent back to the original component and no local text edit is automatically shared with Copilot.
- Verification: Run `npm run build` at this checkpoint and inspect the packaged second component/tool schema. Manually inspect available Workbench UI and, only with explicit calendar-write authority, verify save/reload/repeat/unknown outcomes against the same event in Outlook. Record unavailable checks as pending.
- Dependencies: AG-03 and AG-04.
- Likely files (2, small): `src/copilotComponents/prepareMeetingAgenda/components/MeetingAgendaReview.tsx`, `src/copilotComponents/prepareMeetingAgenda/PrepareMeetingAgendaCopilotComponent.tsx`.
- Spec coverage: MA-06, MA-07, MA-08, MA-11, MA-13.

## Checkpoint after AG-05

Build/package must include the independent review component and the actual save path. Inspect that invocation itself only reads, writes require the user action, and recovery never calls event creation. Record available UI evidence separately from real-event preservation/confirmation checks; missing tenant authority does not prevent the remaining local tasks.

## AG-06

**Publish the confirmed meeting context through the ordered Bridge handoff**

Add `onPrepareMeetingAgenda` at the original SPFx entry and extend its action contracts. Capture the confirmed booking and verify it through the agenda read before publishing. Extend the existing introduction controls to honor a shared handoff-disabled state without changing their drafting label/behavior.

- Acceptance: Serialize only the spec's meeting identifiers/title/UTC times/viewing zone, original objective and selected professional fields after attendee matching. Keep the descriptive data label separate from fixed instructions; enforce the complete 9,000-byte budgets for context/follow-up before either call, without truncation or stale-context sends.
- Acceptance: Await `updateModelContextAsync`, recheck disposal/abort and then call `sendFollowUpMessageAsync`, inspecting `isError`/rejection with supported 15-second timeout/signal options. The correlated follow-up requests the second tool and a real generated agenda; resolution acknowledges a handoff only.
- Acceptance: One synchronous entry guard/AbortController covers both introduction and agenda handoffs. Reject competing requests instead of queueing; abort at teardown and prevent an unsent follow-up. Introduction buttons can be disabled for an agenda handoff while **Requesting draft…** still identifies only a true introduction request.
- Verification: Compare call signatures with installed public declarations and inspect captured data, byte/error/abort paths and absence of Graph writes from generation. Build at AG-08; later check rapid cross-action clicks and host rejection through the actual integrated UI, without Bridge mocks or transport alternatives.
- Dependencies: AG-01 and AG-02.
- Likely files (4, medium): `utils/meetingAgenda.ts`, `src/copilotComponents/expertFinder/ExpertFinderCopilotComponent.tsx`, `components/IExpertFinderProps.ts`, `components/ExpertActions.tsx`.
- Spec coverage: MA-02, MA-03, MA-10, MA-11, MA-12.

## AG-07

**Offer agenda preparation after confirmed meeting creation**

Thread the entry callback through the existing React hierarchy and complete the booking-success action. Use the root's shared pending state for introduction/agenda UI while keeping the entry guard authoritative. This task connects the five existing consumers in one compiling change.

- Acceptance: Notes becomes **Session objective / notes (optional)**, retaining its 1,000-character limit and sharing disclosure. Capture the successful objective and immutable expert/meeting/zone for preparation; empty Notes remains valid and requests a general skill-based learning agenda.
- Acceptance: Only confirmed creation exposes **Prepare agenda with Copilot**; pending, failed or uncertain creation cannot initiate it. Show **Requesting agenda…**, truthful request-accepted/error feedback and deliberate retry behavior; closing the initial surface does not disable an already delivered independent card.
- Acceptance: Shared UI locking prevents concurrent introduction/agenda clicks without false draft labels, queued calls or effect-driven sends. Preserve booking guards/Busy marks, existing directory/contact/availability behavior and the second card's sole ownership of reviewed text/save actions.
- Verification: Inspect the complete callback chain and source-only eligibility/teardown paths; include the AG-08 build. In available authorized hosts check Cancel, mock/self/past/unknown states, rapid cross-action clicks, original objective, mode changes and handoff errors. Actual create-to-save and conversation behavior remain final checks.
- Dependencies: AG-05 and AG-06.
- Likely files (5, medium): `components/ExpertFinder.tsx`, `components/ExpertFinderFullscreen.tsx`, `components/ExpertCard.tsx`, `components/ExpertAvailabilityPanel.tsx`, `components/MeetingConfirmationDialog.tsx`.
- Spec coverage: MA-01, MA-02, MA-05, MA-10, MA-11, MA-13.

## AG-08

**Route agenda preparation to the second tool without changing Introduce me**

Update the baseline instructions and descriptions for the actual three-intent behavior: search, introduction drafting, and preparation of the already created meeting. Use the pattern reviewed in Choice Relay without copying its older dependencies or treating its behavior as a guarantee.

- Acceptance: A matching `meetingAgenda` context/request produces a concise objective, timed blocks totaling 30 minutes and exactly three relevant questions, then one `PrepareMeetingAgendaTool` invocation with exact IDs and nonempty agenda. Missing/conflicting context asks the user to use the confirmed-booking action; no fabricated IDs or replacement search/event.
- Acceptance: Explicitly scope the introduction no-other-tools prohibition to `introductionDraft`; the agenda route permits its required second tool. Preserve skill mappings, normal search and introduction behavior, supported profile facts, untrusted-data handling and the preceding human request's language, including Spanish.
- Acceptance: Generation never claims a calendar save or invitation delivery. The complete routing/instruction pair documents that the follow-up starts a turn and the agent chooses the tool; generic refusal, prose-only output or blocked conversation are failures to observe, not grounds for a feasibility phase or alternate transport.
- Verification: Review the introduction prompt plus full agent instructions for contradictory restrictions and unsupported assertions. Run `npm run build`; inspect the generated agent's updated instructions and the component manifests/bundle configuration for both tool registrations. Preserve dynamic discovery rather than expecting a static tool catalog in the generated agent. Actual natural second-tool arguments and conversation continuity are checked only against the fully implemented deployed feature.
- Dependencies: AG-03 and AG-07.
- Likely files (3, medium): `copilot/instruction.txt`, `copilot/ai-plugin.json`, `copilot/declarativeAgent.json` (descriptions only as needed; preserve action binding, starters and schema versions).
- Spec coverage: MA-03, MA-05, MA-10, MA-12, MA-14.

## Checkpoint after AG-08

Run the existing build and inspect the complete local creation-to-handoff-to-review-to-save path, manifest/schema generation and baseline routing. The feature is fully connected in source; no preliminary proof of Copilot invocation is required to finish documentation/packaging. Preserve the unresolved Introduce me host failure as a known risk and carry all actual-host acceptance checks into AG-10.

## AG-09

**Document the complete conference journey and its limits**

Update existing usage resources for the implemented feature, using the spec's final manual matrix and observable acceptance criteria rather than introducing another verification system.

- Acceptance: Explain Notes/context sharing, confirmed-booking preparation, independent editable card, explicit save to the same event, possible update notifications, Outlook access and failure/status recovery. Include no-copy/paste/no-second-meeting behavior and the limits of closing/disposal.
- Acceptance: Invocation/runbook checks cover actual second-tool ID/correlation/agenda arguments, Spanish/English content, conversation continuity after saving/errors, Introduce me/search branch separation, preservation/idempotency/concurrency and existing-feature regressions. Distinguish source/build/Workbench injection from deployed natural invocation.
- Acceptance: Preserve the recorded host issue and unknown cause; document accepted model/service assumptions and minimal authorized data without promising a fix, privacy setting or conditional-write guarantee. No extra consent commands, fallback generator or speculative product flow is added.
- Verification: Review resource text against the final source and spec acceptance coverage; check local links/anchors and executable build examples without executing tenant actions. Documentation is evidence only of described behavior, not successful tenant rehearsal.
- Dependencies: AG-05, AG-07 and AG-08.
- Likely files (4, medium): `docs/copilot-invocation.md`, `docs/demo-runbook.md`, `docs/code-walkthrough.md`, `src/copilot-expert-finder/README.md`.
- Spec coverage: MA-01 through MA-14 through the documented final matrix.

## AG-10

**Package the implementation and record full-journey verification**

Prepare the normal local release and report actual build/package/manual evidence. The baseline instructions change, so document the existing update/sync route rather than treating a component-only replacement as sufficient. This final task evaluates the whole feature, not a decision to develop a remainder.

- Acceptance: Advance then-current app/solution numeric versions, preserve existing IDs/schema versions and `teams/.copilot-agent-hotfix.json` history, and build with the dev server stopped. The sppkg embeds one current agent ZIP with updated instructions, plus the registered component assets/manifests. The second tool's schema agrees with its properties export; keep dynamic discovery rather than adding a static catalog or extra deployment/activation infrastructure.
- Acceptance: Apply the spec's complete acceptance/manual matrix when the required host and authority exist: one real creation, correct nonempty second-tool invocation, edited confirmed save in the same event, original Notes/Teams preservation, no duplicates, language, conversation continuity, failures/uncertainty, teardown and regressions. Record the actual `If-Match` and body-normalization behavior instead of assuming it.
- Acceptance: Keep local completion distinct from external acceptance. Existing baseline build/check marks are not evidence for this release. Unperformed checks stay open; do not upload/sync/grant permissions or create/modify meetings without the corresponding later user direction. Final failures are not masked by a fallback, fake result, weakened contract or prior feasibility gate.
- Verification: Run `npm run build`, inspect sppkg/agent ZIP contents, component/tool bindings, schema, versions and archive integrity, check Markdown links and `git diff --check`, and record actual authorized manual results. Refer to [the spec's verification matrix](spec.md#verification-criteria-after-implementation); never report build output as live Graph/Copilot success.
- Dependencies: AG-01 through AG-09.
- Likely authored files (4, medium): `copilot/manifest.json`, `config/package-solution.json`, `docs/deployment.md`, `specs/tasks/todo.md`. The existing build produces ZIP/sppkg/version-history outputs; no generated file is hand-edited.
- Spec coverage: MA-01 through MA-14.

## Single implementation prompt

Use this as a later request; writing the plan does not execute it:

```text
Implement the complete Meeting agenda feature in specs/meeting-agenda/spec.md,
following AG-01 -> AG-10 in specs/meeting-agenda/tasks.md and the shared plan.
First inspect current source, user edits and specs/tasks/todo.md; preserve all
existing progress and record actual results only in that shared checklist.
Keep task IDs AG-* distinct from the spec's acceptance IDs MA-*.

Complete all local tasks without per-task approval pauses. The decision to build
the full journey is already made and Copilot orchestration risk is accepted.
Do not add a spike, prototype, feasibility gate, partial-delivery decision or
require proof of the second tool before continuing local implementation.

Reuse the existing booking flow, Notes, SPFx public Bridge, React, Fluent UI and
delegated GraphMeetingService. Stamp provenance in the original event creation,
keep exact event/booking IDs, publish minimum context before the follow-up and
have the agent generate actual content for PrepareMeetingAgendaTool. Treat that
tool as an independent component with authenticated event retrieval, editable
review and explicit Save agenda to meeting. Rendering/generation must not save.
Preserve the current body, original Notes and Teams connection; replace one
escaped agenda section and confirm the same event before reporting success.
Handle duplicate, conflict, uncertain-write and teardown cases without creating
another meeting or automatically retrying an ambiguous PATCH.

Preserve Introduce me/search routing and all existing directory/photo/contact/
Schedule/availability/booking behaviors. Keep the no-other-tools introduction
rule scoped to that request. Document the known unresolved host refusal/blocked
conversation without inventing its cause or claiming that this feature fixes it.
The follow-up starts a conversation turn; it does not force a tool or return an
agenda to the original component. Keep the spec's service assumptions explicit.

Add no dependencies, scopes, backend, alternate generator/transport, external
storage, polling, automated test suites/libraries, coverage, CI or release flags.
Use the existing build at checkpoints and final packaging, plus manual checks
only where host access and action authority exist. Preserve IDs, version history
and one current agent ZIP; describe the required baseline-agent re-sync.

This later request authorizes local implementation/package work only. Do not
deploy, upload/sync, change consent, send messages or create/modify real meetings
without corresponding user direction. Missing external access/authority leaves
the affected manual checks pending while independent local work continues.
Report actual local and tenant evidence separately; never claim unperformed
checks passed. Keep repository docs/code/UI in English and explanations in Spanish.
```

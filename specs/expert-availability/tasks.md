# Tasks: Expert Availability Panel

Four sequential tasks for the community demo. Paths are relative to `src/copilot-expert-finder`; component paths start at `src/copilotComponents/expertFinder`. Keep completion status only in [tasks/todo.md](../tasks/todo.md). No unit testing, new dependencies or extra architecture.

## AV-01

**Read one expert's free/busy**

Add optional SMTP email to Expert and the SharePoint Person projection/schema (`Expert/EMail`), without using it as the Teams UPN. Add a small typed availability model and GraphAvailabilityService that follows the [request/time/privacy contract](spec.md#calendar-read-contract) using the existing delegated SPFx Graph infrastructure. Keep mapping/window helpers in that service unless a separate file actually helps.

- Acceptance: Exactly one SMTP target, 30-minute slots and today's local 09:00–18:00 converted to UTC. Validate the availability view and per-schedule error; no email, missing mailbox, HTTP failures, malformed/empty responses and timeouts stay unavailable rather than Free. Missing email must not break expert-list loading.
- Verification: Compile with the installed SDK types. During final manual validation inspect the outgoing payload, compare a known calendar, check null/empty email and inspect local/UTC alignment. Do not execute live requests against unrelated users while planning.
- Dependencies: Existing SharePoint provider and Graph authentication infrastructure.
- Likely files (4): `models/Expert.ts`, `services/SharePointExpertService.ts`, new `models/ExpertAvailability.ts`, new `services/GraphAvailabilityService.ts`.

## AV-02

**Render the compact Availability panel**

Create ExpertAvailabilityPanel using Fluent v9 and the typed service. Show local date/time-zone context and the 18 loaded slots in Morning/Afternoon pages, with larger time-labeled blocks, local arrows, responsive one/two-row layouts, legend, current-slot outline, local loading and friendly unavailable/error states. Provide Refresh/retry; clear results and ignore stale requests when the expert changes or the panel unmounts.

- Acceptance: Free/Tentative/Busy/Out of Office are identifiable beyond color; no event details appear. The marker only applies inside the window, and a local clock update makes no Graph call. Refresh is disabled while pending and recomputes today. Only the current panel's result is retained in memory.
- Verification: Review the lifecycle and labels; manually check all four statuses, boundaries, loading, unavailable/failed data, both themes and narrow-card layout after integration. No synthetic availability in the live path.
- Dependencies: AV-01.
- Likely files (1): new `components/ExpertAvailabilityPanel.tsx`; split a timeline only if implementation size warrants it.

## AV-03

**Connect selection in fullscreen cards**

Compose the availability service in the SPFx entry using the same framework Graph factory as photos. Pass it through the existing app props to fullscreen. Add Check availability and render the panel inside the selected ExpertCard, with selection owned by ExpertFinderFullscreen. This is the existing details surface; do not build a second detail view.

- Acceptance: No calendar request on search, inline rendering or initial fullscreen render. One selected panel; repeated selection is idempotent and Refresh is the only reload for that selection. Switching experts, hiding the selection through filters, changing skill or leaving fullscreen clears the old panel and ignores late responses. Mock mode and missing identity make no calendar call. Existing list/photo/contact behavior remains intact.
- Verification: Run `npm run build` after wiring. Manually select A then B while A is pending, double-click, filter the selected expert out, switch display modes and inspect calendar request counts. Record unavailable host access as pending.
- Dependencies: AV-01 and AV-02.
- Likely files (5): `ExpertFinderCopilotComponent.tsx`, `components/IExpertFinderProps.ts`, `components/ExpertFinder.tsx`, `components/ExpertFinderFullscreen.tsx`, `components/ExpertCard.tsx`.

## AV-04

**Prepare and validate the conference demonstration**

Extend the existing CLI permissions guide for calendar scope inspection/reuse and optional Calendars.ReadBasic consent, with no CLI setup or tenant changes. Update SMTP setup notes, feature overview and demo script. Reuse the existing Calendars.Read grant if it is still present. Keep a successful demonstration on a non-family expert with an accessible Exchange calendar and known free/busy entries; a mailbox-free account is an unavailable-state example.

- Acceptance: Existing `npm run build` passes; docs distinguish local completion from actual host acceptance. Manual checks cover one real calendar, all four states when prepared, missing email/mailbox, denied/policy access, empty/per-schedule errors and service failure without changing other experts. No calendar writes, scheduling, presence or automatic consent.
- Verification: Compare slot times with Outlook and the request payload for winter/summer and DST transition dates (for Europe/Madrid, 2026-03-29 and 2026-10-25). Inspect date/zone conversion in browser tooling without adding date navigation or test infrastructure to the product. Check current-slot start/end, A→B while pending, Refresh, both themes and small widths. Record build/manual results in the existing checklist; keep unperformed tenant checks open.
- Dependencies: AV-01 through AV-03.
- Likely files (4): `README.md`, `docs/graph-permissions.md`, `docs/sharepoint-setup.md`, `docs/demo-runbook.md`; progress updates in specs are bookkeeping only.

## Single implementation prompt

> Implement `specs/expert-availability/spec.md` following AV-01 → AV-04 in this file. Keep scope to this feature, reuse the existing SPFx/Fluent/Graph patterns, and preserve current expert search, photos and contact actions. Use the existing build and manual validation; add no unit tests, dependencies or package permission requests and make no tenant changes. Update the feature's progress with actual results, leaving unavailable Workbench/Copilot checks pending.

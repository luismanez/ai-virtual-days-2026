# Expert Finder demo checklist

The baseline, Graph photos and expert availability are implemented locally. User-provided Copilot screenshots now confirm basic deployed invocation/rendering, profile images and an availability timeline in dark theme; broader external acceptance remains pending. Checked tasks have evidence below. Tasks needing additional Workbench, tenant or dialog interaction remain unchecked until those specific checks are verified; their code can already be implemented. No automated test suites are being added.

Meeting agenda source is implemented locally (2026-11-13); the production build/package passed. Execution stopped there as requested; the user will verify host/calendar behavior. Earlier baseline check marks do not verify this feature's runtime acceptance.

## Skill scoped mock data

- [x] **DAT-01** — [Define skill and domain contracts](../expert-data/tasks.md#dat-01)
- [x] **DAT-02** — [Implement the mock service](../expert-data/tasks.md#dat-02)
- [x] **DAT-03** — [Bundle demo portraits](../expert-data/tasks.md#dat-03)

## Contact actions

- [ ] **ACT-01** — [Implement honest contact actions](../contact-actions/tasks.md#act-01)

## Inline experience and host shell

- [ ] **INL-01** — [Connect skill input to the inline view](../inline-experience/tasks.md#inl-01)
- [ ] **INL-02** — [Polish inline states and lifetime](../inline-experience/tasks.md#inl-02)

## Fullscreen exploration

- [ ] **FUL-01** — [Add local filter controls](../fullscreen-experience/tasks.md#ful-01)
- [ ] **FUL-02** — [Compose and polish fullscreen cards](../fullscreen-experience/tasks.md#ful-02)

## Copilot tool and agent integration

- [x] **COP-01** — [Describe the tool and agent behavior](../copilot-integration/tasks.md#cop-01)
- [ ] **COP-02** — [Add starters and invocation examples](../copilot-integration/tasks.md#cop-02)

## SharePoint expert data

- [x] **SP-01** — [Document list setup and sample data](../sharepoint-integration/tasks.md#sp-01)
- [ ] **SP-02** — [Implement filtered REST retrieval](../sharepoint-integration/tasks.md#sp-02)
- [ ] **SP-03** — [Select data source from root list availability](../sharepoint-integration/tasks.md#sp-03)

## Package and conference runbook

- [x] **DEM-01** — [Build and document installation](../demo-readiness/tasks.md#dem-01)
- [ ] **DEM-02** — [Write and rehearse the demo](../demo-readiness/tasks.md#dem-02)

## Graph profile photos — host acceptance pending

- [x] **GP-01** — [Retrieve Graph photos with a small cached loader](plan.md#gp-01-retrieve-graph-photos)
- [ ] **GP-02** — [Show progressive avatars in the existing UI](plan.md#gp-02-show-progressive-avatars) — source implemented; host acceptance pending
- [ ] **GP-03** — [Build, check the demo and update current docs](plan.md#gp-03-build-and-check-the-demo) — build/docs complete; tenant checks pending

## Expert availability panel — host acceptance pending

- [x] **AV-01** — [Read one expert's free/busy](../expert-availability/tasks.md#av-01)
- [ ] **AV-02** — [Render the compact Availability panel](../expert-availability/tasks.md#av-02) — source implemented; host visual checks pending
- [ ] **AV-03** — [Connect selection in fullscreen cards](../expert-availability/tasks.md#av-03) — source/build complete; host lifecycle checks pending
- [ ] **AV-04** — [Prepare and validate the conference demonstration](../expert-availability/tasks.md#av-04) — build/docs complete; tenant rehearsal pending

## Confirmed Teams meetings — host acceptance pending

- [x] Implement and build the [confirmed Teams meeting flow](../expert-availability/spec.md#confirmed-teams-meetings), its guarded Graph service, UI and permission/demo notes.
- [x] Immediately mark confirmed meetings Busy locally, retaining the expert/interval across Refresh and panel remounts for the component instance; isolated production build/package passed.
- [ ] Verify real invitation delivery, Teams link, self/past/no-write cancellation, both-calendar conflicts, duplicate/uncertain outcomes, immediate/retained Busy marks and modal focus with controlled non-family accounts in Workbench/Copilot.

## Introduce me — host acceptance pending

- [x] **IM-01** — [Publish selected context and request a draft](../introduce-me/tasks.md#im-01) — public declarations, source inspection and production build verified locally.
- [ ] **IM-02** — [Add the fullscreen action and shared pending/error feedback](../introduce-me/tasks.md#im-02) — UI and shared guard implemented; build passes. Workbench themes, keyboard, rapid-click and failure behavior pending.
- [ ] **IM-03** — [Teach the agent to draft without another search](../introduce-me/tasks.md#im-03) — Instructions and documentation updated. Real Copilot draft, language, question count, selected context and routing pending.
- [ ] **IM-04** — [Package and record actual demo validation](../introduce-me/tasks.md#im-04) — Local production package and version checks pass. Deployed-host acceptance pending; no tenant changes performed.

## Meeting agenda — planned

Local source/docs are implemented and compilation/packaging passed. Task/checkpoint boxes below retain their complete acceptance meaning and are not automatically checked from source/build work alone. Per the latest request, intermediate/rehearsal checks were not executed; remaining host checks belong to the user.

- [ ] **AG-01** — [Retain verifiable provenance on confirmed bookings](../meeting-agenda/tasks.md#ag-01).
- [ ] **AG-02** — [Read only an authorized ExpertFinder event for agenda review](../meeting-agenda/tasks.md#ag-02).
- [ ] **Checkpoint after AG-02** — [Build and inspect provenance, event-read boundaries and schema](../meeting-agenda/tasks.md#checkpoint-after-ag-02); no preliminary live feasibility check.
- [ ] **AG-03** — [Render an independent event-backed agenda review card](../meeting-agenda/tasks.md#ag-03).
- [ ] **AG-04** — [Update only the managed agenda section in the same event](../meeting-agenda/tasks.md#ag-04).
- [ ] **AG-05** — [Save a reviewed draft with truthful result and recovery states](../meeting-agenda/tasks.md#ag-05).
- [ ] **Checkpoint after AG-05** — [Build/package and inspect the explicit card save/recovery path](../meeting-agenda/tasks.md#checkpoint-after-ag-05); record available UI evidence and pending tenant checks separately.
- [ ] **AG-06** — [Publish the confirmed meeting context through the ordered Bridge handoff](../meeting-agenda/tasks.md#ag-06).
- [ ] **AG-07** — [Offer agenda preparation after confirmed meeting creation](../meeting-agenda/tasks.md#ag-07).
- [ ] **AG-08** — [Route agenda preparation to the second tool without changing Introduce me](../meeting-agenda/tasks.md#ag-08).
- [ ] **Checkpoint after AG-08** — [Build and inspect the complete local path and agent routing](../meeting-agenda/tasks.md#checkpoint-after-ag-08); no per-task approval pause or required orchestration proof.
- [ ] **AG-09** — [Document the complete conference journey and its limits](../meeting-agenda/tasks.md#ag-09).
- [ ] **AG-10** — [Package the implementation and record full-journey verification](../meeting-agenda/tasks.md#ag-10).
- [ ] **Final checkpoint** — [Verify complete feature acceptance](../meeting-agenda/spec.md#verification-criteria-after-implementation): one actual creation, exact event/booking IDs and nonempty agenda in the second tool, edit, confirmed same-event save, original Notes/Teams preserved, no duplicates and continued conversation; real tenant/calendar actions require the corresponding later direction.

## Final practical checks

- [x] Production build passes and the sppkg/agent ZIP include the required local assets.
- [ ] Workbench accepts skill arguments; inline shows at most three people and the correct total.
- [ ] Fullscreen, both themes, narrow/wide layouts and keyboard/dialog focus are usable.
- [ ] Name prefix, Department, Role and Clear filters work without another data read; mode changes preserve state.
- [ ] Many/one/zero results, loading/error/retry and image fallback are understandable.
- [ ] Root availability selects SharePoint on 200, mocks on 404, and errors on 403/network failures; existing empty lists stay empty and host/filter changes do not repeat the check.
- [ ] Live SharePoint requests are Skill-filtered and obey the actual user's permissions; identity/photos behave correctly.
- [ ] Natural Copilot prompts select the correct tool/skill; unsupported requests do not invent results.
- [ ] Live Teams navigation works for a verified user; mock contacts and Schedule remain clearly simulated.
- [ ] The conference script and discovery fallback have been rehearsed in the available environment.

## Actual results and blockers

2026-11-03:

- DAT-01/DAT-02: implemented the typed skill registry, runtime normalization, provider contract and fictional mock records. TypeScript/lint build passed. Source/fixture review confirms SPFx 6, Copilot 8, DB 1, Spanish 0. Some of the eight fictional people have records for multiple skills; no duplicate person exists within a skill result set.
- DAT-03: generated and visually inspected eight fictional adult headshots. Local copies and provenance are in the app assets folder; the intermediate production package included all eight portrait files. No external image host is used.
- ACT-01, INL-01/02, FUL-01/02: source implemented, including contact/Schedule dialogs, invalid/loading/empty/error/retry states, persistent root/filter state, local filtering and theme/mode adaptation. The complete source passes TypeScript/lint/webpack. Actual host layout, keyboard/dialog behavior and request-count observation remain pending.
- COP-01: descriptions and instructions updated; the generated agent package uses dynamic discovery and preserves identities. COP-02 starters and invocation guide are written; natural selection is not yet verified in Copilot.
- SP-01: list instructions and 15 fictional sample rows are written; Person placeholders require actual authorized tenant users. No tenant provisioning or seeding was performed.
- SP-02/SP-03: the delegated skill-filtered provider now uses automatic root-list selection. Manual source/site settings have been removed. HTTP 404 is the only mock selection condition, and service/isDemo are returned together. Actual tenant 200/404/403 responses, permissions, Person projections, photo access and Teams navigation remain pending.
- DEM-01: final npm run build exited 0 without lint warnings or TypeScript errors. Package/agent ZIP CRC integrity checks passed; the sppkg embeds the generated agent and all eight local portraits. Package size: 16.57 MiB. The compiled Draft 7 schema has exactly one required skill and no message argument. Four starters are packaged. The generated Teams version is 1.0.0.1; component/solution identities are unchanged.
- Local HTTPS: npm start -- --nobrowser compiled successfully outside the restricted sandbox; curl returned HTTP 200 for /temp/build/manifests.js. No browser was opened. The temporary server was stopped after inspection.
- DEM-02: installation/runbook/fallback documents are complete; actual host rehearsal remains pending. This task remains unchecked until the available environment has been rehearsed.
- Computer Use could not obtain permission to control the browser. After explaining its purpose, the implementation continued without it; visual/tenant checks are explicitly pending for the user. No real chat, meeting, tenant change, upload or sync has occurred.
- Existing build tooling was retained. No unit tests, automated integration suites, test libraries, coverage rules, CI or new dependencies were added.

### Automatic root-list selection update (2026-11-03)

The user approved the demo convention: Experts is in the tenant root. Selection runs once in onInit; root is derived from the context URL origin, including Workbench opened on another site. A present but empty list, zero matching items, or later item-read failure never activates mocks. Verification: npm run build exited 0 with no TypeScript/lint errors or warnings. The regenerated sppkg passed archive integrity inspection and includes the eight portraits and agent ZIP. Manual 200/404/403 availability checks and empty-list/host behavior remain unverified in the tenant. No tests, dependencies, browser automation or tenant writes are added.

### Graph profile photo implementation (2026-11-03)

- GP-01: added GraphPhotoService using the installed SPFx MSGraphClientV3 and Graph v1.0 `/users/{UPN}/photo/$value`. Source review confirms one lazy client, per-UPN pending/success/unavailable caching, a five-second logical timeout, no Blob URL creation from stale responses, and URL disposal at teardown. The SharePoint provider no longer builds `userphoto.aspx` URLs. No dependencies or package permission requests were added; the existing compatible consent is reused.
- GP-02: the app renders SharePoint results independently and starts all photos for the small result set in parallel. Photo/image-loading state lives above inline/fullscreen, with circular 48/72 px Fluent Skeletons until image load and initials on failure. A separate five-second image-load limit prevents indefinite shimmer. Mock mode retains local portraits and creates no Graph loader. Compilation/lint passed; actual host visuals, mode/filter behavior and licensed/unlicensed user photos remain pending.
- GP-03: `npm run build` exited 0 with no TypeScript or lint warnings/errors. The generated sppkg and current agent ZIP passed archive integrity inspection; the packaged production bundle includes the Graph photo endpoint and omits the previous SharePoint photo URL. README, setup, deployment and CLI permission docs now describe the implementation. Local Markdown links and `git diff --check` passed. No unit tests, browser automation, tenant changes or deployment were performed.
- Manual acceptance still required in Workbench and Copilot: authenticated SPFx photo access, the unlicensed user's uploaded photo, unavailable-photo fallback, both themes, no repeat Graph loads across fullscreen/filters, and cold/warm first-three-photo timing. The 1–2 second target is unmeasured. Graph Explorer success alone does not verify the component's authorization. GP-02/GP-03 remain unchecked for those host checks.

### Expert Availability Panel implementation (2026-11-03)

- AV-01: added optional SMTP email from Person EMail without changing the Teams/photo UPN, a small typed slot model and GraphAvailabilityService. The service uses the existing delegated SPFx Graph factory and posts exactly one SMTP schedule to `/me/calendar/getSchedule`, with UTC boundaries for local 09:00–18:00 and a 30-minute interval. Source review confirms validated codes, per-schedule error handling, no raw event details in UI state, a ten-second logical timeout and friendly HTTP/mailbox/sharing/service errors. Missing or invalid email is optional in the model and does not fail response-schema validation. Actual SharePoint SMTP projection and calendar access are unverified in the tenant.
- AV-02: implemented the compact panel with 18 text-marked, keyboard-focusable Fluent tooltip slots, a labeled legend, current-slot outline, viewing date/zone, local loading and explicit unavailable/error/Refresh states. Only a local minute clock updates automatically. No calendar cache or Graph polling was added; unavailable/invalid data is never shown as Free. Visual, keyboard and actual host runtime checks remain pending.
- AV-03: connected only the selected fullscreen card. Selection is idempotent, clears when hidden by filters or when fullscreen/skill changes, and old panel requests are ignored through cleanup and identity guards. Mock profiles/missing email make no calendar request. Search, GraphPhotoService and ExpertActions were preserved. Actual A→B, duplicate-click, filter/mode request-count and regression checks remain pending.
- AV-04: TypeScript checks passed after AV-01 and AV-02. Final `npm run build` exited 0 without TypeScript/lint warnings/errors; the existing Jest stage found no tests, and no test suite was added. Both the sppkg and current agent ZIP passed CRC integrity checks; the production bundle contains both calendar and photo endpoints. The package has no webApiPermissionRequests; dependency manifests and existing photo/contact implementation files are unchanged. Documentation links/anchors and nine shell examples passed static validation without executing consent commands.
- Native Date conversion was inspected locally in Node with TZ=Europe/Madrid: Jan 15 maps 09:00–18:00 to 08:00–17:00 UTC; Jul 15 and Mar 29 map to 07:00–16:00 UTC; Oct 25 maps to 08:00–17:00 UTC. All four ranges contain 18 half-hour slots. This is a local time-conversion spot check, not Graph/Outlook/browser acceptance.
- README, SMTP list notes, the scope-preserving M365 CLI guide and the live conference segment are updated. The previously reported Calendars.Read grant has not been rechecked or changed. No live calendar requests, tenant changes, calendar writes, browser automation, new dependencies or unit tests were performed/added.
- Remaining manual acceptance: effective SPFx calendar consent; selected non-family expert with an accessible Exchange mailbox; comparison against Outlook for all four states and time zones/DST; missing email/mailbox and policy/401/403/404/429/empty/per-schedule/network failure states; current-slot boundaries; Refresh; rapid A→B; filter/mode behavior; both themes and narrow cards. AV-02/AV-03/AV-04 remain unchecked until these host checks have actual results.

### Availability period navigation (2026-11-04)

- Updated only ExpertAvailabilityPanel UI/state: 56 px high time-labeled blocks with 18 px status markers; Morning 09:00–14:00 (10 slots) and Afternoon 14:00–18:00 (8 slots); accessible local previous/next buttons disabled at the ends. Initial selection follows the local hour; Refresh and minute clock updates preserve it. The Graph service and its full-day request are unchanged.
- CSS container queries use the panel's own width: one row from 600 px, otherwise two rows of five morning slots or four afternoon slots. Colors, legend, tooltips, current-slot outline and mock/unavailable behavior are retained. README, the demo segment and the existing AV-02/spec text describe the new layout; no new task structure or dependencies were introduced.
- `npm run build` exited 0 without TypeScript/lint warnings/errors and regenerated the sppkg. No new tests or tenant operations were added. Actual arrow/Refresh request-count checks, initial selection around 14:00, keyboard announcements/focus and narrow/wide light/dark host layout remain pending; no browser automation or live host verification was performed for this update.

### Confirmed Teams meeting creation (2026-11-04)

- Added GraphMeetingService, MeetingConfirmationDialog and small typed organizer/result models. Future Free slots become native Fluent buttons only for another expert with resolved SMTP; self checks normalize the SPFx user's email/loginName and expert mail/UPN. Non-bookable cells remain informational and retain their time/status labels. Mock mode constructs no meeting service.
- Confirmation displays parties/date/local time/duration and optional plain-text Notes (1,000 characters). Cancel before confirmation makes no write; pending actions/dismissal are locked. The service checks Teams provider support and both participants' free/busy for the selected half hour before POST /me/events with Teams settings, one expert attendee, UTC times and transactionId.
- Per-instance in-flight/completed/uncertain attempts are guarded by expert/interval. Unknown POST outcomes stay blocked and ask the user to inspect Outlook; a delayed/missing join URL triggers one read of the existing event, never another creation. Calendar results refresh after an attempt without treating the expert as Busy without evidence. Guard state is not persisted across reloads.
- Final npm run build exited 0 without TypeScript/lint warnings/errors. Both archives passed CRC checks; the production bundle includes the create-event path, transactionId, confirmation/Teams links and existing photo/availability endpoints. Dependency manifests, package permission configuration, GraphPhotoService, GraphAvailabilityService and existing ExpertActions are unchanged. Documentation links and ten shell examples passed static checks; no consent example was executed. Initial supported-style/Promise-lib/ref-lint issues were fixed without disabling checks or changing compiler configuration; a transient Heft staging collision did not recur in the final build.
- README, SharePoint setup, CLI permissions and conference runbook now distinguish real confirmed invitations from the existing Schedule preview. The user reports Calendars.ReadWrite granted; no tenant grant was inspected or modified, no real meeting was created, and no browser automation or unit tests were added/run. Manual runtime and end-to-end invitation/link checks remain pending.


### Local availability after confirmed booking (2026-11-04)

- The user reported successful meeting creation but a slot still showing Free, including after Refresh. The previous code refreshed Graph and blocked duplicate requests but rendered only the returned free/busy status. An attendee invitation can also await calendar processing/response; a local booking mark must not imply acceptance.
- GraphMeetingService now records only confirmed creations in a small in-memory Set keyed by normalized expert email and UTC interval. The panel immediately renders those intervals Busy after the success callback, without an additional availability read. Refresh still requests Graph but cannot overwrite the local mark with a stale Free response. The same service retains records across expert switches/filter/mode remounts until component disposal; a full reload clears them. Tooltips and a panel note identify session-created meetings. Subsequent Outlook declines, cancellations and rescheduling are outside this local record's scope.
- Failed, canceled and uncertain attempts do not add confirmed booking records. Existing uncertain-write duplicate protection and error refresh remain intact. Mock mode, source selection, search, photos, contact actions, permissions and dependencies are unchanged; no unit tests or persistent storage were added.
- npm run build exited 0 in an isolated temporary source copy using the workspace's installed dependencies, without TypeScript/lint errors or warnings. The existing Jest stage found zero tests. Both generated archives passed CRC checks and were copied to the app's usual teams/expert-finder.zip and sharepoint/solution/copilot-expert-finder.sppkg paths. Two initial builds in the app directory failed in Copilot staging while a separate heft start was active; the isolated build avoided the shared temp directory without stopping that server or changing build configuration.
- README, the conference runbook and existing feature/global specs describe the local booking behavior. No live Graph calls, calendar writes, tenant changes or browser automation were performed. Actual immediate Busy rendering, persistence after stale-Graph Refresh/expert/mode changes, failure/uncertainty behavior and keyboard focus remain pending in Workbench/Copilot.


### Agent icons and deployment retry (2026-11-04)

- User-provided screenshots show a natural Copilot-agent prompt rendering the directory inline, three of five results with profile images, fullscreen cards in dark theme and a selected expert's availability timeline. This is observed deployed behavior, not an automated runtime check. New-branding publication, light theme, keyboard/focus, error paths and end-to-end meeting/Busy-refresh behavior in Copilot remain pending; the generic Add to Teams failure is not declared resolved.
- Validated the user-replaced copilot/color.png (192 × 192 RGB PNG) and copilot/outline.png (32 × 32 RGBA PNG). Visual inspection confirms the intended color mark; outline inspection confirms 355 visible white pixels, 669 fully transparent pixels and transparent corners. The existing manifest already references both filenames. The React header logo is a separate asset.
- Advanced the app version to 1.0.2 and the SPFx solution version to 1.0.0.2 without changing identities. Found and removed an obsolete tracked teams/expertfinder-agent.zip containing the same app ID at version 1.0.0.2 with default icons; the packager had embedded it alongside the current agent ZIP. A backup was retained outside the repository. Duplicate agent packages are a possible synchronization issue, not a proven explanation of the tenant's 400 response.
- Final npm run build exited 0 in the actual project folder, without TypeScript/lint errors or warnings. The existing Jest stage found zero tests. Archive verification confirms exactly one embedded agent ZIP, app version 1.0.2 and icon bytes identical to the new source PNGs. No dependencies or permission requests were added.
- Regenerated /private/tmp/expert-finder-inheritscloud-1.0.2.zip for the optional Teams Admin Center Upload file retry. It retains the release's app identity/version, definitions and new icons, replacing only the runtime MCP placeholder with the inheritscloud tenant/solution URL. All three manifests pass the installed Microsoft schema validator and both ZIP/sppkg integrity checks pass. The regular package retains its deploy-time placeholder for the standard SharePoint synchronization route. No tenant upload, synchronization, permission change or calendar action was performed by the assistant.
- docs/deployment.md now explains the two icon locations, production build flags, version-state preservation, duplicate ZIP cleanup, manual tenant-bound update route and the observed/pending host evidence.


### Introduce me specification and planning (2026-11-05)

- Read the user-provided feature brief and existing spec/task conventions. Added specs/introduce-me/spec.md and tasks.md, linked the feature in the index/global plan and preserved every prior progress entry. This is documentation/planning only; no application, agent instruction, package configuration or generated artifact was changed.
- Verified the installed SPFx 1.24.1 public declarations: createCopilotTextContent; updateModelContextAsync returning Promise<void>; sendFollowUpMessageAsync returning an acknowledgement with optional isError; request options timeout/signal and replacement-context semantics. Confirmed Sparkle20Regular is already exported by the installed Fluent icons package. The public documentation specifies the 10 KB text-context limit and deployed Copilot acceptance requirement.
- Planned exactly four sequential tasks with existing build/manual validation, no unit tests/dependencies/Graph consent/extra tool/backend. Fullscreen-only UI, shared click guard, five-field authorized profile context, mock labeling, a conservative byte budget, untrusted-profile handling, drafting instructions and local host errors are explicit.
- No Bridge call, LLM draft, build, Workbench/Copilot rehearsal, tenant change, message or calendar action was executed for this planning request. IM-01 through IM-04 remain unchecked until implementation and the corresponding actual checks have evidence.

### Introduce me local implementation (2026-11-05)

- Implemented IM-01 -> IM-04 local scope. The helper publishes only name, jobTitle, department, skill and shortCV with purpose/fictional metadata. The complete context is rejected above 9,000 UTF-8 bytes before host calls; profile strings remain explicitly untrusted data, separate from the fixed prompt.
- The entry awaits updateModelContextAsync before sendFollowUpMessageAsync, checks the acknowledgement's isError, uses 15-second timeout/signal options and aborts at teardown. A synchronous React-root guard serializes card handoffs; only fullscreen cards expose Introduce me, with shared pending state and inline accessible errors. No effect triggers a handoff, no new draft view or Workbench Bridge simulator was added, and mock profiles use the same explicitly fictional context.
- Updated the existing agent drafting branch, invocation guide, demo runbook, deployment notes and code walkthrough. Drafting requests first person, exactly three numbered relevant questions, supported profile facts and the preceding human request's language, without another search or external send. These are model-dependent acceptance criteria, not output validated by the local build.
- Three incremental npm run build executions exited 0; the final production build completed without TypeScript/lint errors or warnings. The existing Jest stage found zero tests; no unit tests, dependencies, permission requests or additional tools were added. Archive CRC checks pass; the sppkg embeds exactly one agent ZIP. Agent version is 1.0.3 and solution version is 1.0.0.3, with existing IDs preserved and icon bytes matching the source. Generated ZIP/version state was regenerated through the existing build. The packaged bundle contains the selected-context and ordered Bridge paths.
- No live Bridge request, LLM draft, browser automation, Workbench/Copilot runtime check, tenant synchronization, permission change or Teams/calendar write was performed. Manual themes/layout/keyboard, same/different-card clicks, failure/oversize/disposal, fictional profiles, actual chat location/context/language/question count/tool diagnostics and regressions remain pending. Deployment must re-sync the baseline agent because instructions changed.


### Introduce me blocked conversation investigation (2026-11-05)

- User evidence: Introduce me posts its follow-up, then Copilot refuses and the conversation accepts no further input, including after closing the component. Pasting the identical follow-up manually in a fresh conversation without the component responds and leaves chat usable. The prompt text alone is therefore less likely; context delivery, Bridge integration and host behavior are not yet isolated. No definitive safety-filter or lifecycle cause is claimed.
- Prepared one controlled component-only change: the context prefix is now a descriptive untrusted-data label without behavioral directives. The five profile fields and purpose/fictional metadata, 9,000-byte guard, full follow-up text, agent instructions, shared concurrency guard, ordered public Bridge calls, cancellation and existing features remain unchanged. No additional close call, delay, retry, dependency, permission or unit test was added.
- npm run build exited 0 without TypeScript/lint warnings or errors; the existing Jest stage found zero tests. ZIP/sppkg CRC checks pass; solution 1.0.0.4 contains one agent ZIP whose entry contents exactly match the prior agent 1.0.3. The compiled bundle contains the new context label and excludes the removed directive. This retry needs only replacing/enabling the sppkg if agent 1.0.3 is already synchronized. No tenant action, live Bridge call or browser automation was performed by the assistant.
- Actual draft generation and continued input in a fresh deployed conversation remain pending. This candidate is not a confirmed fix. Updated the invocation/deployment guide and walkthrough to record the comparison and controlled retry; previous host acceptance checkboxes remain open.

### Meeting agenda development planning (2026-11-12)

- Added ten implementation tasks, their dependency/file-size/acceptance mapping, four technical/final checkpoints and a feature-specific future implementation prompt. Extended the existing plan/index/checklist in place without resetting earlier incomplete tasks or evidence. `AG-*` task IDs are separate from the feature spec's `MA-*` acceptance IDs.
- Scope covers the complete booking-to-Copilot-to-independent-review-to-confirmed-save journey, using event provenance, minimal context, scoped agent routing, body/Teams preservation, explicit save/status recovery and final conversation/regression checks. No spike, preliminary tool-invocation proof, partial-delivery decision or approval checkpoint was introduced.
- Planning only: no application/agent/config code, versions or generated packages changed; no build, Bridge call, tenant change, deployment or real calendar/message operation performed. All agenda implementation/checkpoint entries remain unchecked; previously reported host limitations and unperformed tenant checks remain unresolved.

### Meeting agenda local implementation (2026-11-13)

- Implemented event-local provenance/correlation, authorized agenda reads, independent `PrepareMeetingAgendaTool` review editor, explicit body-only save with preservation/conflict/unknown-status handling, and the confirmed-booking action with a shared ordered Bridge guard. Added scoped agent routing and concise usage/release notes. Reused existing dependencies and delegated permissions.
- Final `npm run build` exited 0 without TypeScript/lint warnings or errors; the existing test stage found zero tests. Both component manifests/bundles and dynamic tool discovery were included; the build generated `teams/expert-finder.zip` (app 1.0.4) and `sharepoint/solution/copilot-expert-finder.sppkg` (solution 1.0.0.5). Initial lint/ESM schema-loading and Promise-library compatibility failures were fixed without suppressions or compiler/dependency changes.
- Stopped after the successful build per the latest user instruction. No new testing files/infrastructure, extra package checks, browser/Workbench/Copilot checks, deployment/sync, consent change or real-calendar/message operation. Full functional/manual task acceptance stays pending for the user, including the known host conversation issue and conditional-write/body-normalization assumptions.

### Add to Teams artifact audit (2026-11-13)

- At the user's request, inspected the actual sppkg and nested agent ZIP after the reported generic Add to Teams failure. Both archives pass CRC checks with no duplicate entries; the sppkg contains exactly one agent ZIP, byte-identical to `teams/expert-finder.zip`, and two intended Copilot components. The agent contains only its three manifests and two icons; the version-history sidecar is not packaged.
- Confirmed stable solution/app IDs, solution `1.0.0.5`, app `1.0.4` (previous tracked app `1.0.3`), matching source/generated/embedded manifests and version history. All three JSON manifests pass the installed Microsoft validator with the MCP placeholder resolved only in a temporary validation copy. No speculative version/ID/schema changes or rebuild were made.
- Read-only Teams catalog lookup could not establish the deployed version because the existing CLI grant lacks an app-catalog read scope. No permission or tenant change was performed. The actual SyncSolutionToTeams response is still needed to distinguish a version conflict from another synchronization error; the generic message is not a proven packaging fault.
- Prepared `/private/tmp/expert-finder-inheritscloud-1.0.4.zip` for the already documented manual update route, retaining five files and changing only the tenant MCP URL. It stays outside `teams/` to avoid reintroducing duplicate ZIP packaging. Deployment guidance records findings, version caveats and the response needed for further diagnosis; host/calendar acceptance remains pending.

- Subsequent user evidence: a new SyncSolutionToTeams attempt returned HTTP 200 and integer `value: 1`, without an error payload. This supports successful completion of that sync call; visibility/version `1.0.4` in the catalog and a fresh Copilot conversation have not yet been observed. The earlier failure's cause remains unknown; no further version/package change was made.

### Remove obsolete Schedule preview (2026-11-13)

- At the user's request, removed the Schedule icon button, its `schedule` dialog state, preview title/content and unused icon import from ExpertActions. Teams contact and its preview/error dialogs, Introduce me, actual availability/booking and agenda preparation are unchanged. Updated current instructions/specs/usage notes; older requirements to preserve Schedule are superseded by this explicit removal.
- Advanced solution to `1.0.0.6` and Teams app to `1.0.5` because the baseline instruction also changed, preserving IDs/schema versions and build version history. `npm run build` passed without TypeScript/lint warnings or errors; the existing test stage found zero tests. No testing infrastructure or live tenant/calendar checks were added/executed.
- The initial build overlapped the active development server and included development hot-update assets. Rebuilt in an isolated temporary copy using the installed dependencies and preserved version history, then copied the clean sppkg/agent ZIP/version state to the normal project paths. The final archive contains no hot-update/unhashed development component bundles and exactly one agent ZIP. No tenant deployment/sync was performed; runtime checks remain with the user.

### Inline frame content-size notification (2026-11-13)

- User screenshot shows the inline frame clipping results after the first profile. The inline row/header styles have not changed in the recent feature work; no definitive host regression is claimed. Added an inline-only ResizeObserver on the natural React content section, forwarding changed positive dimensions to the public requestSizeChangeAsync entry callback. It disconnects on fullscreen/unmount and ignores disposed callbacks; no fixed height, layout redesign or automatic request loop was introduced.
- Follows the official access-data guidance for asynchronous content/frame resizing. Solution advanced to `1.0.0.7`; Teams app stays `1.0.5` because baseline metadata/instructions/tools are unchanged. No additional agent synchronization is needed for this component-only change.
- `npm run build` passed without TypeScript/lint warnings/errors in an isolated temporary source copy, preserving version history and avoiding the active development server's staging files. The existing test stage found zero tests. Clean sppkg/ZIP/version state were copied to the normal project paths; no hot-update assets or duplicate ZIPs are included. Stopped after compilation/packaging; actual three-profile visibility and host height constraints remain for user verification.

### Agenda follow-up routing correction (2026-11-13)

- User reports the visible agenda follow-up followed by a chat-only claim that a review card is ready, with no card displayed. This confirms delivery of the user-role message, not a successful tool invocation/render. Action diagnostics were unavailable to the user; omission, discovery failure and render failure have not been distinguished. No Graph/calendar change or confirmed host root cause is claimed.
- Confirmed both components and the second tool's properties are registered. The installed generator uses manifest tool names, distinct from component aliases, and documents possible runtime suffixes. Kept dynamic discovery and existing IDs/tool names; no static catalog, alternate transport or generator was added.
- Shortened the visible follow-up to the meeting-agenda request, exact identifiers and requested review component. Moved the repeated behavioral wording into stronger intent-based baseline routing, with an explicit real-tool-call requirement, suffix-aware selection, actual agenda content, failure wording and no fictitious card/draft completion acknowledgement. Updated tool/plugin/agent descriptions and usage/spec notes.
- `npm run build` passed without TypeScript/lint warnings/errors in the isolated source copy; no tests were added and the existing stage found zero tests. Copied a clean package with one agent ZIP and no development hot-update files: solution `1.0.0.8`, Teams app `1.0.6`. Baseline sync and a new conversation are required for the instruction change. Stopped after compilation/packaging; whether this changes the real Copilot invocation/rendering behavior remains for user verification.

### Agenda tool catalog fallback after host diagnostics (2026-11-13)

- The user confirms the inline height issue is resolved. Agenda preparation still fails: raw debug info for agent `1.0.6.1` shows empty `matchedFunctionCandidates`, `functionsSelectedForInvocation` and `functionExecutions`. This is evidence of no function match/call in that turn, not a rendering or Graph-save error.
- Read-only tenant inspection confirmed solution `1.0.0.8` is deployed/enabled. Downloaded its sppkg and confirmed byte equality with the local artifact, both component/tool definitions, and one agent ZIP. It also contained development hot-update assets/unhashed bundles from shared watch/build output. A direct MCP tools/list request returned 404, so the live discovery response/underlying service cause remains unknown. No scopes were granted or tenant state changed.
- Selected the installed SPFx generator's `toolDiscovery: "static"` fallback. It generates both function definitions and MCP/UI catalog entries from the existing component schemas, retaining the endpoint, identities, names, arguments and read-only invocation semantics. No hand-maintained schema, alternate transport, generator, prompt change or calendar change was added. Future tool/schema updates require baseline sync while this catalog is pinned.
- `npm run build` passed without TypeScript/lint warnings/errors in an isolated source copy; the existing stage found zero tests and no tests were added. Final solution **1.0.0.9** / app **1.0.7** contains both functions and UI metadata, one embedded agent ZIP and no development assets. Copied clean sppkg/ZIP/version history to the project and kept a stable sppkg at `/private/tmp/expert-finder-1.0.0.9.sppkg`.
- Stopped after the successful build/package inspection. No deployment/sync, live tool call or calendar write was performed. User must update/enable, Add to Teams, verify app version and retry in a fresh conversation; tool invocation/rendering and full agenda acceptance remain pending.


### Today / next working day availability (2026-11-13)

- User requested the proposed small demo enhancement and explicitly asked to stop after compilation. Added Today / Next working day selection with the next date visible, weekday-only local date arithmetic and the existing 09:00–18:00 / 30-minute window. Friday advances to Monday; holidays are not considered. On weekends or from 17:30, when no future slot start remains today, the panel initially selects the next working day and Morning. Today retains hour-based Morning/Afternoon selection; Refresh preserves the chosen day/period.
- Reused the availability service's existing date input, clarifying its parameter name. Reads and errors are keyed by expert email and day, and late responses are ignored when switching. Slot labels include the selected date; booking validation, UTC payloads, duplicate/Busy guards and agenda behavior remain unchanged. No dependencies, permissions, tool schemas or baseline-agent changes were added. Updated the existing availability spec/tasks and usage/deployment notes.
- `npm run build` passed without TypeScript/lint warnings or errors in an isolated source copy, avoiding shared development staging. The existing stage found zero tests; no tests or test infrastructure were added. Final solution **1.0.0.10** retains app **1.0.7** and the pinned tool catalog. Copied clean artifacts/version history to the normal paths; the sppkg contains one agent ZIP and no hot-update/unhashed development bundles. Stable package: `/private/tmp/expert-finder-1.0.0.10.sppkg`.
- Stopped after compilation/packaging. No deployment/sync, browser/runtime check, Graph call or real booking was performed. User will verify Friday/Monday selection, period navigation, stale-response switching and the scheduling/agenda flow. Replace/enable only the sppkg; an already synced app 1.0.7 needs no new Add to Teams operation. The user also confirmed that the agenda review component renders with generated content; saving to the event remains unconfirmed.


### Agenda inline height and false unknown-save correction (2026-11-13)

- User reports a clipped independent agenda frame and confirms that the first Save updates Outlook, while confirmation and Check save status stay unknown; unlocking/reloading and saving again returns alreadySaved. Rendering and the actual calendar write therefore occur, but the confirmation flow is incorrect.
- Reproduced the same predicate behavior locally with existing TypeScript/jsdom dependencies in an ephemeral command: unchanged Notes/Teams anchor and an exact managed agenda, with only HTML indentation/CSS changes, fail the old outside-body signature; the identical-agenda shortcut succeeds. No test file, suite, dependency or infrastructure was added. This isolates a concrete confirmation defect; the tenant's exact returned HTML was not retrieved.
- Excluded non-content head/style/script/title nodes and folded presentation whitespace for the outside-body preservation signature, retaining visible text and anchor URLs in order. A small local check now accepts the reproduced normalization, while changed Notes or anchor destinations remain rejected. Composition still preserves original outside markup; exact agenda, authorized event identity/provenance and Teams join URL verification remain required. Genuine uncertain outcomes keep the existing read-only Check save status/write guard; there is no automatic second PATCH.
- Added the existing inline ResizeObserver/public requestSizeChangeAsync pattern to the independent review card, including content/editor/save-state changes and fullscreen/unmount/disposal guards. No directory redesign, fixed height, tool/schema/baseline or permission changes.
- `npm run build` passed without TypeScript/lint warnings/errors in an isolated source copy; the existing stage found zero tests. Solution **1.0.0.11** retains Teams app **1.0.7**. Copied clean artifacts/version history to normal paths; one embedded agent ZIP, no development hot-update/unhashed bundles. Stable package: `/private/tmp/expert-finder-1.0.0.11.sppkg`.
- Stopped after compilation/packaging. No tenant sync/deployment, real Graph request/write or browser/host verification was performed. User will retry natural frame height, first-save confirmation and body/Teams preservation. Replacing/enabling the sppkg is sufficient for an already synced app 1.0.7.

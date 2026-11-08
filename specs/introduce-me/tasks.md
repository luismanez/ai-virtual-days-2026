# Tasks: Introduce me

Four sequential tasks for the community demo. Follow [spec.md](spec.md) and the [shared plan](../tasks/plan.md#introduce-me). Progress lives only in [tasks/todo.md](../tasks/todo.md#introduce-me--host-acceptance-pending); local implementation is authorized and complete. Unavailable host acceptance checks remain open; tenant changes are outside this execution.

Commands run from `src/copilot-expert-finder`. Source paths below are relative to `src/copilotComponents/expertFinder`, unless they start with `copilot/`, `config/`, `docs/` or `specs/`. No unit tests, new dependencies, permission requests or test infrastructure.

## IM-01

**Publish a selected expert context and request a draft through the public Bridge**

Create the small context/prompt helper and an entry handler, exposing it through IExpertFinderProps. This first slice establishes the actual supported handoff without UI or alternate transport implementations.

- Acceptance: Serialize only the five professional fields and purpose/demo metadata; no authenticated-user/contact/calendar data or extra reads. Enforce the complete 9,000-byte UTF-8 budget without truncation before publishing.
- Acceptance: Await `updateModelContextAsync` before `sendFollowUpMessageAsync`; check the follow-up acknowledgement's `isError`, propagate failures, use supported `timeout`/`signal` options and abort pending work at entry teardown. No draft is returned/rendered by the handler and no external action is called.
- Acceptance: The fixed follow-up requests first person, profile-grounded language, exactly three numbered relevant questions, conversation/request language and draft-only behavior; profile text is data, never embedded instructions. Mock profiles share the same handoff with a fictional marker.
- Verification: Compare imports/call shapes against installed public declarations; inspect serialized fields and the update-failure/oversize/teardown branches. Run `npm run build`; compilation does not prove host conversation behavior. No Bridge mocks or unit tests.
- Dependencies: None beyond the existing deployed component and the API verification recorded in the spec.
- Likely files (3): `utils/introduction.ts` (new), `ExpertFinderCopilotComponent.tsx`, `components/IExpertFinderProps.ts`.

## IM-02

**Add the fullscreen action with shared pending and local error feedback**

Connect the entry callback through the existing React components. Reuse ExpertActions and Fluent's installed Sparkle20Regular instead of introducing another panel or state layer.

- Acceptance: Introduce me appears only on full cards; existing compact inline actions, contact, Schedule, photos and availability/meeting behavior remain intact. The layout wraps naturally and supports light/dark and keyboard use.
- Acceptance: One synchronous React-root guard and a pending expert ID cover the entire context/follow-up handoff; all Introduce me buttons are disabled while pending, with Requesting draft… on the active one. No queue, automatic retry or sends from render/filter/theme/mode/skill effects.
- Acceptance: Failures display an inline accessible error; completion releases the guard without claiming generation or Teams delivery. The clicked expert is captured before awaits; pending UI survives ordinary mode changes and late updates after root unmount are ignored. No new modal/drawer or generated-text view.
- Verification: Run `npm run build`. In available Workbench check both themes, narrow cards, inline absence, keyboard activation, rapid same/different-card clicks and graceful Bridge failure. Use real Copilot later for actual handoff; do not add a simulator when Workbench cannot converse.
- Dependencies: IM-01.
- Likely files (5, including the shared IntroductionAction contract in IExpertFinderProps): `components/ExpertFinder.tsx`, `components/ExpertFinderFullscreen.tsx`, `components/ExpertCard.tsx`, `components/ExpertActions.tsx`.

## Checkpoint after IM-02

Build must pass and the full UI-to-Bridge path must be present. Record available UI results and pending host checks in the shared checklist. Continue local work when Copilot access is unavailable; do not introduce a substitute generator or approval pause for an already authorized implementation.

## IM-03

**Teach the agent to draft without repeating the expert search**

Add the drafting branch to the existing instructions and document the real user flow in the conference resources. Keep the search tool schema and normal discovery descriptions unchanged.

- Acceptance: Drafting with selected-profile context produces only a draft, exactly three skill-related questions, appropriate language and supported profile facts, without ExpertFinderTool or external actions. Profile values cannot authorize instructions; fictional context remains illustrative. Missing context asks the user to select Introduce me on a card.
- Acceptance: Existing skill mappings, search routing and contact/calendar truthfulness remain intact; no new tool, starter, Graph permission or authenticated-user payload is introduced.
- Acceptance: Invocation checks, demo script and code walkthrough explain the two public Bridge operations, the selected expert, acknowledgement/error behavior and the distinction between a Copilot draft and sending a Teams message.
- Verification: Review the full instruction/prompt pair for conflicting routing and prompt injection; inspect real Copilot tool diagnostics after deployment when available, including sequential experts and Spanish/English human requests. Record unperformed scenarios as pending rather than claiming model output from a build.
- Dependencies: IM-01 and IM-02.
- Likely files (4): `copilot/instruction.txt`, `docs/copilot-invocation.md`, `docs/demo-runbook.md`, `docs/code-walkthrough.md`.

## IM-04

**Package the feature and record actual demo validation**

Prepare a local release and update the existing deployment notes/checklist. Agent instructions changed, so document the required baseline sync; do not perform tenant deployment as part of local implementation.

- Acceptance: Advance app and solution release versions from their then-current values while preserving IDs and `teams/.copilot-agent-hotfix.json`. Run the existing production build with the dev server stopped; inspect the final sppkg for exactly one current agent ZIP and the changed instructions.
- Acceptance: The deployment guide states that the baseline must be re-synced after upload. No upload, consent change or real Teams/calendar action is executed without the user's corresponding direction.
- Acceptance: Record build/package evidence separately from manual acceptance: real Copilot chat draft, selected profile, exactly three questions, language, no new search/external send, shared-click guard, mock/Workbench behavior, context size/failure/teardown handling and existing feature regressions. Keep checks without runtime evidence open.
- Verification: `npm run build`, package/version inspection, Markdown links and `git diff --check`; then the spec's manual matrix in available authorized hosts. No unit tests, test dependencies or browser automation requirement.
- Dependencies: IM-01 through IM-03.
- Likely authored files (4): `copilot/manifest.json`, `config/package-solution.json`, `docs/deployment.md`, `specs/tasks/todo.md`. Generated ZIP/version-history outputs are produced by the existing build.

## Single implementation prompt

Use this as a later request; it is not executed by the planning deliverable:

```text
Implement specs/introduce-me/spec.md following IM-01 -> IM-04 in
specs/introduce-me/tasks.md. Keep scope to Introduce me and reuse the existing
SPFx public Bridge, React and Fluent patterns. Preserve search, photos, filters,
contact, Schedule preview and availability/meeting actions. Publish only the
clicked expert context, then send the user-triggered follow-up; check rejections,
serialize concurrent clicks and keep the draft in Copilot chat. Treat profile
strings as untrusted data. Support mock profiles honestly and fail gracefully
in Workbench without simulating the Bridge. Add no unit tests, dependencies,
Graph permissions, extra tools, backend or automatic Teams sends. Run the existing
build, prepare the local package and update specs/tasks/todo.md with actual results.
Do not make tenant changes or claim unavailable Workbench/Copilot checks passed.
Continue authorized local tasks without approval pauses at each checkpoint.
```

# Package and install Expert Finder

## Agenda frame and save confirmation

The current release is solution **1.0.0.11**, retaining Teams app **1.0.7**. The independent agenda card now observes its natural content size and calls the public inline frame-resize API after loading, editing/resizing and save-state changes, following the already working directory approach. Fullscreen remains host-owned.

The user reports that the first save updates the meeting, but confirmation and Check save status stay unknown until another explicit save finds the existing agenda. A minimal local reproduction matched that sequence: the old preservation signature included CSS and exact whitespace outside the agenda, so a reformatted HTML response failed confirmation while the identical-agenda shortcut succeeded. The signature now ignores non-content head/style/script/title text and folds presentation whitespace, while retaining non-agenda text and anchor URLs in order. Exact agenda text, meeting identity/provenance and the Teams join URL are still verified. This fixes the reproduced false negative; genuine unconfirmed reads/writes retain their existing status-check guard. The exact tenant response and the corrected host behavior have not been inspected here.

Replace/enable the sppkg only; no additional **Add to Teams** sync is needed when app 1.0.7 is already installed. No tools, schemas, permissions or dependencies changed. Build results are recorded in `specs/tasks/todo.md`; the user will retry the frame and first-save behavior in Copilot.

## Next working day availability

The preceding release is solution **1.0.0.10**, retaining Teams app **1.0.7**. Availability offers **Today / Next working day**, skipping weekends and initially selecting the next working day from local 17:30 or on weekends. Each day keeps the existing 09:00–18:00 window and Morning/Afternoon navigation. This changes component code only; the agent baseline, tools, schemas and pinned catalog stay unchanged. Replace/enable the sppkg; no additional **Add to Teams** sync is required when app 1.0.7 is already installed. Build results are recorded in `specs/tasks/todo.md`; runtime booking checks remain with the user.

## Agenda tool catalog correction

The preceding release is solution **1.0.0.9** and generated Teams app **1.0.7**. The user's raw debug info for agent `1.0.6.1` has empty `matchedFunctionCandidates`, `functionsSelectedForInvocation` and `functionExecutions`. A read-only download confirmed that deployed solution `1.0.0.8` contains both tools and matches the local package. This places the observed failure before component execution; it does not establish the underlying Microsoft service cause. A direct MCP `tools/list` request returned 404 and could not verify discovery. The user subsequently confirmed that the independent agenda review component renders with generated content; saving the agenda to the meeting is still unconfirmed.

This release selects the installed SPFx generator's `toolDiscovery: "static"` fallback in `config/copilot-agent.json`. The generator derives both `functions` and `mcp_tool_description.tools` from the existing component manifests/schemas, with explicit `run_for_functions` entries. The MCP endpoint, component IDs, tools, arguments, instructions and calendar behavior stay the same. No catalog is maintained by hand. This is a deployment correction to test, not a confirmed host fix. Replace/enable the sppkg, run **Add to Teams** to synchronize the changed baseline, verify app **1.0.7** or its generated hotfix version, then start a new conversation. With the pinned catalog, future tool/schema changes also require a baseline update/sync.

The downloaded `1.0.0.8` package also contained development hot-update files and unhashed bundles from shared watch/build output. The new production package was built in an isolated source copy using the existing dependencies/version history. `npm run build` passed, including the installed package validators; archive inspection confirmed both generated functions/UI definitions, one embedded agent ZIP and no development assets. Clean artifacts and version history were copied to the normal project paths. A stable copy is also at `/private/tmp/expert-finder-1.0.0.9.sppkg`. Stop the development server before future production builds in the normal project folder; otherwise it can repopulate the shared staging folders. Use the supplied release artifact directly instead of rebuilding while `npm start` is running. No deployment/sync, permission change, live tool invocation or calendar write was performed.

## Agenda routing correction

The preceding release is solution **1.0.0.8** and authored Teams app **1.0.6** (deployed as **1.0.6.1**). It shortened the visible agenda follow-up, strengthened intent-based tool routing/descriptions and prohibited invented card/draft completion acknowledgements. It retained dynamic discovery. The subsequent raw diagnostics above show that this did not resolve the agenda invocation.

## Inline content height update

The preceding component-only release is solution **1.0.0.7**, retaining Teams app **1.0.5** and all identities. It explicitly reports inline content-size changes to the host after asynchronous results/layout updates. That change alone needed no new Add to Teams synchronization; the later agenda releases above do change the baseline. The user has confirmed that the inline height issue is resolved.

## Schedule preview removal

The preceding release uses solution **1.0.0.6** and Teams app **1.0.5**. It removes the obsolete Schedule preview button, its dialog state/content and the corresponding agent instruction. Real booking through Check availability is unchanged. App/solution/component IDs and schema versions are preserved. Because that baseline instruction changed, update/sync the agent if Teams app 1.0.5 has not yet been published. Earlier release/audit entries below describe their original versions.

## Meeting agenda release

The initial agenda release used solution `1.0.0.5` and authored app `1.0.4`, preserving existing identities/history and adding a separate agenda component. After the normal package upload/enable step, synchronize the baseline agent again because instructions/descriptions changed. It originally used dynamic discovery; the later release above selects the generator's catalog fallback after the user's failed host invocation. No deployment/synchronization or live calendar action was performed by this implementation. Per the user's request, verification stops when the existing build passes; host/calendar checks are left to the user. The Introduce me host refusal remains unresolved.

## Add to Teams package audit (2026-11-13)

The reported failure occurs after SharePoint upload/enable succeeds. The local artifact audit found no duplicate agent ZIP or inconsistent release identity/version:

| Artifact | Verified contents/version |
| --- | --- |
| `sharepoint/solution/copilot-expert-finder.sppkg` | CRC valid, no duplicate ZIP entries; solution ID `aa4326b3-eeff-4ca7-8f74-f695935a8bbb`, version `1.0.0.5`; exactly two Copilot component definitions and one `ClientSideAssets/expert-finder.zip`. |
| Embedded agent ZIP | Byte-identical to `teams/expert-finder.zip`; CRC valid; exactly `manifest.json`, `declarativeAgent.json`, `ai-plugin.json`, `color.png`, `outline.png`. No obsolete ZIP, source, build-state or validation file is embedded. |
| Teams app manifest | Existing app ID `9b076d6a-9c80-4c1d-ae8b-83dd4c3184f0`, version `1.0.4`, advanced from the previous tracked `1.0.3`. Source, generated ZIP and embedded ZIP agree. |
| Version state | `teams/.copilot-agent-hotfix.json` records authored/shipped `1.0.4` and is not in the archives. The component/npm version `0.0.1`, app release version and solution release version are separate fields; they need not match numerically. Schema versions remain Teams `1.29`, declarative agent `v1.8`, plugin `v2.4`. |

All three agent JSON manifests passed the installed Microsoft schema validator; the plugin was validated with a resolved URL in a temporary copy, preserving the normal source/package placeholder. The instructions contain 4,747 characters. Packaged icons match the source at 192×192 and 32×32. The app contains no Teams tabs, bot or compose extension; two Copilot components alone do not establish the documented combined Copilot/Teams sync limitation. The remaining outer-package JavaScript, localization, mock portraits/logo, license notices and SharePoint XML are used resources or packaging metadata, not obsolete apps.

No application/configuration/version change or rebuild was justified by these findings. They do not prove that the version currently installed in the tenant is older: a read-only CLI lookup failed because the existing connection lacks an app-catalog read scope. No permissions were granted or tenant writes attempted.

To identify the generic error, inspect the failed **SyncSolutionToTeams** request in the app catalog browser's Network tab and retain its status/response/correlation information, excluding credentials. Microsoft specifically documents that the generic message can hide a nested **409 Conflict** caused by an already deployed app ID/version. If the deployed app is already `1.0.4` or later, compare that actual version before preparing a higher-version release; do not change IDs or schema versions to bypass the conflict. See [Add to Teams fails](https://learn.microsoft.com/en-us/sharepoint/dev/spfx/copilot/troubleshooting-copilot-ux-components#add-to-teams-fails).

A tenant-bound manual-update copy is ready at `/private/tmp/expert-finder-inheritscloud-1.0.4.zip`, outside `teams/` so it cannot be accidentally embedded in a later sppkg. It has the same five files, ID/version/instructions/icons; only the MCP URL is resolved to the documented conference tenant/solution. Use the existing app's **Upload file** route only when its deployed version permits this update, following [the manual update route](#manual-agent-update-fallback). The regular ZIP intentionally retains `{{TENANT_MCP_URL}}` for SharePoint to resolve. No successful tenant update or root cause is claimed from the local audit.

**Subsequent user evidence:** a new `SyncSolutionToTeams` attempt returned HTTP **200** with `{"@odata.context":"…#Edm.Int32","value":1}`. That attempt no longer reports the earlier synchronization error; [PnP Framework's implementation](https://github.com/pnp/pnpframework/blob/dev/src/lib/PnP.Framework/ALM/AppManager.cs) treats a successful HTTP status with a response body as a successful sync call. The exact integer result is not an app version. Confirmation of catalog version `1.0.4` and availability in a fresh Copilot conversation remain user checks; the cause of the earlier intermittent failure is unknown. No package/version change is justified by this successful response alone.

## Build locally

Run from src/copilot-expert-finder:

```bash
npm install
npm run build
```

The existing build compiles TypeScript, lints, bundles, expands the declarative agent and creates `sharepoint/solution/copilot-expert-finder.sppkg`. The generated agent ZIP is `teams/expert-finder.zip`. Portraits are bundled as client assets. Do not edit generated manifests or archives directly.

Component ID: 980d322c-2c03-4d8b-a05a-6b724ee174a1. Solution ID: aa4326b3-eeff-4ca7-8f74-f695935a8bbb. Existing identities are preserved. The toolchain manages the generated Teams manifest version when content changes.

Stop `npm start` before running the production build: both commands share staging folders. The `--production` flags are already included in `npm run build`; no separate `--ship` command is needed.

## Agent branding and update versions

- `copilot/color.png`: 192 × 192 PNG color icon for the agent.
- `copilot/outline.png`: 32 × 32 PNG with a white symbol and transparent background.
- `copilot/manifest.json` already references these two filenames. The logo inside the React UI is a separate asset at `src/copilotComponents/expertFinder/assets/expert-finder-logo.png`.
- Agent branding changes require rebuilding the package, replacing/enabling the sppkg and syncing the baseline agent again. Preserve app/solution/component IDs. Advance the numeric app version in `copilot/manifest.json` and the solution version in `config/package-solution.json`; do not change the declarative-agent schema version as a release counter.
- Keep `teams/.copilot-agent-hotfix.json` when building from an isolated copy. It holds local version history; omitting it can reset the generated app version. Always inspect the manifest embedded in the final sppkg before uploading.
- Keep only the current `teams/expert-finder.zip` for this agent. The SPFx packager embeds other ZIP files left in `teams/` too. An obsolete `teams/expertfinder-agent.zip` with the same app ID and version `1.0.0.2` was removed during this update; the next sppkg must contain exactly one agent ZIP.

The 2026-11-04 branding update uses app version `1.0.2` and solution version `1.0.0.2`. Both source icons were visually inspected; PNG dimensions and white/transparent outline pixels were checked locally. Reference: [Microsoft 365 app icon requirements](https://learn.microsoft.com/en-us/microsoft-365/extensibility/schema/root-icons?view=m365-app-1.29).

## Validate locally in Workbench

```bash
npm start -- --nobrowser
```

Open the authenticated licensed tenant's `/_layouts/15/copilotworkbench.aspx`, load the local debug manifests from `https://localhost:4321/temp/build/manifests.js` as prompted by Workbench, select ExpertFinderTool and fire `{"skill":"SPFx"}`. A trusted local HTTPS development certificate is required. Setting SPFX_SERVE_TENANT_DOMAIN supplies the tenant domain when opening the configured initial page automatically.

The component always checks the tenant root, even when this Workbench is opened on another site. Reload the component after creating the Experts list; source selection runs once per instance. Choose host inline/fullscreen and light/dark modes. Check compact rows, expansion, local filters, clear, dialogs and small widths. No additional preview app or hosting project is required. If an agent sandbox cannot watch files, run the same command in the normal authorized terminal rather than altering the project watch configuration.

## Tenant installation (manual, separately authorized)

1. Choose the tenant setup for the demo. An existing Experts list in the root selects SharePoint; an HTTP 404 selects mock data. Follow sharepoint-setup.md to prepare real data. The same package needs no tenant-specific URL changes.
2. Before distributing, replace placeholder publisher/contact/website/privacy/terms metadata in copilot/manifest.json, copilot/ai-plugin.json and config/package-solution.json with reviewed real values. These were not fabricated for this demo.
3. Upload the sppkg to the tenant SharePoint App Catalog and enable it for all sites as required by the Copilot integration.
4. Perform the initial Add to Teams / baseline agent synchronization from the App Catalog, following your tenant's current controls.
5. Allow policy/propagation time, open the agent in Microsoft 365 Copilot, start a new conversation, and follow copilot-invocation.md.
6. When changing the baseline agent (for example instructions, descriptions or starters), synchronize the updated baseline as required. Ordinary component/tool parameter changes still require a rebuilt/redeployed solution but use dynamic tool discovery.

If upgrading an already deployed package, increase the solution/package version as required by the tenant before uploading the new build. Keep identities stable. This implementation does not upload, sync, grant permissions, create lists or modify the tenant automatically.

## Introduce me release

The initial Introduce me release uses app version `1.0.3` and SharePoint solution version `1.0.0.3`, retaining the existing IDs, icons and single generated agent ZIP. This release changes the agent's drafting instructions as well as the UI, so replace/enable the sppkg and select **Add to Teams** again. Keep the existing agent in Copilot, allow propagation, reload and start a new conversation.

The earlier solution `1.0.0.4` was a component-only retry: its model-context label contains no behavioral directives. Agent `1.0.3` and its instructions were unchanged for that retry. If that agent version was already synchronized, that release only required replacing/enabling the sppkg. The later meeting-agenda release changes the baseline and requires the update described above. The reported conversation block is still under investigation; see the [recorded comparison and retry](copilot-invocation.md#investigating-the-rejected-follow-up).

Search for a supported skill, open fullscreen and choose **Introduce me**. Validate that Copilot receives only the selected profile and produces the draft in the conversation with exactly three questions, without another search or external message. Use the [invocation checks](copilot-invocation.md#introduce-me-contextual-drafting) and [demo segment](demo-runbook.md#introduce-me-segment). No additional Graph consent is required for the action. Workbench can show local UI/errors; only deployed Copilot verifies the conversation result.

These are release instructions, not evidence of a tenant upload or successful draft. The implementation only prepares the local package; actual host results remain in the shared feature checklist.

## Manual agent update fallback

If Add to Teams returns a generic server-side error, the app's existing Teams Admin Center details page offers **New version → Upload file**. Use a tenant-bound copy of the generated agent ZIP for that route, not the unmodified `teams/expert-finder.zip`: its `{{TENANT_MCP_URL}}` is normally resolved by SharePoint during synchronization.

For the conference tenant, that runtime URL is `https://inheritscloud.sharepoint.com/_api/mcp/beta/spfx?solutionId=aa4326b3-eeff-4ca7-8f74-f695935a8bbb`. Prepare a separate ZIP that replaces only the placeholder, retaining the app ID, release version, icons and agent definition. Validate all three manifests locally, then select the copy in **Upload file**. Tenant-specific copies are deployment artifacts, not edits to the reusable source manifest. A local schema pass does not establish that the tenant will accept the upload; inspect any service validation response.

On 2026-11-04 the user supplied Copilot screenshots showing natural invocation, inline/fullscreen rendering in dark theme, tenant expert records/profile images and a calendar timeline. These demonstrate a working deployed experience even though Add to Teams reported an error; they do not establish that the newer agent metadata/icon was published. Actual acceptance of the branding update, other themes/keyboard behavior and end-to-end meeting creation remain separate manual checks.

References: [Update a custom app in Teams Admin Center](https://learn.microsoft.com/en-us/microsoftteams/teams-custom-app-policies-and-settings#update-a-custom-app-using-teams-admin-center) and [SPFx agent-to-solution binding](https://learn.microsoft.com/en-us/sharepoint/dev/spfx/copilot/troubleshooting-copilot-ux-components#the-agent-calls-an-old-solution).

## Graph profile photos

Live photos use delegated Graph access managed explicitly through M365 CLI. Follow [Graph photo permissions with M365 CLI](graph-permissions.md) to inspect the SPFx grant, reuse an existing compatible permission, or grant `ProfilePhoto.Read.All` while preserving existing scopes. The conference tenant reuses `User.ReadBasic.All`. This workflow does not add a photo permission request to the solution manifest for approval in SharePoint Admin Center.

After loading the rebuilt component, invoke SPFx with the real Experts list. Check a licensed user and the unlicensed user whose Graph photo was verified: cards should appear first, then photos replace circular shimmers. Expand, filter and switch themes; loaded photos should remain without new Graph requests. Also check an unavailable photo falls back to initials. Record cold/warm photo timing in Copilot; about 1–2 seconds after the cards is a demo target, not a verified guarantee. Reload creates a fresh component cache. These host checks remain pending until performed in the tenant.

Reference: [Build and deploy a Copilot UX component](https://learn.microsoft.com/en-us/sharepoint/dev/spfx/copilot/get-started/build-your-first-copilot-app).

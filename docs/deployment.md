# Package and install Expert Finder

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

The current local solution `1.0.0.4` is a component-only retry: its model-context label contains no behavioral directives. Agent `1.0.3` and its instructions are unchanged. If that agent version is already synchronized, replace/enable the sppkg and test in a new conversation without another Add to Teams. The reported conversation block is still under investigation; see the [recorded comparison and retry](copilot-invocation.md#investigating-the-rejected-follow-up).

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

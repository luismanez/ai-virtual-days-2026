# Graph photo and calendar permissions with M365 CLI

The component loads live profile photos, selected-expert calendar free/busy and explicitly confirmed Teams meetings through Graph v1.0 using SPFx's MSGraphClientV3. This guide covers their separate delegated permissions. SharePoint supplies expert records, UPNs and SMTP email; mock portraits remain local and mock profiles make no calendar requests.

Assume M365 CLI is already configured, authenticated and authorized to inspect service principals and manage delegated consent in the target tenant. CLI installation, application configuration and sign-in are outside this guide.

## Choose the permission

The minimum dedicated delegated permission for `GET /users/{userPrincipalName}/photo/$value` is `ProfilePhoto.Read.All`. Existing compatible permissions, including `User.ReadBasic.All` and `User.Read.All`, also authorize this operation. Reuse a compatible permission instead of adding a redundant grant. See the [Graph photo API permission table](https://learn.microsoft.com/en-us/graph/api/profilephoto-get?view=graph-rest-1.0#permissions).

For the conference tenant, the existing SPFx grant includes `User.ReadBasic.All`. We reused that authorization and did not add `ProfilePhoto.Read.All`.

Photo consent belongs to the SPFx service principal used by `MSGraphClientV3`. The CLI application is the administrative tool that manages this consent. This demo uses explicit CLI provisioning rather than adding a photo permission request to `package-solution.json` for approval in SharePoint Admin Center.

### Calendar availability permission

For `POST /me/calendar/getSchedule`, the current [endpoint permission table](https://learn.microsoft.com/en-us/graph/api/calendar-getschedule?view=graph-rest-1.0#permissions) lists delegated `Calendars.ReadBasic` as the minimum; existing `Calendars.Read` or `Calendars.ReadWrite` also work. The older free/busy overview still says Calendars.Read is the minimum; use the current endpoint table. The previously inspected conference SPFx grant contained `Calendars.Read`. Recheck it with step 2; if still present, no additional calendar consent is needed. Photo permissions alone do not authorize calendar reads.

If no compatible calendar permission exists, an authorized administrator can grant `Calendars.ReadBasic` using the same scope-preserving CLI procedure below. The component performs no consent operation and adds no `webApiPermissionRequests`. Consent alone does not provision a mailbox or override Exchange sharing policies.

### Teams meeting creation permission

`POST /me/events` requires delegated `Calendars.ReadWrite`. `Calendars.Read` and `Calendars.ReadBasic` only cover the read feature. The [calendar create-event API](https://learn.microsoft.com/en-us/graph/api/user-post-events?view=graph-rest-1.0#permissions) supports Teams meetings through `isOnlineMeeting: true` and `onlineMeetingProvider: teamsForBusiness`; this calendar-backed path does not require a separate OnlineMeetings.ReadWrite grant. Events with attendees send real invitations.

On 2026-11-04 the user reported granting the calendar permission for this feature. No consent change or live meeting creation was performed by the implementation session. Reuse that grant; inspect it with step 2 if needed. Existing compatible read scopes do not need to be removed. Calendar Teams-provider support, mailbox access and actual event/link creation remain host checks.

Choose the missing permission for the operation being enabled before step 3. For photos:

```bash
requiredScope="ProfilePhoto.Read.All"
```

For calendar availability instead, use:

```bash
requiredScope="Calendars.ReadBasic"
```

For confirmed Teams meeting creation, only when Calendars.ReadWrite is absent:

```bash
requiredScope="Calendars.ReadWrite"
```

Only use a branch when its compatible permission is absent. Keep all unrelated scopes, including the existing photo grant.

## 1. Find the tenant-specific Object IDs

Use [entra enterpriseapp get](https://pnp.github.io/cli-microsoft365/cmd/entra/enterpriseapp/enterpriseapp-get/) to inspect the current SPFx principal:

```bash
m365 entra enterpriseapp get \
  --id "08e18876-6177-487e-b8b5-cf950c1e598c" \
  --query "{name:displayName,appId:appId,objectId:id}" \
  --output json
```

The expected name is `SharePoint Online Web Client Extensibility`. Microsoft documents this principal in its [Copilot UX component permission guidance](https://learn.microsoft.com/en-us/sharepoint/dev/spfx/copilot/troubleshooting-copilot-ux-components#a-microsoft-graph-permission-cant-be-approved).

Find Microsoft Graph by name:

```bash
m365 entra enterpriseapp get \
  --displayName "Microsoft Graph" \
  --query "{name:displayName,appId:appId,objectId:id}" \
  --output json
```

Check that the returned Graph `appId` is `00000003-0000-0000-c000-000000000000`. Querying by name avoids the `Invalid UUID` validation error observed when passing that special App ID to `--id` in the demo CLI.

Copy the two returned `objectId` values into these shell variables. Object IDs belong to this tenant; they are different from the fixed App IDs above.

```bash
spfxPrincipalId="<SPFX_SERVICE_PRINCIPAL_OBJECT_ID>"
graphPrincipalId="<GRAPH_SERVICE_PRINCIPAL_OBJECT_ID>"
```

## 2. Inspect the existing delegated consent

Use [entra oauth2grant list](https://pnp.github.io/cli-microsoft365/cmd/entra/oauth2grant/oauth2grant-list/) to select the Graph grant with `consentType` equal to `AllPrincipals`:

```bash
m365 entra oauth2grant list \
  --spObjectId "$spfxPrincipalId" \
  --query "[?resourceId=='$graphPrincipalId' && consentType=='AllPrincipals'].{grantId:id,scope:scope}" \
  --output json
```

For photos, `ProfilePhoto.Read.All` or a compatible scope such as `User.ReadBasic.All` is sufficient; this was the conference tenant's outcome. For availability, look separately for `Calendars.ReadBasic`, `Calendars.Read` or `Calendars.ReadWrite`. Creating meetings specifically requires `Calendars.ReadWrite`. If the required permission for the selected operation is present, no change is needed. Do not infer calendar write access from successful photos or free/busy reads.

If a grant is returned without a compatible permission, follow the existing-grant branch below. If the result is `[]`, follow the new-grant branch.

## 3. Grant the selected access only when missing

### Update an existing grant

Copy its `grantId` and the entire current `scope` value from step 2:

```bash
grantId="<EXISTING_GRANT_ID>"
existingScopes="<EXACT_CURRENT_SCOPE_VALUE>"

m365 entra oauth2grant set \
  --grantId "$grantId" \
  --scope "$existingScopes $requiredScope"
```

The [set command](https://pnp.github.io/cli-microsoft365/cmd/entra/oauth2grant/oauth2grant-set/) replaces the scope list. Keeping the full existing value and appending the selected missing permission preserves the other grants. Scopes are separated by spaces. This consent is shared by SPFx solutions using the same principal in the tenant.

### Create a grant when none exists

Only when step 2 returned `[]`, use [entra oauth2grant add](https://pnp.github.io/cli-microsoft365/cmd/entra/oauth2grant/oauth2grant-add/):

```bash
m365 entra oauth2grant add \
  --clientId "$spfxPrincipalId" \
  --resourceId "$graphPrincipalId" \
  --scope "$requiredScope"
```

Here, `--clientId` and `--resourceId` are service-principal Object IDs. Use `oauth2grant` for the component's delegated access; `entra approleassignment add` grants application permissions and is not the consent model used by this component.

## 4. Verify the result

Rerun the query from step 2. Confirm that the grant includes the selected permission or a compatible scope for that operation. After an update, also confirm that the previous scopes are still present.

This verifies consent configuration. Validate the authenticated photo request from the component in Workbench and Copilot, including the unlicensed demo user with an uploaded photo. Cards render independently; avatars show a circular shimmer, then the photo or initials on failure. Filters and inline/fullscreen reuse the per-instance photo cache. Reload after changing consent or uploading a missing photo to retry an unavailable result. Graph Explorer has its own consent and does not establish that the SPFx client can make the same call.

Implementation reference: [Use MSGraphClientV3 in SPFx](https://learn.microsoft.com/en-us/sharepoint/dev/spfx/use-msgraph). Actual component authorization and cold/warm photo latency still require a host check; a successful local build does not verify them.

For availability, select Check availability on a real expert's fullscreen card. Confirm that one selected SMTP address is sent to `/me/calendar/getSchedule`, the local 09:00–18:00 range is converted to UTC, and only free/busy states appear. The caller's calendar access, expert mailbox/sharing, HTTP/per-schedule failures and A→B selection behavior require live validation. An unlicensed user whose Graph photo works may still lack an accessible Exchange mailbox; that is an unavailable calendar state. Follow the [live demo segment](demo-runbook.md#live-calendar-availability-segment). No calendar consent command or live calendar request was executed during local implementation.

For meeting creation, choose a future Free slot belonging to another controlled demo expert and inspect the confirmation before creating it. This action sends a real invitation. Check Teams support, both-calendar revalidation, the returned onlineMeeting.joinUrl, duplicate-click blocking and uncertain-response messaging. No real meeting or permission change was executed as a build check. Cancel any rehearsal meeting manually in Outlook when finished if appropriate; the component has no cancel-event or delete-event operation.

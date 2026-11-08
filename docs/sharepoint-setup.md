# Experts list setup

The component selects its data source automatically by checking the list named Experts in the tenant root site. List creation and seeding are manual tenant-owner steps; SharePoint list data is only read. Separately, an explicit meeting confirmation can create a calendar event and send an invitation through Graph.

## Create the list

1. Open the tenant root site, for example `https://contoso.sharepoint.com`. The list must be here, not under `/sites/demo` or another site. Use an account with the required access.
2. Create a blank list named Experts. Keep the Title internal name and use it for the job title.
3. Create the following columns using the exact names below before renaming any display labels.

| Internal name | Type | Setting |
|---|---|---|
| Title | Single line text | Required; job title |
| Expert | Person | Required; one person, people only |
| Skill | Choice | Required; one value; SPFx, Copilot, DB, Spanish; fill-in disabled |
| ShortCV | Multiple lines | Plain text |
| Department | Single line text | Department/team |

Use roughly 15 rows. A person may have separate rows for different skills, but avoid duplicate rows for the same person/skill. `experts-sample-data.json` contains fictional example rows, not an import script. Replace ExpertName placeholders with actual authorized tenant test users in the Person picker; do not create accounts or copy fictional names into claims. Row counts for a skill depend on the records the current user can read.

## Automatic source selection

There is no site URL or data-source setting to edit. The same build can be installed in several tenants.

At initialization the factory derives the root from `new URL(this.context.pageContext.web.absoluteUrl).origin` and sends:

```http
GET https://YOUR-TENANT.sharepoint.com/_api/web/lists/getbytitle('Experts')?$select=Id
```

| Response | Selected source |
|---|---|
| HTTP 200 | SharePointExpertService; the header shows SharePoint |
| HTTP 404 | MockExpertService; the header shows Demo data |
| Any other HTTP failure or network error | An error is displayed; mock data is not substituted |

To use real data, create/populate Experts in the root and reload the component. To demonstrate mocks, use a controlled tenant where this availability request returns HTTP 404. A network connection is needed for this initial check even when mocks are eventually selected. An empty existing list or no matches for a skill stays an empty SharePoint result, not a mock search.

The selection is made once per component instance, before the first React render. Filtering, theme changes and inline/fullscreen transitions reuse that selection. Reopening/reloading a new instance checks again. In Workbench the context web can be the site hosting Workbench, but `.origin` still selects the root; no host-type detection is needed. The LLM supplies only the skill and cannot choose the site or source.

## What is read

When SharePoint is selected, a second authenticated delegated GET retrieves matching items with `$filter=Skill eq 'SPFx'` (canonical value), a minimal `$select`, `$expand=Expert`, and `$top=100`. The service never reads the directory or the whole list as a search strategy. An unexpected continuation is reported instead of displaying a misleading truncated count; keep this conference list small.

The Person projection uses Id, Title, Name and EMail. Standard SharePoint Online membership claims (`i:0#.f|membership|UPN`) supply the UPN for photos/Teams. EMail separately supplies the SMTP address for calendar free/busy and is not substituted for the Teams UPN. Missing, empty or invalid email is mapped to undefined without rejecting the expert record. Unresolved/nonstandard claims render the profile with Teams contact unavailable and initials. No custom Email list column is needed.

## Profile photos

Live photos come from Graph v1.0 `GET /users/{encodedUPN}/photo/$value` through the authenticated SPFx MSGraphClientV3, rather than SharePoint's `userphoto.aspx`. This uses the Graph photo already verified in Graph Explorer for the unlicensed demo user; photo access from the component must also be checked in the actual host.

Cards render immediately after the list responds. Photos for the small skill result set load in parallel, including those beyond the first three inline rows. Each avatar reserves its existing size and shows a circular Fluent shimmer until its image loads. Missing identities, unavailable photos, denied requests and other photo failures show initials without failing the expert list or selecting mocks.

The loader shares one lazily acquired client and caches pending, successful and unavailable photos per normalized UPN for the component lifetime. Filters and mode/theme changes reuse the results. Graph loading has a five-second logical timeout, followed by a separate five-second image-load limit when necessary; late Graph responses are ignored rather than cancelling transport. Blob URLs are released at teardown. Reload the component to retry a cached unavailable photo after uploading a picture or fixing consent. Mock data retains its bundled portraits and never calls Graph.

Follow [Graph photo permissions with M365 CLI](graph-permissions.md) to reuse compatible delegated consent or grant `ProfilePhoto.Read.All` when missing. The conference tenant already has `User.ReadBasic.All`; its consent is reused. The package does not request additional permissions.

## Calendar availability

In fullscreen, Check availability selects one expert and opens their calendar panel. The component posts to Graph v1.0 `/me/calendar/getSchedule` with `schedules: [expert.email]` and `availabilityViewInterval: 30`. `/me` is the signed-in caller. The expert is identified by the Person field's SMTP email, never a display name or an assumed UPN/email match.

Today's 09:00–18:00 window is constructed in the viewing browser's local time zone and each boundary is converted to UTC. The 18 returned codes are displayed as local half-hour slots: Free (including Working Elsewhere), Tentative, Busy and Out of Office. Letter markers, labels/tooltips and a current-slot outline supplement the theme colors. Only availability codes enter UI state; returned event details are discarded. An invalid/empty response or per-schedule error never becomes a Free timeline.

There is no calendar prefetch, persistent cache or Graph polling. The current panel keeps its result until Refresh or unmount; its minute timer only updates the current-slot marker. Selecting another expert, filtering the selected expert out, changing skill or leaving fullscreen clears it. A ten-second logical request timeout and effect cleanup keep slow/stale requests from changing the active panel; the transport is not cancelled. Missing SMTP, mock mode, missing mailbox, permissions and sharing failures leave the rest of the application usable. Refresh/retry is available for request failures.

Follow the [CLI consent guide](graph-permissions.md) to check compatible calendar permissions. An accessible Exchange mailbox and appropriate calendar sharing are separate prerequisites: a working Entra profile photo alone does not establish calendar access. Choose a non-family demo expert with a working calendar for the success case; a user without a mailbox demonstrates the unavailable state. The component does not create mailboxes, change licenses or create calendar events.

## Check with the tenant

In your controlled tenant, check source selection with the root list absent (404), present, and present but empty. Verify that an explicit 403 or network error is shown rather than activating mocks. Then use an authorized account and, where available, another account with genuinely restricted list/item access. Verify filtered network requests, counts, Person names, photos, UPN chat navigation, permission failures, and the no-results case. Check the local name/department/role filters make no new GETs. If the selected list later disappears, or an item request is denied, slow or malformed, an understandable error is shown; there is no second fallback. Reload to perform source selection again. No automatic tenant writes, messages or meetings are performed.

The provider and automatic selection are implemented locally. Actual tenant availability responses, projection/photo behavior and permissions still need validation in your environment.

Calendar host checks are also pending: verify Person SMTP projection, a successful selected expert, unavailable mailbox/email, denied access, Refresh, rapid A→B selection, no calendar calls on inline/search, and the displayed range against Outlook. Use local browser time-zone inspection for winter/summer and DST boundaries. No tenant changes are performed by this implementation.

## Confirm a Teams meeting

A future Free slot can be booked with another expert after a confirmation dialog. Expert SMTP still comes from Person EMail. Organizer identity comes from the signed-in SPFx page context's email/login name; normalized email/UPN comparisons keep self slots informational. An unresolved organizer email also leaves booking unavailable. Mock profiles never expose real booking.

After confirmation, Graph checks both the organizer and expert for the selected half hour and verifies that the organizer's calendar supports Teams. Only then does it create `/me/events` with the expert as an attendee, `isOnlineMeeting: true` and `onlineMeetingProvider: teamsForBusiness`. Optional Notes are sent as text. The user reports `Calendars.ReadWrite` already granted; see the [permission guide](graph-permissions.md#teams-meeting-creation-permission). Cancel before confirmation makes no write. While processing, the dialog cannot be dismissed as though it had cancelled a sent request.

Use a non-family controlled account to validate the real invitation, Teams URL, self/past-slot restrictions, conflicts in either calendar and double-click handling. Both-calendar checks are a snapshot, not a reservation. Creation errors after sending the POST can have an uncertain result: check Outlook before trying again or reloading. The component retains a per-instance attempt guard and uses Graph transactionId; it does not persist meeting state across reloads. Actual end-to-end meeting validation is pending, and no invitations were sent by the implementation session.

References: [SharePoint REST query operations](https://learn.microsoft.com/en-us/sharepoint/dev/sp-add-ins/use-odata-query-operations-in-sharepoint-rest-requests), [Teams UPN chat links](https://learn.microsoft.com/en-us/microsoftteams/platform/concepts/build-and-test/deep-link-teams).

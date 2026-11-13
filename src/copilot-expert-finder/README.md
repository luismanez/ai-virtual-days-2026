# Expert Finder for Microsoft 365 Copilot

A SharePoint Framework 1.24.1 Copilot UX Component for the Microsoft 365 Live 2026 community demo. Copilot supplies a skill; React renders matching experts inline or fullscreen. The UI uses React 18, TypeScript, Fluent UI v9 and local fictional portraits.

## Run and build

Use Node.js 24.x (24.20.0 in this workspace). From this folder:

```bash
npm install
npm start -- --nobrowser
npm run build
```

The development server is HTTPS localhost:4321. Open your tenant's Copilot Workbench and fire `{"skill":"SPFx"}`. Set SPFX_SERVE_TENANT_DOMAIN if using automatic initial-page tenant substitution. The production package is `sharepoint/solution/copilot-expert-finder.sppkg`.

## Demo behavior

- Fixed invocation skills: SPFx, Copilot, DB, Spanish. There is no Skill selector.
- Inline: correct total and up to three experts, portraits, roles, contact and expansion.
- Inline content-size changes notify the host explicitly after asynchronous results/layout updates, so it can grow the frame to the natural content height. Fullscreen sizing remains host-owned.
- Fullscreen: responsive cards plus local name-prefix, department and role filters.
- Check availability opens one selected fullscreen card's calendar timeline for today, 09:00–18:00 in the viewer's local time. Availability loads on selection or Refresh; a future Free slot can open a separate confirmation for a real Teams meeting. Unavailable calendars remain an explicit unavailable state.
- Clear filters and host mode/theme changes reuse the fetched data.
- When the root Experts list returns HTTP 404, mock data provides SPFx 6, Copilot 8, DB 1, Spanish 0. Profiles are fictional and visibly labeled.
- Mock contact is a preview. Live Teams links never send a message.

## Data source

The component automatically checks `Experts` in the tenant root site once during initialization. HTTP 200 selects SharePoint; HTTP 404 selects the fictional mock collection. Other errors are shown without switching to mocks. There is no manual data-source or site-URL setting. The same package works across tenants using this convention.

The root URL comes from `new URL(this.context.pageContext.web.absoluteUrl).origin`, including when Workbench is opened in a subsite. The SharePoint provider filters Skill on the server using SPHttpClient with the current user's permissions. An existing list with zero matching items shows the empty state. Reload the component after creating the list to select the source again.

Live profile photos use delegated Microsoft Graph through SPFx's MSGraphClientV3: `GET /users/{UPN}/photo/$value`. Cards appear before the photos, with a circular shimmer until each image loads and initials when unavailable. Requests run in parallel and are cached per UPN for the component lifetime; filters and inline/fullscreen reuse them. Mock profiles still use bundled portraits and make no Graph requests. See the [CLI consent guide](docs/graph-permissions.md); the conference tenant reuses its existing `User.ReadBasic.All` grant. No package permission requests or new dependencies were added. Actual photo access and latency in Workbench/Copilot still need a live check.

Calendar availability uses `POST /me/calendar/getSchedule` with only the selected expert's SMTP email from the SharePoint Person field. Its 18 half-hour slots show Free, Tentative, Busy or Out of Office with text markers and tooltips. Times are normalized to UTC for Graph and displayed locally. The panel keeps only its active result, ignores stale responses, and clears when another expert is selected, filters hide it, or fullscreen closes. Event details are discarded. Mock mode makes no calendar calls. Recheck the previously observed `Calendars.Read` consent, or follow the guide for `Calendars.ReadBasic` when no compatible permission exists; actual mailbox/policy access still requires a tenant check.

The timeline displays Morning (09:00–14:00, ten slots) or Afternoon (14:00–18:00, eight slots), with arrows disabled at the ends. Blocks are 56 px high and show their start time plus a larger status marker. Each period uses two rows in narrow cards and one row from 600 px of panel width. Initial selection follows the local hour (afternoon from 14:00); Refresh preserves it. Period arrows only slice the already loaded day and make no Graph requests.

## Confirmed Teams meetings

Future Free slots for another expert are buttons. The signed-in user's own slots, past/current slots and previously requested slots remain informational. A Fluent confirmation shows the expert, organizer, date, local time, duration and optional plain-text Notes (up to 1,000 characters). Cancel closes it without a write. Create Teams meeting checks Teams support and rechecks both calendars, then posts one calendar-backed meeting to `/me/events`. The caller organizes it and the selected expert receives a real invitation. Booking is reached only from Check availability and explicit confirmation.

Meeting creation requires delegated `Calendars.ReadWrite`, which the user reports already granted. No package permission request, dependency or new authentication flow was added. The service uses the same SPFx Graph factory and identifies the organizer from the current SPFx user's email/login name. It blocks self invitations in both UI and service.

Double clicks/in-flight repeats share one per-instance request, and `transactionId` protects SDK retries. If a POST response is lost or uncertain, the slot stays blocked in that instance and the user is told to check Outlook. Reloading is not a safe substitute for checking whether an invitation was already sent. A missing Teams link is read from the existing event once; no second meeting is created to recover it. Success provides Open in Teams, or an Outlook link when the event exists but its Teams link could not be retrieved.

After confirmed creation, the slot immediately shows Busy using a small in-memory booking record keyed by expert email and UTC interval. Refresh still reads Graph, but local bookings take precedence even if the response still says Free. Switching experts or closing/reopening fullscreen retains these records for the component instance; a full reload clears them. Tooltips and a panel note identify meetings created in this session. No polling, localStorage or extra post-success availability request is needed. Failed or uncertain creation does not add a confirmed booking. This local Busy mark represents an invitation we created, not an atomic reservation or proof that the expert accepted; it does not track later declines, cancellations or changes made in Outlook.

Build/package checks are local only. Real invitation delivery, self detection, Teams link opening and calendar conflicts still require a controlled Workbench/Copilot rehearsal. No real meeting was created during implementation.

## Meeting agenda with Copilot

After a confirmed booking, **Prepare agenda with Copilot** shares the selected professional profile, event identity/time and original **Session objective / notes** with the existing conversation. Copilot generates an agenda and calls `PrepareMeetingAgendaTool`, which loads that event in an independent editable card. **Save agenda to meeting** explicitly updates the same invitation and may notify attendees. Generation/rendering alone never saves.

New bookings carry an event-local marker and booking correlation; older unmarked events are ineligible. Saving reads the current body and preserves Notes/Teams information while adding or replacing one escaped agenda section. Unknown writes require a status check and Outlook inspection before another deliberate save. No additional permission, dependency or testing infrastructure was added. Runtime checks are left to the user after compilation; the known Introduce me host issue remains unverified.

## Session resources

- [Code walkthrough and slide snippets](docs/code-walkthrough.md)
- [SharePoint list setup and sample data](docs/sharepoint-setup.md)
- [Graph photo and calendar permissions with M365 CLI](docs/graph-permissions.md)
- [Build, Workbench and installation](docs/deployment.md)
- [Natural invocation checklist](docs/copilot-invocation.md)
- [Conference script and discovery fallback](docs/demo-runbook.md)
- [Portrait provenance](src/copilotComponents/expertFinder/assets/README.md)
- [Repository implementation specs](../../specs/README.md)

The scope is a community demo: no automated test suites or extra infrastructure were added. Use the existing build and practical host checks. Actual Workbench, tenant permission/photo, natural discovery and live Teams validation must be recorded separately from compilation.

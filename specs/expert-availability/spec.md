# Feature: Expert Availability Panel

Implemented locally. Build/packaging passed; actual Workbench/Copilot and tenant validation remain pending. This extends the existing [global spec](../GLOBAL.md) without reopening unrelated tasks.

## Scope and existing seams

In fullscreen, add `Check availability` to each existing `ExpertCard`. These cards already contain the detailed profile; no new details page or drawer is needed. One selected card expands a compact Availability section. Inline stays compact; Teams contact, Schedule preview, Graph photos, search and source selection retain their behavior.

Use one small `GraphAvailabilityService`, one `ExpertAvailabilityPanel` and local fullscreen selection state. Reuse SPFx's `msGraphClientFactory.getClient('3')` infrastructure already used by GraphPhotoService. Do not initialize another SDK client or authentication flow, add dependencies, or change photo caching. Add optional `Expert.email` from the existing SharePoint Person field's `EMail`; retain the UPN separately for photos/Teams. Missing or invalid email must not reject an otherwise valid expert record.

This feature follows the session's explicit demo choices: existing build plus manual checks, no unit tests or test infrastructure, and delegated consent through M365 CLI rather than `webApiPermissionRequests`. Application code is implemented; tenant settings have not been changed.

## Calendar read contract

Use delegated Graph v1.0 `POST /me/calendar/getSchedule`. `/me` is the signed-in caller; `schedules` contains exactly the selected expert's SMTP email, never their display name or an assumed UPN/email substitution. Request `availabilityViewInterval: 30`, with `startTime`/`endTime` expressed as UTC `dateTimeTimeZone` values.

Build today's 09:00 and 18:00 boundaries in the viewing browser's local time zone, then convert each boundary to UTC for the request. Use the local date, not the UTC date, to decide today. Native Date/Intl are sufficient; do not hardcode a UTC offset, use the expert's workingHours as the viewing zone, or introduce a date library. Recompute the window on Check/Refresh. The initial window contains 18 half-hour slots; align each code with the requested UTC start plus its slot offset, then format labels locally.

Validate the single returned schedule before using `availabilityView`. A valid 18-character view maps `0` to Free (including Working Elsewhere), `1` to Tentative, `2` to Busy and `3` to Out of Office. An empty/missing view, unexpected length/code or `scheduleInformation.error` yields unavailable/error, never fabricated Free slots. Keep only normalized slots/date/zone needed by the UI. Discard scheduleItems and workingHours; never render, retain in state or log event subjects, locations, attendees, raw payloads or tokens.

A simple bounded request timeout and friendly errors are sufficient. Handle HTTP 401/403/404/429, missing mailbox, Exchange sharing restrictions and network/service failures without interrupting expert results. Permission errors and mailbox/policy errors must not be presented as definitive proof of one specific cause. Retry is explicit; no retry framework or Graph polling.

## Panel behavior

- Show `Availability`, `Today · 09:00–18:00`, and the viewing time-zone label. Show the loaded day in two locally navigable periods: Morning 09:00–14:00 (10 slots) and Afternoon 14:00–18:00 (8 slots). Use 56 px high blocks with a visible start time and larger status marker, plus the legend, full-range tooltips and accessible labels. Show one row at panel widths of at least 600 px and two rows below that, using CSS container queries. Previous/next arrows are disabled at their respective ends and never call Graph. Initialize to Afternoon from local 14:00 onward, otherwise Morning; Refresh and clock ticks preserve the chosen period. Distinguish states beyond color with a short marker or pattern; use Fluent semantic tokens for both themes. Preserve readability at narrow card widths without horizontal page overflow.
- Highlight the current segment when `start <= now < end`; no segment is current outside the window. A local minute timer may update this marker while open, without another Graph call. Refresh recalculates today's range. Keep the queried date associated with the result so an older date is not mislabeled Today.
- Show loading only inside the panel, plus understandable unavailable/error states and `Refresh` or `Try again` when useful. Prevent duplicate clicks while loading. An unresolved SMTP address shows `Availability unavailable: no email address` with no Graph call.
- Mount a panel only after Check availability selects that expert. Repeating Check for the already selected expert does not refetch; Refresh does. Switching experts immediately removes the previous result and ignores late completions using an ordinary effect cleanup/request guard. Clear selection when filters hide the expert, a new skill is loaded, or fullscreen closes; it must not silently fetch again when a card returns.
- Keep only the active panel's result in memory until selection changes/unmounts. No cross-expert or persistent calendar cache. Mock mode shows an honest unavailable explanation and performs no calendar calls or simulated free/busy reads. Calendar state is separate from Teams presence; Schedule remains a preview and creates no meeting.

## Confirmed Teams meetings

User-approved extension, implemented locally on 2026-11-04. This replaces the initial calendar-write exclusion only for explicitly confirmed meetings; the original Schedule action remains a preview.

- Only future Free slots for another expert are actionable. Resolve the current organizer from SPFx pageContext.user email/loginName and compare normalized SMTP/UPN, never display names. Mock mode, self, unresolved organizer/expert email, past/current slots and an already requested interval make no booking action available. Validate eligibility and 30-minute duration again at the service boundary.
- A Fluent dialog shows subject, expert, organizer, local date/time/zone and duration, with optional plain-text Notes limited to 1,000 characters. Cancel before confirmation makes no write. Create Teams meeting is explicit; actions, dismissal and notes are locked while processing. Dialog focus returns to the slot, Refresh or the panel after closing.
- GraphMeetingService reuses the installed SPFx Graph factory. On confirmation, it checks the caller's allowedOnlineMeetingProviders for teamsForBusiness and requests getSchedule for exactly the organizer and expert in the selected half hour. Only two valid Free views allow creation; preflight failures, conflicts or a slot that has started stop before writing. These checks are a snapshot, not an atomic reservation.
- Create one `/me/events` with the signed-in user as organizer, one required expert attendee, UTC start/end, Text notes, isOnlineMeeting true, onlineMeetingProvider teamsForBusiness and a generated transactionId. This sends a real invitation. Calendars.ReadWrite is required; the user reports already granting it. No OnlineMeetings.ReadWrite, package permission request, new auth flow or tenant consent operation was added.
- A per-instance promise guard deduplicates the expert/interval while pending, after success or after an uncertain POST outcome. The same payload transactionId protects SDK retries. Preflight failures can be retried; once POST begins, errors/timeouts retain the guard and instruct the user to check Outlook. No automatic second create is used to recover a lost response. Guards are not persisted across component reloads/instances; check the calendar before trying elsewhere after uncertainty.
- Read-only preflight has a 10-second limit; event creation has a 20-second logical limit. If the event exists but its join link is missing, read that existing event once (up to five seconds). Return only ID and HTTPS Teams/Outlook links to UI. If the Teams link remains unavailable, say so and offer Outlook rather than creating another invitation. After confirmed creation, immediately render that expert/UTC interval as Busy using a small in-memory record in GraphMeetingService. Merge this local mark when rendering any later Graph result, including stale Free responses after Refresh or panel remounts. Success updates the timeline locally without an extra getSchedule request; manual Refresh still reads Graph. Keep records for the component instance, then clear them at disposal. Do not persist them or mark failed/uncertain attempts as confirmed bookings. Label session-created meetings in the tooltip and panel note. This mark represents the invitation created here, not attendee acceptance; it does not track subsequent declines, cancellations or rescheduling in Outlook.
- Existing availability navigation, theme tokens, photos, filters and contact actions are preserved. No raw Graph payloads, tokens or private calendar event details are logged. Notes live only in the confirmation/request and resulting event; no localStorage or persistent calendar cache is introduced.

Manual acceptance remains pending: actual SPFx calendar write permission, Teams provider/link, invitation delivery to a non-family controlled account, self/past guards, Cancel/no-write, Notes, conflicts in either calendar, duplicate clicks, uncertain outcomes and modal focus/host link navigation. No real calendar request, consent operation or invitation was performed as an implementation/build check.

## Permissions and conference prerequisites

The current getSchedule method reference lists delegated `Calendars.ReadBasic` as least privilege and accepts existing `Calendars.Read`/`Calendars.ReadWrite`. The previously supplied SPFx grant already included `Calendars.Read`; recheck it through the documented CLI workflow before proposing another consent. If a compatible scope is absent, document adding `Calendars.ReadBasic` while preserving every existing scope. Do not execute grants or add manifest permission requests. CLI installation, login and application setup remain outside the demo docs.

The general free/busy overview still says Calendars.Read is the minimum; follow the current endpoint permission table for Calendars.ReadBasic. Both the caller's calendar access and the expert's accessible Exchange mailbox/sharing matter. An Entra photo or account alone does not establish mailbox availability. Use a controlled expert with a working Exchange calendar for the successful demonstration; a user without an accessible mailbox demonstrates the unavailable state. Do not use family accounts for the demo, provision mailboxes, change licenses or seed calendar events automatically.

## Acceptance and execution

Follow [AV-01 through AV-04](tasks.md); progress lives only in [tasks/todo.md](../tasks/todo.md). Build/packaging can be completed locally. Actual SMTP projection, consent, mailbox access, timeline alignment, theme/layout and stale-response behavior need manual Workbench/Copilot verification. Record unavailable checks as pending; Graph Explorer alone is not component verification.

## References

- [Create calendar event / delegated Calendars.ReadWrite](https://learn.microsoft.com/en-us/graph/api/user-post-events?view=graph-rest-1.0)
- [Calendar-backed Teams meetings and joinUrl](https://learn.microsoft.com/en-us/graph/outlook-calendar-online-meetings)
- [Event transactionId](https://learn.microsoft.com/en-us/graph/api/resources/event?view=graph-rest-1.0#properties)

- [getSchedule endpoint, request and current permissions](https://learn.microsoft.com/en-us/graph/api/calendar-getschedule?view=graph-rest-1.0)
- [scheduleInformation and availability codes](https://learn.microsoft.com/en-us/graph/api/resources/scheduleinformation?view=graph-rest-1.0)
- [Free/busy overview, sharing and returned event data](https://learn.microsoft.com/en-us/graph/outlook-get-free-busy-schedule)
- [Date: local time, UTC and daylight saving offsets](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date#date_components_and_time_zones)
- [Existing CLI permission workflow](../../src/copilot-expert-finder/docs/graph-permissions.md)

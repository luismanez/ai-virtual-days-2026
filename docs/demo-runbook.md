# Expert Finder: conference opening demo

Allow about 3-4 minutes. Open the deployed agent in advance, check the selected data source and local portrait loading, and keep a fresh conversation ready. Source selection is automatic: Experts in the tenant root selects SharePoint; HTTP 404 selects fictional Demo data. The counts, names and filter examples below assume the mock collection. For that rehearsal, use a controlled tenant without the root Experts list; if it exists, adapt the script to its real rows and match the narration to the source label.

1. Ask: **Who can help me with SPFx?** Copilot selects the tool and supplies the context. Mock mode returns six records; three appear inline.
2. Point out the photo, name, job title and Teams action. Open a mock contact preview and explain that live mode opens a Teams draft chat for the selected tenant person, without sending anything.
3. Choose **Open in fullscreen**. Highlight the fixed SPFx badge and detailed cards.
4. Enter **Al** in Search by name. Select **Digital Workplace** and **Senior M365 Developer**. Alex remains. Clear filters to restore the original skill results. Explain that these interactions are local React state, not another LLM call or data retrieval.
5. Open **Schedule**. The dialog explicitly states that no meeting has been created; it demonstrates where a richer interaction could go.
6. Use the host's light/dark control and, if useful, collapse/reopen to show retained local state.
7. Ask a new question for **databases**: one mock expert. Ask about **Spanish**: no matches. This demonstrates a new skill invocation, not an in-app skill selector.

## Discovery fallback

If propagation delays tool discovery, start a new conversation and use `-developer on` diagnostics where available. If it still does not select the tool, switch openly to Copilot Workbench and fire `{"skill":"SPFx"}` directly. Say: “I am supplying the tool argument manually to show the visual experience while discovery propagates.” This verifies the component, not natural tool discovery. Keep the local HTTPS server running only when using the Workbench fallback.

## Live calendar availability segment

This segment needs SharePoint mode and a non-family demo expert with a valid Person email and an accessible Exchange calendar. The earlier mock script does not provide live calendar data. Recheck the [delegated calendar permission](graph-permissions.md#calendar-availability-permission) and compare the prepared expert's calendar in Outlook before the session. Prepare any known Free/Tentative/Busy/Out of Office entries manually; the app creates none.

1. Open fullscreen and select **Check availability** on that expert. Point out that expert cards and photos already worked before any calendar request was made.
2. Explain **Today · 09:00–18:00**, the viewing time zone and the legend. Use the arrows between Morning (09:00–14:00) and Afternoon (14:00–18:00). Larger blocks show their start time and status; narrow cards use two rows. These arrows reuse the loaded day without calling Graph. The initial period follows the local hour, and Refresh preserves the selection. This is calendar free/busy; it does not indicate Teams presence.
3. Choose **Refresh** for a new read. Switching to another expert replaces the panel and ignores the previous response if it arrives late. Filters and ordinary host re-renders do not poll calendars.
4. Optionally select a controlled mailbox-free user to show the honest unavailable state. In mock mode the panel explains that fictional profiles have no calendar; it does not invent a timeline.
5. Keep **Schedule** as the existing preview. To demonstrate actual booking, use the separate flow below; merely opening availability or the Schedule preview sends no invitation.

Manual rehearsal: check light/dark and narrow cards, keyboard slot tooltips, current-slot boundaries, duplicate clicks and A→B while pending. Compare local/UTC payload alignment for winter, summer and the Europe/Madrid transition dates 2026-03-29 / 2026-10-25 using browser time-zone inspection rather than adding date navigation. Record actual results and leave unavailable permission/mailbox/error scenarios pending. If live availability is unavailable on stage, retain the rest of Expert Finder and explain the calendar state instead of showing simulated free time.

## Confirmed Teams meeting segment

Use a non-family demo expert and a future Free slot today. Past/current slots, the signed-in user's own calendar and mock profiles cannot create meetings. After 18:00 there are no future slots in the current day's 09:00–18:00 window. The user reports Calendars.ReadWrite already granted; the implementation session did not reconfigure consent or send test invitations.

1. Select a future Free slot. Show the confirmation's expert, organizer, date, local time, 30-minute duration and optional Notes. Choose **Cancel** once to demonstrate that this makes no calendar write.
2. Reopen a future slot, optionally enter a short note, then explicitly choose **Create Teams meeting**. This sends a real invitation after verifying Teams support and rechecking both calendars. The dialog remains locked while processing.
3. Show the confirmation and use **Open in Teams**. Close the dialog: the booked slot immediately shows **Busy** and cannot be selected again. Choose **Refresh**, then switch to another expert and back: the local booking stays Busy even if Graph still reports Free. The panel note and tooltip identify this as a meeting created in this session. These records live only for the component instance, with no polling or persistent storage. The expert can still accept or decline; this local mark does not track later changes in Outlook or prove attendee acceptance.
4. If the event exists but its Teams link could not be read, open it in Outlook; the app does not create another meeting to recover the link. If creation is uncertain, check Outlook before doing anything else. The per-instance guard blocks a repeat for that expert/interval; reloading is not a safe substitute for checking the calendar.

Manual acceptance still required: self detection, future-slot boundaries, Cancel/no-write behavior, plain-text notes, actual invitation/link, immediate Busy after success, Busy retained after Refresh/expert/mode changes with a stale Free response, no confirmed local booking after failure/uncertainty, either participant becoming Busy before confirmation, no Teams provider, denied access, double clicks and uncertain responses. Use only controlled demo accounts and cancel the rehearsal event manually in Outlook afterward when appropriate. No real meeting was created as an automated build or implementation check.

## Introduce me segment

Use the agent release with the updated drafting instructions synced to Copilot. Either a real expert or a visibly fictional mock profile can demonstrate this flow; it requires no new Graph permission or mailbox.

1. Ask for experts, expand and point to **Introduce me** beside Chat in Teams and Schedule.
2. Choose one expert. Show the brief **Requesting draft…** state. The component shares a bounded snapshot of that profile through `updateModelContextAsync`, then asks for a draft through `sendFollowUpMessageAsync`.
3. Read the draft in the Copilot conversation. Check the selected name, first-person introduction and exactly three practical questions about the skill, grounded in the visible profile. Explain that the user reviews/copies the text; this action sends nothing to Teams.
4. Optionally choose a different expert after the handoff finishes: the next request replaces the profile context. Demonstrate that filters and fullscreen changes remain local interactions rather than automatic follow-ups.

Rehearse Spanish and English human requests, rapid same/different-card clicks, both themes, narrow cards and keyboard activation. Check missing profile text, mock labeling and instruction-like CV content using controlled demo data. Verify no repeat ExpertFinderTool call or external action in developer diagnostics. The component handles Bridge rejection locally; if Workbench cannot continue the conversation, show the error honestly and keep the full drafting demonstration in deployed Copilot. No Bridge call or draft was generated as a local build check.

## Before going on stage

Check the actual tenant/agent, whether the root Experts list is present and accessible, the resulting data-source label, network/profile images, allowed list access if live, Teams navigation for a tenant user if live, host modes/themes, and the fallback. For calendar reads, verify an accessible mailbox, consent and the returned free/busy snapshot; it is not a guarantee that an expert can accept a meeting. The separate confirmed booking flow sends a real meeting invitation; use only the prepared non-family demo accounts. Keep Hello Copilot separate; this workspace is only the prepared Expert Finder demo.

A real Workbench/Copilot rehearsal is still required in your environment. No browser/tenant rehearsal was claimed by the implementation session.

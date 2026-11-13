# Feature: Inline experience and host shell

The first complete flow: a Workbench skill input retrieves experts from the selected service and renders a compact Copilot component, with supported host theming and expansion.

In inline mode, observe the natural content section with the owning window's ResizeObserver and notify the host through the entry's public `requestSizeChangeAsync(width, height)` callback when its dimensions change. This includes asynchronous loading/results/errors, wrapped text/photos and returning from fullscreen. Report only positive changed sizes, disconnect at unmount/fullscreen, and ignore entry callbacks after teardown. Do not size against the iframe viewport, impose a fixed height, change the three-profile layout or resize the host-owned fullscreen view. The host remains authoritative and may constrain the requested dimensions.

The 2026-11-13 screenshot shows an inline frame clipping the results; source comparison found no recent inline row/header layout change. Explicit content-size notification follows [Microsoft's documented guidance](https://learn.microsoft.com/en-us/sharepoint/dev/spfx/copilot/access-data#resize-the-frame-when-content-changes); actual improvement in the deployed host remains for user verification after compilation.

Follow [GLOBAL.md](../GLOBAL.md) for the shared data, visual, safety and demo-scope contract. Do not add supporting infrastructure beyond what the session needs.

See [tasks.md](tasks.md) for the small implementation steps and [the checklist](../tasks/todo.md) for progress.

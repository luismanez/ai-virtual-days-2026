# Feature: SharePoint expert data

A simple delegated SPHttpClient provider using server-side Skill filtering, the final root-site list schema, automatic selection from its availability, and supported Person/photo information. HTTP 200 selects SharePoint; HTTP 404 selects mocks; other failures show errors. Empty results do not change the source.

Follow [GLOBAL.md](../GLOBAL.md) for the shared data, visual, safety and demo-scope contract. Do not add supporting infrastructure beyond what the session needs.

See [tasks.md](tasks.md) for the small implementation steps and [the checklist](../tasks/todo.md) for progress.

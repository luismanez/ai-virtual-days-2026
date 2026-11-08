# Copilot invocation checklist

Workbench injects arguments directly. It verifies the UI/argument path, not natural tool selection. Actual Copilot selection requires the installed/synced agent and a new conversation. Counts below apply when the root Experts list returns HTTP 404 and mock data is selected. If the list exists, use its actual rows/counts instead.

| Prompt | Expected behavior |
|---|---|
| Who can help me with SPFx? | ExpertFinderTool, skill SPFx; 6 mock results, first 3 inline |
| Find a SharePoint Framework developer. | skill SPFx |
| Who can help me build a Copilot agent? | skill Copilot; 8 mock results |
| Find a database expert. | skill DB; 1 mock result |
| Find someone who knows Spanish. | skill Spanish; honest empty state |
| Find an expert. | Ask which skill |
| Who can help with Azure Functions? | Explain the supported skills; do not invent a match |

In Workbench, fire {"skill":"SPFx"}, then try lowercase/whitespace variants, an empty string, an unknown skill and a non-string value. Invalid input must show a correction without querying expert items. The independent list-availability check still runs once when the component initializes. The app never changes the skill inside an invocation.

Enable Copilot Developer Mode with `-developer on` if available in your tenant to inspect actual tool choice and arguments. Record what you observe; do not describe Workbench injection as natural discovery. On agent or discovery updates, start a new conversation. Runtime tool discovery and baseline-agent updates are different: rebuild/redeploy component changes, and re-sync when changing baseline instructions, descriptions or starters as appropriate to the current platform guidance.

## Introduce me: contextual drafting

Deploy the updated sppkg and re-sync the baseline agent: this feature changes `copilot/instruction.txt`. Start a new Copilot conversation, search for a supported skill, expand and choose **Introduce me** on one expert. The component updates model context with only that expert's name, title, department, skill and summary, plus purpose/demo metadata, then sends a fixed user-triggered follow-up. It does not call another model or retrieve more profile data.

In Copilot and its developer diagnostics, check:

- The draft appears in the conversation, with the selected expert, supported professional facts and exactly three numbered questions. React shows no draft modal/editor. Check an English request and a Spanish request; the English technical follow-up must not override the preceding human request's language.
- Drafting does not unnecessarily execute ExpertFinderTool again, call another tool, open Teams or send a real message. A normal subsequent request to find people still invokes the search tool. A drafting request without selected context asks the user to use the button.
- After requesting A, request B once the acknowledgement completes: B must use the replacement profile. Rapid A/B clicks while pending must create only one handoff. Filters and mode/theme changes alone send nothing.
- Missing optional text does not invent experience. Mock profiles produce an illustrative draft and require no email/UPN. Profile text resembling instructions remains data and must not cause external actions or change the request's constraints.
- Context over the 9,000 UTF-8 byte budget is rejected locally without a follow-up or silent truncation. Transport/context errors and a follow-up acknowledgement with `isError` produce an inline error and allow a deliberate retry. Disposal stops pending work before an unsent follow-up.

Workbench can check the action and local error/layout behavior, but may not support the complete conversation flow. Do not add a Bridge simulator. The host acknowledgement does not contain the generated draft or prove generation completed. The model's wording, question count, language and tool choice require a real deployed Copilot check; those outcomes were not verified during local implementation.

### Investigating the rejected follow-up

The user observed the component's follow-up appearing in chat, followed by a generic refusal and a conversation that no longer accepted messages, even after closing the component. Pasting the exact same follow-up manually in a new conversation without opening the component allowed a response and further messages. This makes the follow-up text alone a less likely cause; it does not establish whether the published context, Bridge integration or host behavior caused the failure.

Solution 1.0.0.4 removes behavioral directives from the model-context prefix, leaving a descriptive data label and the same five-field JSON/purpose/demo snapshot. The follow-up text, agent instructions, Bridge call order, guards, timeout and teardown remain unchanged so this retry isolates one variable. Trust handling remains in the existing agent instructions and follow-up. No extra close/teardown call, delay or automatic retry is added. This is a candidate correction, not a verified host fix.

With agent 1.0.3 already synchronized, replace/enable only the updated sppkg and start a fresh conversation; the agent package has not changed. Check whether Introduce me drafts for the selected expert and the chat still accepts another ordinary message. If it still fails, retain the developer diagnostics and relevant failed response/error code (excluding credentials/tokens) before further changes. Do not retry in the already blocked conversation or assume that a successful build proves recovery.

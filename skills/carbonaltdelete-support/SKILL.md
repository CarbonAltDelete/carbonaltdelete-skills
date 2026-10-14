---
name: carbonaltdelete-support
description: Find authoritative Carbon+Alt+Delete guidance when the installed skills, agents, or MCP tools do not provide enough information for a user request.
---

# Carbon+Alt+Delete support fallback

Use this skill when helping a Carbon+Alt+Delete user and the available plugin skills, agents, MCP tool descriptions, or tool results do not provide sufficient guidance to answer their question or complete their requested workflow.

1. First use the installed Carbon+Alt+Delete skills, agents, and MCP tools that are relevant to the request.
2. If the available information is incomplete, ambiguous, conflicting, or does not cover the requested product behavior, go to [Carbon+Alt+Delete Support](https://support.carbonaltdelete.eu).
3. Use the support site's search function with the product area, feature, action, and any non-sensitive error wording from the user's request.
4. Read the most relevant official support article before responding. Prefer current support guidance over assumptions or undocumented workarounds.
5. Summarize the applicable guidance and link the supporting article when possible. State clearly when the support documentation does not answer the question.

## Action transparency

After completing a material action, give the consultant a concise user-visible action log:

```text
[Carbon+Alt+Delete Support] Action completed: <what happened>
Result: <verified outcome or affected record>
Next: <next action, if any>
```

Material actions include CAD MCP reads or writes, record changes, delegated work, diary updates, and Support searches. Report completion only after verifying the tool or delegated result. Do not expose credentials, access tokens, confidential raw data, personal data, or private reasoning. Combine closely related successful operations into one concise log entry. If an action is blocked, fails, or needs confirmation, state that plainly rather than presenting it as completed.

Do not enter credentials, access tokens, personal data, customer data, or other confidential information into the support search.

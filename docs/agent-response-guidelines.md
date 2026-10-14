# Agent response guidelines

These guidelines define the user-visible action reporting expected from all Carbon+Alt+Delete plugin skills and Claude Code subagents.

## Action transparency

After completing a material action, give the consultant a concise user-visible action log using this format:

```text
[<Agent name>] Action completed: <what happened>
Result: <verified outcome or affected record>
Next: <next action, if any>
```

Material actions include Carbon+Alt+Delete MCP reads or writes, creating or updating records, delegating or completing subagent work, updating a project diary, and searching official support guidance.

Only report an action as completed after its tool result or delegated outcome has been verified. Do not expose credentials, access tokens, confidential raw data, personal data, or private reasoning. Combine closely related successful operations into one concise log entry rather than producing noisy per-call updates.

When an action is blocked, fails, or needs user confirmation, state that status plainly instead of presenting it as completed.

## Cross-client maintenance

The policy is embedded in every portable `skills/*/SKILL.md` and in every matching Claude Code `agents/*.md` so it works in all supported clients. Keep the embedded policy behavior synchronized with this document in the same changeset.

---
name: project-diarist
description: Keep project-diary.md, the audit trail of a corporate carbon-footprint project, with verified actions, key decisions, and assumptions. Use after a material Carbon+Alt+Delete change, consultant decision, assumption, milestone, or blocker, or when the consultant asks to update or review the project diary.
---

# Project diarist

Maintain one accurate, up-to-date Markdown project diary that lets the consultant understand what happened, why it happened, and what still needs attention.

Your deliverable is `project-diary.md` in the active project workspace unless the consultant specifies another path. Create it if it does not exist; otherwise update it in place.

## Source and accuracy rules

- Record only actions, outcomes, decisions, assumptions, and evidence observable in the current task context, project files, tool results, or a delegated agent's verified report.
- Never invent events, timestamps, actors, approvals, company IDs, data values, or completion states.
- If a detail is unknown, write `Not recorded` or add it to the follow-up rather than guessing.
- Use a supplied timestamp when available. Otherwise, use the current session date and label it as `Recorded` rather than implying an exact execution time.
- Do not include credentials, access tokens, personal data, customer data, or confidential raw source data. Refer to protected records descriptively instead.
- Treat the diary as an audit trail: correct errors by adding a dated amendment or clarifying note instead of silently rewriting historical facts.

## Update procedure

1. Read the existing `project-diary.md` before editing it.
2. Identify new, materially relevant events since the last entry: consultant instructions or approvals, agent work, CAD MCP reads or writes, imported data, methodology choices, completed milestones, blockers, decisions, and assumptions.
3. Upsert the diary entries: update an existing row when it describes the same ongoing action; append a new row for a distinct action. Do not duplicate previously recorded events.
4. Update the action summary, decisions register, and assumptions register together when the new information affects them.
5. Preserve chronological history and keep the current status and next action visible.
6. Report the diary path and a short summary of rows added or updated.

## Required diary format

Use this structure unless the consultant explicitly requests another format:

```markdown
# Corporate carbon footprint project diary

## Project context

| Field            | Value                                         |
| ---------------- | --------------------------------------------- |
| Company          | <user-confirmed company name or Not recorded> |
| Company ID       | <user-confirmed ID or Not recorded>           |
| Reporting period | <period or Not recorded>                      |
| Last updated     | <date/time or Recorded on YYYY-MM-DD>         |

## Action summary

| Date / time | Actor | Action | Outcome / status | Evidence or reference | Follow-up |
| ----------- | ----- | ------ | ---------------- | --------------------- | --------- |

## Key decisions

| Date | Decision | Owner / source | Rationale and evidence | Impact |
| ---- | -------- | -------------- | ---------------------- | ------ |

## Assumptions

| Date | Assumption | Owner / source | Rationale | Validation or expiry condition |
| ---- | ---------- | -------------- | --------- | ------------------------------ |

## Current status

- **Done:**
- **In progress:**
- **Blocked:**
- **Next:**

## Amendments

| Date | Amendment | Reason |
| ---- | --------- | ------ |
```

## Action transparency

After completing a material action, give the consultant a concise user-visible action log:

```text
[Project Diarist] Action completed: <what happened>
Result: <verified outcome or affected record>
Next: <next action, if any>
```

Material actions include CAD MCP reads or writes, record changes, delegated work, diary updates, and Support searches. Report completion only after verifying the tool or delegated result. Do not expose credentials, access tokens, confidential raw data, personal data, or private reasoning. Combine closely related successful operations into one concise log entry. If an action is blocked, fails, or needs confirmation, state that plainly rather than presenting it as completed.

## Carbon-footprint context

Classify diary entries precisely. Keep boundaries, activity taxonomy, organizational units, reporting periods, inventory data, emission factors, reporting, and simulations distinct. Clearly mark estimates, data-quality limitations, and methodology assumptions so they can be reviewed later.

Do not decide carbon-accounting methodology or perform CAD writes merely to fill the diary. Record those actions only after the consultant or an authorized agent has made and verified them.

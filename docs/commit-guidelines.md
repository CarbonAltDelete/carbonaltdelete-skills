# Commit guidelines

Use these conventions when preparing commits for the `carbonaltdelete-skills` plugin repository. They apply to repository maintenance work, not to agents using the installed plugin.

## Structure

A commit message has a tag, an affected plugin area, and a short description. Add a body when it explains intent, compatibility, or user impact. Add an issue reference only when one is known and relevant.

```text
[TAG] Area: short description

Why the change is needed, its compatibility impact, and any relevant context.

Refs: #123
```

## Tags

Choose one tag that best describes the change.

| Tag        | Use for                                                                |
| ---------- | ---------------------------------------------------------------------- |
| `[SKILL]`  | Adding or materially changing a reusable skill                         |
| `[PLUGIN]` | Plugin manifests, packaging, marketplace, or Claude Code compatibility |
| `[FIX]`    | Correcting broken plugin, skill, or repository behavior                |
| `[DOCS]`   | Documentation and contributor or agent guidance                        |
| `[TOOL]`   | Development tooling, linting, hooks, or repository automation          |
| `[REF]`    | Internal refactoring with no intended behavior change                  |
| `[REL]`    | Version, release, or distribution changes                              |
| `[SEC]`    | Security or credential-handling improvements                           |

## Areas

Use the primary affected repository area. Typical areas are `Skills`, `Plugin`, `Claude`, `Marketplace`, `Tooling`, `Docs`, or the name of a specific skill. Split commits that affect unrelated areas.

Examples:

```text
[SKILL] Manage Projects: add project archival workflow
[PLUGIN] Claude: add compatible plugin manifest
[TOOL] Tooling: validate JSON with Biome
[DOCS] Docs: document local plugin installation
```

## Header and body

Use this header format:

```text
[TAG] Area: short description
```

Keep the header imperative, concise, and descriptive. It should read naturally after “This commit will”.

When a body is useful, describe why the change is needed, what user or maintainer value it provides, and any compatibility implications. Do not merely restate the diff. Wrap lines at about 120 characters.

## Procedure

1. Run `git status` and inspect staged and unstaged changes with `git diff --staged` and `git diff`.
2. Run `git log --oneline -5` to check the repository’s recent style.
3. Run `mise run lint` and resolve all failures before committing.
4. Select the tag and area, then prepare the plain-text commit message.
5. Stage only the relevant files. Never stage credentials, tokens, `.env` files, or other secrets.
6. Create a signed, signoff commit only when the user explicitly requests a commit. Preserve the prepared message’s line breaks.

## Constraints

- Do not create commits, amend commits, or create branches unless the user explicitly requests that action.
- Do not use interactive Git commands.
- Do not commit likely secrets, including `.env` files, credentials, or tokens.
- If there are no applicable changes, report that and stop.
- When asked for message-only output, return only the proposed commit message.

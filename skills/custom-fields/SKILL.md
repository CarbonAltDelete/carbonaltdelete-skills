---
name: custom-fields
description: Manage Carbon+Alt+Delete Boundaries custom fields and dropdown options for a user-confirmed company, including safe upserts, cleanup, deduplication, and ordering.
---

# Boundaries: custom fields

Use this skill when a Carbon+Alt+Delete user wants to create, update, or organize custom fields or their allowed dropdown options in the **Boundaries** module.

## Required company selection

Custom fields are company-scoped. Never assume, infer, or reuse a `company_id` from a prior task without the user's explicit confirmation for this request.

1. Call `list_companies` to obtain the accessible companies and their IDs.
2. Ask the user to select the intended company when more than one company is available, or when the user has not explicitly provided a company ID.
3. Proceed only with the company ID the user explicitly confirms. If they provide an ID, validate it against `list_companies`; do not silently substitute a different company.

## Clean requested names before mutation

Before comparing, creating, or updating fields and options:

1. Strip leading and trailing whitespace and collapse internal repeated whitespace to one space.
2. Spell-check field and option labels. Treat names, supplier names, acronyms, identifiers, and deliberately stylized text as potentially intentional.
3. Apply only unambiguous spelling corrections. For any uncertain correction, show the original and proposed value to the user and request confirmation before changing it.
4. Reject empty labels after cleaning.
5. Compare labels using a normalized key: Unicode-normalized, whitespace-collapsed, and case-insensitive. Preserve the approved display casing in stored values.

## Upsert fields and options

Always inspect the existing state before writing. Never blindly create a field or option.

1. Call `list_custom_fields` for the confirmed company.
2. Match the requested field name against existing fields by normalized key.
   - **No match:** create it with `create_custom_field`.
   - **One match:** use the existing field; update it with `update_custom_field` only when the requested display name or position differs.
   - **Multiple matches:** do not mutate automatically. Report the duplicate fields and ask the user which one to retain or how to resolve them.
3. Call `list_custom_field_options` for the confirmed company and isolate the options belonging to the selected field.
4. Clean and deduplicate both the requested options and the existing options using the normalized key.
   - Do not create an option that already exists under the selected field.
   - If existing duplicate options have the same normalized key, do not choose or delete one automatically; report them because the available tools do not provide option deletion.
   - Create only missing, approved options with `create_custom_field_option`.
   - Use `update_custom_field_option` only to apply an approved label correction, move an option, or otherwise reconcile the desired state.

## Option ordering

Default option ordering is alphabetical by normalized display label. The user may explicitly provide a different order; follow that order instead.

After all requested options exist:

1. Build the complete, deduplicated option set for the selected field.
2. Sort it alphabetically by normalized label unless the user supplied an explicit order.
3. Assign consecutive zero-based positions.
4. Update the positions of existing options as necessary with `update_custom_field_option`.
5. Create new options at their intended position. If creating options changes later positions, perform a final position reconciliation with `update_custom_field_option`.

## Action transparency

After completing a material action, give the consultant a concise user-visible action log:

```text
[Custom Fields] Action completed: <what happened>
Result: <verified outcome or affected record>
Next: <next action, if any>
```

Material actions include CAD MCP reads or writes, record changes, delegated work, diary updates, and Support searches. Report completion only after verifying the tool or delegated result. Do not expose credentials, access tokens, confidential raw data, personal data, or private reasoning. Combine closely related successful operations into one concise log entry. If an action is blocked, fails, or needs confirmation, state that plainly rather than presenting it as completed.

## Safe response

Before writes, summarize the confirmed company, cleaned labels, intended creations, updates, duplicate findings, and final option order. After writes, report the field ID, created or updated option values, skipped duplicates, and any unresolved ambiguity.

Do not use custom fields or options to record measurements, activity taxonomy, organizational structure, or reporting periods.

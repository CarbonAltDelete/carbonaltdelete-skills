---
name: custom-fields
description: Create, rename, clean up, deduplicate, and reorder Carbon+Alt+Delete Boundaries custom fields and their dropdown options for a confirmed company. Use when the user asks to add, change, tidy, or sort custom fields or dropdown values in Carbon+Alt+Delete.
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
4. Reject empty requested text labels after cleaning. The API also accepts null labels; preserve existing nulls and clear a label only when explicitly requested.
5. Keep null labels separate from text comparisons; never normalize null to an empty string or use it to infer a duplicate. Compare text labels using a normalized key: Unicode-normalized, whitespace-collapsed, and case-insensitive. Preserve the approved display casing in stored values.

## MCP payload contract

Use public tool names; resolve any client prefix from the connected tool list. Application keys use snake_case. Use `cad_tool_catalog(category="tags")` for routing; only refresh-capable clients can add the tags category through `focus_context` and refresh `tools/list`. Verify connected schemas before writing; reviewed documentation is not proof of deployed parity.

All writes require company WRITE access. Create calls use top-level `company_id` and nested `field_create` or `option_create`; updates require complete `field_update` or `option_update` objects, not partial patches:

| Payload | Required keys | Label contract |
| ------- | ------------- | -------------- |
| `field_create` | `sibling_position` | Optional nullable `name`, maximum 128 characters |
| `field_update` | `id`, `name`, `sibling_position` | Required nullable `name`, maximum 128 characters |
| `option_create` | `field_id`, `sibling_position` | Optional nullable `value`, maximum 1024 characters |
| `option_update` | `id`, `field_id`, `value`, `sibling_position` | Required nullable `value`, maximum 1024 characters |

`sibling_position` is a nonnegative, zero-based integer. When changing only order, preserve the existing label and option parent. Use `read_custom_field(company_id, field_id)` or `read_custom_field_option(company_id, option_id)` to obtain full current values as needed.

Example field update:

```json
{
  "company_id": "11111111-1111-4111-8111-111111111111",
  "field_update": {
    "id": "22222222-2222-4222-8222-222222222222",
    "name": "Priority",
    "sibling_position": 0
  }
}
```

## Upsert fields and options

Always inspect the existing state before writing. Never blindly create a field or option.

1. Call `list_custom_fields` for the confirmed company.
2. Match the requested field name against existing fields by normalized key.
   - **No match:** create it with `create_custom_field` if within the 10-existing-fields-per-company quota. The preflight count is advisory; the server enforces the limit. At the limit, explain the quota and offer to update an existing field.
   - **One match:** use the existing field; update it with `update_custom_field` only when the requested display name or position differs.
   - **Multiple matches:** do not mutate automatically. Report the duplicate fields and ask the user which one to retain or how to resolve them.
3. Call `list_custom_field_options` for the confirmed company and isolate options by returned `field_id`. This list has no top-level `field_id` argument. Optional `jq_filter="map({id, field_id, value, sibling_position})"` preserves the required context; fields can use `jq_filter="map({id, name, sibling_position})"`.
4. Clean and deduplicate both the requested options and the existing options using the normalized key.
   - Do not create an option that already exists under the selected field.
   - If existing duplicate options have the same normalized key, do not choose or delete one automatically; report them because the available tools do not provide option deletion.
   - Create only missing, approved options with `create_custom_field_option`.
   - Use `update_custom_field_option` only to apply an approved label correction, move an option, or otherwise reconcile the desired state.

## Option ordering

Default option ordering is alphabetical by normalized display label. The user may explicitly provide a different order; follow that order instead.

For text labels, use the following ordering workflow. Preserve unlabeled options and their order unless the user specifies how to place them; resolve ambiguous placement before sorting. Duplicate avoidance is a workflow check, not server-enforced label uniqueness.

Plan the final order before creating options, then reconcile after writes:

1. Build the complete, deduplicated option set for the selected field.
2. Sort it alphabetically by normalized label unless the user supplied an explicit order.
3. Assign consecutive zero-based `sibling_position` values.
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

List fields and options again after writes and verify returned IDs, parent `field_id`, labels, and `sibling_position`. Do not report success from the request alone.

Before writes, summarize the confirmed company, cleaned labels, intended creations, updates, duplicate findings, and final option order. After writes, report the field ID, created or updated option values, skipped duplicates, and any unresolved ambiguity.

Do not use custom fields or options to record measurements, activity taxonomy, organizational structure, or reporting periods.

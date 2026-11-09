# Carbon+Alt+Delete MCP tool catalog

This catalog describes the reviewed CAD MCP contracts, including the simulation target below. It is a routing reference, not evidence that a deployment has those changes. Verify the connected server's `tools/list` schemas before use; release requires parity with the matching reviewed Backend implementation.

## Scope and discovery

- **CAD-owned public names:** 51 in the reviewed inventory below, including core discovery. The earlier authenticated snapshot from `2026-10-14T15:49:30Z` reported 48; neither count establishes the current connected inventory.
- **Federated integration:** OCAI procurement emission-factor mapping tools, discovered separately from the live server.
- **Public names:** client prefixes and display titles do not rename the tools below.
- **Tool-list behavior:** use `cad_tool_catalog()` or a category such as `cad_tool_catalog(category="tags")` for routing. `focus_context` is exposed only to refresh-capable clients; after changing focus, refresh `tools/list` in response to `notifications/tools/list_changed`. Static clients cannot unlock tools through focus. In auto scope, refresh-capable clients start in core; static or unknown clients see all categories. Configured core/all scopes can differ.
- **Permissions:** annotations describe side effects and repeat behavior; they do not grant access.

## Tool-selection principles

- Use `list_*` tools to discover IDs before `read_*` or `update_*` tools.
- Activity groups and categories are reusable taxonomy; inventory lines are measured activity data.
- Emission factors convert activity data to emissions; they do not record activity data.
- Organizational units describe company structure; simulation groups organize scenario planning work.
- Custom fields and options describe records; they do not create company structure, reporting periods, or measurements.

## Core discovery

| Tool               | Purpose                                                                                            |
| ------------------ | -------------------------------------------------------------------------------------------------- |
| `cad_tool_catalog` | Returns the static, grouped CAD tool catalog and routing guidance.                                 |
| `focus_context`    | Inspects or changes session tool categories with get/set/add/remove; refresh-capable clients only. |

## Accounts and companies

| Tool             | Purpose                                                     |
| ---------------- | ----------------------------------------------------------- |
| `list_companies` | Lists active companies available to the authenticated user. |

## Organizational units

| Tool                         | Purpose                                 |
| ---------------------------- | --------------------------------------- |
| `list_organizational_units`  | Lists a company's organizational units. |
| `read_organizational_unit`   | Reads an organizational unit by ID.     |
| `create_organizational_unit` | Creates an organizational unit.         |
| `update_organizational_unit` | Updates an organizational unit.         |

## Reporting periods

| Tool                      | Purpose                                                             |
| ------------------------- | ------------------------------------------------------------------- |
| `list_reporting_periods`  | Lists reporting periods.                                            |
| `read_reporting_period`   | Reads a reporting period by ID.                                     |
| `create_reporting_period` | Creates a fiscal year, calendar year, or custom reporting interval. |
| `update_reporting_period` | Updates a reporting period.                                         |

## Activity taxonomy

| Tool                          | Purpose                                                                            |
| ----------------------------- | ---------------------------------------------------------------------------------- |
| `list_activity_groups`        | Lists reusable activity groups.                                                    |
| `read_activity_group`         | Reads an activity group by ID.                                                     |
| `create_activity_group`       | Creates an activity group.                                                         |
| `update_activity_group`       | Updates an activity group.                                                         |
| `list_activity_categories`    | Lists reusable activity categories.                                                |
| `read_activity_category`      | Reads an activity category by ID.                                                  |
| `duplicate_activity_category` | Duplicates an existing category as the required starting point for a new category. |
| `update_activity_category`    | Updates an activity category.                                                      |

## Measurements and inventory lines

| Tool                   | Purpose                                                                                |
| ---------------------- | -------------------------------------------------------------------------------------- |
| `list_inventory_lines` | Lists measured inventory lines for a company, reporting period, and activity category. |

## Emission factors

| Tool                      | Purpose                                                                          |
| ------------------------- | -------------------------------------------------------------------------------- |
| `list_datasets`           | Discovers accessible datasets and dataset versions.                              |
| `search_emission_factors` | Searches using `search_context`, optionally restricted by `dataset_version_ids`. |
| `create_emission_factor`  | Creates a custom emission factor.                                                |

## Reports and accounting context

| Tool                               | Purpose                                                  |
| ---------------------------------- | -------------------------------------------------------- |
| `read_ghg_protocol_report_context` | Retrieves GHG Protocol reporting and accounting context. |

## Simulations and decarbonization planning

| Tool                             | Purpose                                                                          |
| -------------------------------- | -------------------------------------------------------------------------------- |
| `list_simulations`               | Lists existing active simulations without initialization.                        |
| `read_simulation`                | Reads the exact active, company-scoped simulation ID.                            |
| `create_simulation`              | Explicitly ensures one active simulation; requires WRITE access even on repeats. |
| `update_simulation`              | Updates a simulation.                                                            |
| `list_simulation_groups`         | Lists simulation groups.                                                         |
| `read_simulation_group`          | Reads a simulation group by ID.                                                  |
| `create_simulation_group`        | Creates a simulation group.                                                      |
| `update_simulation_group`        | Updates a simulation group.                                                      |
| `list_simulation_actions`        | Lists simulation actions.                                                        |
| `read_simulation_action`         | Reads a simulation action by ID.                                                 |
| `create_simulation_action`       | Creates a simulation action.                                                     |
| `update_simulation_action`       | Updates a simulation action.                                                     |
| `list_simulation_manipulations`  | Lists simulation manipulations.                                                  |
| `read_simulation_manipulation`   | Reads a simulation manipulation by ID.                                           |
| `create_simulation_manipulation` | Creates a simulation manipulation.                                               |
| `update_simulation_manipulation` | Updates a simulation manipulation.                                               |

## Benchmarks

| Tool               | Purpose                                      |
| ------------------ | -------------------------------------------- |
| `create_benchmark` | Initializes benchmark data for a company.    |
| `read_benchmark`   | Reads benchmark values and target context.   |
| `update_benchmark` | Updates benchmark values and target context. |

## Tags, custom fields, and options

| Tool                         | Purpose                                       |
| ---------------------------- | --------------------------------------------- |
| `list_custom_fields`         | Lists custom fields.                          |
| `read_custom_field`          | Reads a custom field by ID.                   |
| `create_custom_field`        | Creates a custom field.                       |
| `update_custom_field`        | Updates a custom field.                       |
| `list_custom_field_options`  | Lists a custom field's allowed option values. |
| `read_custom_field_option`   | Reads an option by ID.                        |
| `create_custom_field_option` | Creates an allowed option for a custom field. |
| `update_custom_field_option` | Updates a custom field option.                |

## Federated OCAI integration

OCAI is a separate, federated MCP server used for AI-assisted procurement emission-factor mapping. Its tools arrive through an unprefixed live proxy and can change independently; discover their current names from the server. Use it after tabular procurement or spend data is available and mapping assistance is needed.

CAD `search_emission_factors` is separate: it sends `search_context` to external OpenAI embedding computation, then searches the access-scoped CAD factor catalog and returns up to 25 matches. Access scoping does not mean all computation stays inside CAD. This catalog introduces no partner behavior or legal/residency-policy change.

## Typical CAD workflows

1. **Set up an account:** company → organizational units → reporting periods → activity taxonomy.
2. **Review recorded data:** inventory lines → emission-factor search or OCAI mapping → GHG Protocol reporting context.
3. **Plan decarbonization:** baseline measurements → simulation → groups, actions, and manipulations → benchmarks.
4. **Manage record metadata:** custom fields → custom-field options.

## Arguments and response filtering

Application arguments and response keys use snake_case; MCP envelope keys retain protocol casing. Discover IDs with list/read tools, and keep IDs inside the required nested update objects:

| Update tool area | Nested ID path |
| ---------------- | -------------- |
| Activity category / group | `activity_category_update.id` / `activity_group_update.id` |
| Organizational unit / reporting period | `organizational_unit_update.id` / `reporting_period_update.id` |
| Benchmark / simulation | `benchmark_update.id` / `simulation_update.id` |
| Simulation group / action / manipulation | `group_update.id` / `action_update.id` / `manipulation_update.id` |
| Custom field / option | `field_update.id` / `option_update.id` |

Updates require complete nested request objects, not partial patches. Custom-field creates use `field_create`; option creates use `option_create`, with parent `field_id` inside the payload. Ordering uses `sibling_position`. Factor creation uses nested `request`.

To create a category, list categories, call `duplicate_activity_category(company_id, activity_category_id)` on a suitable source, then update the returned copy using `activity_category_update.id`. This creates reusable taxonomy; `list_inventory_lines` reads measured data and does not create it.

List tools, `search_emission_factors`, and `read_ghg_protocol_report_context` accept optional `jq_filter`. For companies, use `map({id, name, display_name})`, `map(.id)`, or `map(select(.status == "ACTIVE") | {id, name})`. Object projections must be subsets of the public response schema; scalar JSON output is supported. Filtering reduces returned context, not access checks or underlying computation.

## Simulation lifecycle — release parity gate

The following requires the matching reviewed simulation implementation before release. Reviewed source is not deployed proof.

- `list_simulations(company_id)` returns accessible active simulations or `[]` without creating data or committing.
- `read_simulation(company_id, simulation_id)` reads that exact active ID within that company without initialization, substitution, or commit. Unknown, inactive, and inaccessible IDs yield safe errors.
- `create_simulation(company_id)` explicitly initializes with company WRITE access, including repeated calls. It returns the same active ID on repeats and retains archived history. If only archived history exists, initialization creates a new active simulation without restoring or overwriting history.
- Creation needs the reporting-period baseline; missing prerequisites and access failures must leave no partial data. Multiple active records produce a conflict rather than arbitrary selection or consolidation.
- Concurrent cooperating MCP create calls must not duplicate the active simulation. Legacy HTTP/API and template writers do not share that lock; no database-wide uniqueness guarantee is implied. List/read require only READ access.
- Responses retain `id`, `description`, `reporting_period_id`, `start_year`, `end_year`, `notes`, and `sibling_position`. These tools expose no archive operation; do not invent one.

Refresh this reference from `cad_tool_catalog` and `tools/list` when the connected catalog changes, retaining the release parity check.

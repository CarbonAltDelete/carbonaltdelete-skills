# Carbon+Alt+Delete MCP tool catalog

This catalog was retrieved from the authenticated Carbon+Alt+Delete MCP connection at `2026-10-14T15:49:30Z` using `cad_tool_catalog`. It lists the CAD-native tools available to the authenticated client and distinguishes the separately federated OCAI mapping integration.

## Scope

- **CAD-native named tools:** 48
- **Federated integration:** OCAI procurement emission-factor mapping tools
- **Tool-list behavior:** This client uses a static tool list; use `cad_tool_catalog` to choose a tool rather than relying on dynamic tool focusing.

## Tool-selection principles

- Use `list_*` tools to discover IDs before `read_*` or `update_*` tools.
- Activity groups and categories are reusable taxonomy; inventory lines are measured activity data.
- Emission factors convert activity data to emissions; they do not record activity data.
- Organizational units describe company structure; simulation groups organize scenario planning work.
- Custom fields and options describe records; they do not create company structure, reporting periods, or measurements.

## Core discovery

| Tool               | Purpose                                                            |
| ------------------ | ------------------------------------------------------------------ |
| `cad_tool_catalog` | Returns the static, grouped CAD tool catalog and routing guidance. |

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

| Tool                      | Purpose                                          |
| ------------------------- | ------------------------------------------------ |
| `search_emission_factors` | Searches for factors suitable for activity data. |
| `create_emission_factor`  | Creates a custom emission factor.                |

## Reports and accounting context

| Tool                               | Purpose                                                  |
| ---------------------------------- | -------------------------------------------------------- |
| `read_ghg_protocol_report_context` | Retrieves GHG Protocol reporting and accounting context. |

## Simulations and decarbonization planning

| Tool                             | Purpose                                               |
| -------------------------------- | ----------------------------------------------------- |
| `list_simulations`               | Lists simulations.                                    |
| `read_simulation`                | Reads a simulation by ID.                             |
| `create_simulation`              | Retrieves or creates the company's single simulation. |
| `update_simulation`              | Updates a simulation.                                 |
| `list_simulation_groups`         | Lists simulation groups.                              |
| `read_simulation_group`          | Reads a simulation group by ID.                       |
| `create_simulation_group`        | Creates a simulation group.                           |
| `update_simulation_group`        | Updates a simulation group.                           |
| `list_simulation_actions`        | Lists simulation actions.                             |
| `read_simulation_action`         | Reads a simulation action by ID.                      |
| `create_simulation_action`       | Creates a simulation action.                          |
| `update_simulation_action`       | Updates a simulation action.                          |
| `list_simulation_manipulations`  | Lists simulation manipulations.                       |
| `read_simulation_manipulation`   | Reads a simulation manipulation by ID.                |
| `create_simulation_manipulation` | Creates a simulation manipulation.                    |
| `update_simulation_manipulation` | Updates a simulation manipulation.                    |

## Benchmarks

| Tool               | Purpose                                      |
| ------------------ | -------------------------------------------- |
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

OCAI is a separate, federated MCP server used for AI-assisted procurement emission-factor mapping. It is not part of the CAD-native tool list above. Use it after tabular procurement or spend data is available and mapping assistance is needed.

## Typical CAD workflows

1. **Set up an account:** company → organizational units → reporting periods → activity taxonomy.
2. **Review recorded data:** inventory lines → emission-factor search or OCAI mapping → GHG Protocol reporting context.
3. **Plan decarbonization:** baseline measurements → simulation → groups, actions, and manipulations → benchmarks.
4. **Manage record metadata:** custom fields → custom-field options.

Refresh this document by calling `cad_tool_catalog` when the authenticated CAD MCP catalog changes.

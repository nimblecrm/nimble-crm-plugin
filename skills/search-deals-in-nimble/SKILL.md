---
name: search-deals-in-nimble
description: Find, filter, inspect, or compare deals in Nimble. Use for requests about deal names, owners, pipelines, stages, statuses, dates, amounts, or custom deal fields. This skill reads deals; it does not create or update them.
---

# Search deals in Nimble

Use the Nimble MCP connector as the source of truth.

## Build the search

- Call `get_deal_search_field_metadata` when the exact field name, supported operator, custom field alias, or
  account-specific value ID is uncertain. Use the returned search contract instead of guessing from UI column names.
- Use `possible_values` to translate owner, pipeline, stage, status, and other finite labels into their current account
  IDs. Never invent an ID.
- For deal names, use `contain` for complete words, `starts_with` for an unfinished last term, and `is` only when the user
  requests an exact name. These operators do not provide typo tolerance or arbitrary substring matching.
- Put all requested constraints in one `search_deals` query. Do not discard non-name filters when refining a name search.
- Deals have no last-activity field. For stalled or inactive deals, filter or sort by `entered_current_stage_stage`
  and tell the user the result reflects time in the current stage, not the latest activity. Compact results report
  that date as `entered_stage_at`, so no full response is needed to explain it. Won and lost deals stay in their final
  stage too, so add `{"deal_status":{"is":"open"}}` unless the user asks about closed deals.
- When the user asks for an unfiltered deal list, call `search_deals` without a query instead of constructing a wildcard.

## Inspect the results

- Start with the default compact response. Use `get_deal` to inspect the full card for a selected deal; request the
  full search response only when complete data are needed for several matches.
- Keep the same query and sort while paging. Do not present one page as the complete result set when more pages exist.
- Ground comparisons and summaries in returned deal fields, owners, pipelines, stages, and related contacts. Clearly
  distinguish missing values from values that were not included in the selected response format.
- If the request remains ambiguous after metadata discovery, show the relevant choices or ask which field, pipeline,
  stage, or deal the user means.

Do not imply that a deal was created or changed. Use `manage-deals-in-nimble` for requested writes.

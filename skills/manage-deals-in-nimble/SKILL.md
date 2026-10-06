---
name: manage-deals-in-nimble
description: Create or update individual deals in Nimble, including discovering writable field IDs, pipelines, and stages. Use for changes to deal values or relationships, not deal field or pipeline structure.
---

# Manage deals in Nimble

Use the Nimble MCP connector as the source of truth.

1. Use `search_deals` to find an existing deal and `get_deal` to inspect its full card and `is_editable` before an update.
   Use `get_deal_search_field_metadata` only to build search queries.
2. Call `get_deal_field_metadata` for field IDs, types, and value `read_only` flags, and `list_deal_pipelines` for current
   pipeline and stage IDs. Use fields with `read_only: false`; `available_actions` describes structural edits. Prefer
   field IDs because custom field names can repeat across pipelines. Do not guess IDs.
3. For `create_deal`, provide `fields_values` with a name entry and either a pipeline ID or a non-final stage ID.
   Values are lists of `{ "value": "text" }` objects; send numbers and dates as strings in their field's expected
   format. If the selected stage has no default probability, provide a probability field value.
4. For `update_deal`, send only properties to change. Inside `fields_values`, an empty list clears that field; other
   fields stay unchanged. `related_contacts`, `related_external_contacts`, and `tags` replace their complete lists,
   so use `[]` only when clearing them. A stage change must stay within the current pipeline and cannot mark a deal won
   or lost. Do not use `null` for `fields_values` or `stage_id`.
5. Both write tools return the full saved deal. Check the returned `deal_id`, changed values, and `is_editable` before
   reporting success. If a create response is ambiguous after a transport failure, search for its exact name and
   pipeline, then inspect any match with `get_deal` before retrying. If the result remains ambiguous, ask the user
   before another create to avoid a duplicate.

The REST API enforces visibility, edit permission, and account-specific field validation. Report permission or validation
errors; do not switch to another identity or write route.

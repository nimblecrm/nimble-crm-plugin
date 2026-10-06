---
name: manage-contact-fields-in-nimble
description: Create or update contact custom fields, field tabs, groups, and choice options in Nimble. Use when the user asks to customize the contact field structure. Do not use for editing values on individual contacts.
---

# Manage contact fields in Nimble

Use the Nimble MCP connector as the source of truth.

1. Call `get_contact_fields_structure` to inspect the current tabs, groups, fields, IDs, ordering, and `available_actions`.
   Reuse existing structure when it fits the request; never guess IDs or logo values. If the user does not specify a
   position, use the last existing sibling's ID as `insert_after` to append; use `null` only for an empty parent or when
   placing the item first is intended.
2. For a new tab, call `create_contact_field_tab` with the requested person/company contact types. For a new group, call
   `create_contact_field_group` with its tab ID and a valid `logo_id` from an existing group; `"1"` is a known example.
3. Call `create_custom_contact_field` with the target tab and optional group. A choice field needs
   `field_type.values.ordering_type` and stable `{id, value}` options. Number and datetime fields need a matching
   `presentation`. Read the returned structure for the new ID before subsequent changes.
4. Use the corresponding update tools for existing tabs, groups, fields, and choice options. Updates change only supplied
   properties. An explicit `null` for `insert_after` moves an item first; `group_id: null` removes a field from its group.
   When moving a field to another tab, supply its destination `group_id` or `null` explicitly. Field type and multiplicity
   cannot be changed by the update endpoint.
5. To add or rename an option after creating a choice field, use `create_contact_field_choice` or
   `update_contact_field_choice`.

Each write returns the full current structure. Creating a tab, then a group, then a field is a sequence of separate
writes: if a later call fails, earlier changes remain. Check the returned structure before retrying an ambiguous failure.
Do not create duplicate tabs, groups, fields, or options during retries. Writes require the account's Manage Custom Fields
permission; report a permission error rather than attempting another route.

---
name: find-contacts-in-nimble
description: Find, filter, and review people and companies in Nimble, including who has not been contacted recently, who is due a Stay in Touch reminder, and likely duplicates of a contact, and save a search as a reusable list. Use for contact lists, follow-up planning, relationship recency, tags, owners, field-based contact lookups, or saved searches. This skill does not create, update, tag, or merge contacts.
---

# Find contacts in Nimble

Use the Nimble MCP connector as the source of truth. Discover real IDs and field names; never invent them.

## Build the search

- Put every requested constraint into one `search_contacts` query, combining terms with `and`, `or`, and `not`.
  Use `__any` with `contain` for a loose name or company lookup, and exact identifiers such as email or domain when
  the user provides them.
- Call `get_contact_search_field_metadata` when a field name, operator, or custom field handle is uncertain, and use the
  returned `search_name`. Sort only by fields it marks `sortable`.
- Use `record type` to separate people from companies when the request implies one of them.
- For a tag filter, check the exact tag name with `list_contact_tags(starts_with=...)`, then search by the tag field
  that `get_contact_search_field_metadata` returns.

## Relationship recency

- "By me" means `user last contacted`; "by anyone on the team" means `company last contacted`. Ask only when the
  difference matters and the request does not make it clear.
- Last contacted reflects emails, calls, past calendar events, and activity types configured to count. Notes, tags,
  and field edits do not update it.
- For "not contacted in N days", use `not_in_the_last` with `{"unit": "day", "quantity": N}`. It matches only contacts
  that were contacted at some point; add `{"is_empty": true}` on the same field in an `or` when never-contacted contacts
  belong in the answer, and say which case each contact falls into.
- Sort by the same field ascending to list the longest-neglected contacts first, or descending for the most recent.
- Compact results include `last_contacted_by_me` and `last_contacted_by_team`; a missing value means no recorded
  interaction, not an unknown date.
- For "who should I reach out to today", call `list_stay_in_touch_reminders` with `status="due"`; use `status="all"` to
  list every active reminder. Reminders belong to the current user. Use `save-contact-to-nimble` to set, reset, or
  remove one.
- Sequences are read-only through Nimble tools. `list_contact_sequences` shows the sequences a contact is in, but the
  assistant cannot enroll or remove contacts; send the user to Nimble for that.

## Read the results

- Start with the default compact response. It keeps common fields and at most ten tags; `tags_total` shows that more
  exist. Before reasoning about all tags of a contact, read it with `get_single_contact`.
- Request `response_format="full"` only when custom fields of several matches are required; full pages are large.
- Keep the same query and sort while paging and use `meta` to report how many contacts matched. Never present one page
  as the complete result.
- Clearly separate an empty result from a field that was not included in the selected response format.

## Save the search

When the user asks to save a list or share it with the team:

1. Check `list_contact_saved_searches` for a saved search with the same name. If one exists, ask whether to update it
   with `update_contact_saved_search` or choose another name.
2. Confirm the name, describe in plain words which contacts the filter returns, and say whether it will be shared with
   the whole company (`is_shared`).
3. Call `create_contact_saved_search` with the same query and sort that produced the results the user saw.

Delete a saved search only when the user asks: name it, wait for an explicit yes, then call
`delete_contact_saved_search`.

## Duplicates

- To check one contact for duplicates, call `find_contact_duplicates` with its ID. Explain each candidate with its
  `matched_fields`; `likeness` ranks candidates but does not prove they are the same person or company.
- Use `save-contact-to-nimble` when the user wants to merge records or change a contact.

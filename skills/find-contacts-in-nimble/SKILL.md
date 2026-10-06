---
name: find-contacts-in-nimble
description: Find, filter, and review people and companies in Nimble, including who has not been contacted recently and likely duplicates of a contact. Use for contact lists, follow-up planning, relationship recency, tags, owners, or field-based contact lookups. This skill reads contacts; it does not create, update, tag, or merge them.
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

## Read the results

- Start with the default compact response. It keeps common fields and at most ten tags; `tags_total` shows that more
  exist. Before reasoning about all tags of a contact, read it with `get_single_contact`.
- Request `response_format="full"` only when custom fields of several matches are required; full pages are large.
- Keep the same query and sort while paging and use `meta` to report how many contacts matched. Never present one page
  as the complete result.
- Clearly separate an empty result from a field that was not included in the selected response format.

## Duplicates

- To check one contact for duplicates, call `find_contact_duplicates` with its ID. Explain each candidate with its
  `matched_fields`; `likeness` ranks candidates but does not prove they are the same person or company.
- Use `save-contact-to-nimble` when the user wants to merge records or change a contact.

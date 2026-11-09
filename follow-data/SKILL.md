---
name: follow-data
description: Explain why a named field, object, state, or data shape matters by following its writers, readers, stored copies, and caches. Use when the user asks who writes or reads a thing, when it changes, where copies live, how freshness works, or what depends on it.
---

# Follow data

Take the thing named after the command and explain its working life. Lead with why it exists and what decisions or visible behavior depend on it. The reader should be able to answer: **If this value changed, disappeared, or stayed stale, what would happen next?**

This follows one logical thing across its uses, rather than tracing every step of a single request. Default to read-only investigation; explaining the thing does not authorize changing it or testing a live write.

## Investigate thoroughly, explain simply

Investigate as deeply as needed to establish what happens and why. Distill the result into the simplest possible plain-English explanation that lets the user understand the situation accurately: what happens, why, and what it means.

Keep any uncertainty, condition, or caveat that materially changes the answer. Include technical names, exact values, tables, and diagrams only when they help the user understand or verify it.

When detailed evidence, a full trace, or a data inventory is useful, use the project’s reporting location, or a user-approved output directory, and link it. A report is optional; the chat response must still stand on its own. The investigation steps below guide the work, not the length or structure of the response.

## Find the actual thing

Resolve the name to an exact field, type, record, expression, or UI value. Identify what it represents, its identity/key and scope, and any units or meanings that change as it travels. Distinguish similarly named values. Ask only if the ambiguity would lead to materially different answers.

Start from its definition and real call sites. Follow aliases, serializers, derived fields, and storage keys rather than stopping at a text search. A schema says what can exist; it does not identify who actually fills it in or uses it.

## Follow the writes and reads

For each meaningful writer, establish:

- Who creates or changes the value, and from what inputs?
- What triggers the write: a user action, request, tick, event, retry, or scheduled job?
- Is it created once, overwritten, appended, recomputed, or deliberately frozen?
- Where does the result land, and what makes it visible to the next reader?

For each meaningful reader, establish:

- Which exact copy does it read, and when?
- What does it use the value to decide, calculate, send, or display?
- What happens if the value is absent, stale, or disagrees with another copy?

Group transparent pass-throughs. Keep a hop visible if it changes meaning, identity, timing, or what a later reader can know. A component rendering again does not imply a network read; a read does not imply fresh data.

## Account for copies

Inventory the meaningful places the thing lives: durable records, historical snapshots, queues, service memory, shared caches, response payloads, browser caches, and local UI state where relevant.

Distinguish an independent copy from a reference to the same object, and a derived value from a duplicate. A frozen historical record is not an expired cache.

For each copy or derived value, identify its key/scope, how it is populated, who reads it, what refreshes or invalidates it, and how long it survives. Name the event that replaces a stale value, not merely a TTL setting. Check whether refresh depends on a mounted view, polling, a write notification, or the next request. Where relevant, say whether expiry removes data or merely marks it stale.

Count **copy locations or kinds** separately from runtime instances: a process cache may exist once per worker, and a query cache once per browser. Do not claim a fixed total when deployment or client counts are unknown. Say which copy readers use when values disagree, or explicitly state that no reconciliation rule was found.

## Explain it compactly

Use only the parts below that help answer the question. Put a detailed inventory in the linked report when needed:

1. **Why it matters:** one short paragraph connecting the value to an actual decision or user-visible result.
2. **One concrete trip:** a small story or arrow diagram showing a representative write reaching its readers. Name triggers and any waits that explain freshness.
3. **Copies and consumers:** a compact table, one row per meaningful location, with who writes it and when, who reads it and why, and how it refreshes or disappears. State the count and its scope.
4. **What to notice:** material uncertainty or a demonstrated problem, such as a writer with no found consumer, conflicting writers, a cache without a refresh path, or a default that hides missing data. Do not invent a redesign merely because copies exist.

Cite the decisive code locations or runtime evidence close to the claims. Separate what code defines from what was observed running. Report the inspected scope and consequential unknowns; absence from a bounded search is not proof of absence everywhere.

Use plain event-shaped sentences: “After the API saves the order, the worker reads that saved amount to size the trade.” Avoid making “owner,” “source of truth,” or “cached” stand in for who acts, which copy is read, and when it changes.

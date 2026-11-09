---
name: trace
description: Trace software, agent, API, or data behavior as a replayable sequence of concrete actors, exact values, calls, waits, saved records, and observed effects. Use when the user asks to trace or walk through behavior, compare before/current/ideal designs, find where fields are lost or regenerated, or diagnose unexpected behavior in a deployed system.
---

# Trace

Paint a story the user can replay. Something happens. A named actor receives exact values, checks or changes them, then calls, queues, saves, returns, or shows something. Say who waits, who continues, and what actually happened.

Default to read-only investigation (read-only workflows against deployments are ok). Do not change code or live systems unless the user separately asks for implementation or an operational change.

## Investigate thoroughly, explain simply

Investigate as deeply as needed to establish what happens and why. Distill the result into the simplest possible plain-English explanation that lets the user understand the situation accurately: what happens, why, and what it means.

Keep any uncertainty, condition, or caveat that materially changes the answer. Include technical names, exact values, tables, and diagrams only when they help the user understand or verify it.

When detailed evidence, a full trace, or a data inventory is useful, use the project’s reporting location, or a user-approved output directory, and link it. A report is optional; the chat response must still stand on its own. The investigation steps below guide the work, not the length or structure of the response.

## Core Vocabulary

| Word | Means | Does not prove |
|---|---|---|
| `trigger` | The fact that starts or resumes this path | Any actor received it or finished work |
| `actor` | A named participant performing a named action | Exclusive responsibility for the whole flow |
| `value` | The actual data available here | Correctness or persistence |
| `reference` | An ID or location used to find a value | The value exists or was loaded |
| `copy` | One place's version of a logical value | Agreement with other copies |
| `record` | A copy saved for later reads | Freshness, correctness, or whether reads use it |
| `attempt` | One try to cause a named effect | The receiver acted or the effect happened |
| `result` | What one actor returns or reports | Saving, display, or a later effect |
| `effect` | The actual change being checked | Anything beyond that named change |

`input` and `output` describe the role a value plays before and after an actor works. They are not separate kinds of data.

Use these derived terms only when they clarify the story:

- `state`: mutable values kept for the next decision.
- `status`: a concise label describing where something stands.
- `cache`: a disposable copy that may be stale and can be rebuilt.

## Core Grammar

```text
When <trigger>, <Actor> receives <value or reference>.
<Actor> reads, checks, or chooses.
It keeps, drops, adds, or derives values.
Then it calls, queues, publishes, saves, returns, or shows <exact output>.
```

Use the plain job name first, then attach the exact code name when it helps the reader verify the trace.

## Use Verbs That Predict the Next Event

- `receives`: data reaches the actor; this does not show that the actor checked or used it.
- `reads`: the actor loads a saved or cached copy.
- `checks`: the actor compares exact values with a named rule. Name the rule and what happens when it fails.
- `chooses`: the actor selects a branch or value; this does not show that the selected work happened.
- `keeps`, `drops`, `adds`, and `derives`: describe outgoing fields relative to incoming fields.
- `saves`: storage confirms that a record survives the current work.
- `shows`: a person can now see the value.
- `verifies`: a named fact establishes a named claim. If it only supports the claim, mark the claim inferred.

Do not use `transforms`, `processes`, or `handles` as the explanation. Say what the actor keeps, drops, adds, derives, saves, or shows.

## Show Who Waits

Use the transfer verb that matches the real call:

```text
A calls B with X and waits for Y.
B returns Y; A continues.
```

```text
A queues X and waits only for the queue to confirm it was saved.
A continues. B reads X later.
```

```text
A publishes event X and continues.
Zero or more actors may receive it.
```

Calling a flow "asynchronous" is not enough. A caller may wait for a queue acknowledgement while not waiting for the worker. Name exactly who waits for what.

## Do Not Let an Earlier Event Prove a Later Event

```text
request -> acknowledgement -> result -> effect
```

- A `request` asks an actor to work.
- An `acknowledgement` confirms one named actor accepted one named thing.
- A `result` reports what that actor concluded.
- An `effect` is the actual change being checked.
- An `event` reports what an actor observed or decided; it does not prove a subscriber received or acted on it.

No earlier event proves a later event. A timeout proves only that no result arrived before the deadline. If an effect could have happened, distinguish starting another attempt from checking the existing attempt. When code calls something a receipt, state the exact fact that receipt proves.

## Trace the Real Path

1. Establish the trigger and the final result or effect in scope. Distinguish one product turn from the model calls, tool calls, or jobs inside it.
2. Include actors that decide, change, save, show, or can drop, delay, duplicate, or retry values. Group transparent pass-throughs unless they explain the behavior.
3. Follow the actual call order in code, logs, tests, and saved records. Do not infer behavior from class or function names alone.
4. For each call, queued job, read, or save, show only the fields that explain the next event or can be lost.
5. State who waits, who continues, which acknowledgement or result returns, and what effect has not happened yet.
6. State which actor creates or changes each mapping and which copy reads use when copies disagree.
7. Mark each important claim as defined, observed, inferred, or unknown.

- `defined`: code or a schema describes the behavior; this does not show a runtime occurrence.
- `observed`: a log, trace, test run, or saved value shows one occurrence; this does not establish universal behavior.
- `inferred`: known facts support the explanation, but do not directly show it.
- `unknown`: the inspected material cannot decide. Name the exact check that would resolve it.

## Trace Unexpected Live Behavior

For unexpected behavior in a deployed system, trace one observed example from its trigger to its returned or saved result and observed effect:

```text
trigger -> actor -> calls or queued work -> next actor -> result -> effect
```

- Confirm the active environment and deployed revision before using local source to explain runtime behavior.
- Follow one request, job, run, event, or user action end to end using its ID when available. At each step, show the exact values and status, elapsed time, and what the actor waited for.
- For slow behavior, show which work ran in sequence and which overlapped. Separate network wait, service work, database or provider wait, disk or CPU work, queue delay, and retries.
- For wrong, stale, missing, or partial results, follow the exact value through each read, choice, save, return, and display. Name the first actor whose actual output differs from the expected output.
- For queued or background work, distinguish the queue acknowledgement, worker result, saved record, and observed effect.
- When possible, compare one failing example with a working example and find the first meaningful difference.
- Call it the root cause only when that difference explains the later symptom. Otherwise mark it inferred or unknown and name the next check.

A successful deployment or health check proves only that check. It does not prove that the affected path and its dependencies work.

## Present the Trace

Start with the smallest useful overview. Show synchronous and later work separately when one arrow would hide who waits:

```text
Caller --calls and waits--> API --returns result--> Caller
API --queues request--> Worker later --saves record--> Database
```

Use compact JSON input and output shapes when they reveal the behavior. For actors included in the explanation, state the checks, changes, waits, and unknowns needed to understand the result.

For **Before**, **Current working tree**, and **Ideal**, keep one representative example stable. Fully trace the current path, then show only changed steps for the other variants unless separate full traces are clearer.

Use a compact comparison table when it makes the differences easier to understand:

| Version | Changed actor or event | Values passed or saved | What the inspected material shows |
|---|---|---|---|
| Before | ... | ... | ... |
| Current | ... | ... | ... |
| Ideal | ... | ... | ... |

## Prefer Events Over Abstract Labels

| Avoid as explanation | Say what happens |
|---|---|
| `owner` or `ownership` | Who creates, checks, chooses, saves, or supplies the value |
| `boundary` or `handoff` | A calls B, queues work for B, publishes an event, or returns to A |
| `context` or `payload` | The exact values and references the actor receives |
| `projection` | B reads X and derives Y without saving Y |
| `source of truth` | Reads use X when copies disagree |
| `contract` | The exact input required, output returned, or promise checked |
| `transformation`, `processing`, or `handling` | The values kept, dropped, added, or derived |

## Presentation Rules

- Tell the tiny story before naming the code abstraction.
- Use concrete actors and active verbs.
- Preserve exact code identifiers where they let the reader verify the trace.
- Cite exact files and line numbers for code-grounded claims when available.
- Call out a misleading field name or promise directly.
- Separate an observed failure from a proposed repair.
- Prefer text arrows for simple flows. Use Mermaid only when branches or loops are otherwise hard to follow.
- Keep the answer compact enough that the entire flow remains visible at once.
- Lead with the answer; add only the explanation needed to understand it without being misled.

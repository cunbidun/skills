---
name: distill
description: >
  Reframe a tangled explanation, problem, solution, or concern through clean,
  simple primitives and a concrete causal story. Use when the user invokes
  distill, asks for a simpler mental model, flags wording in the last response,
  says they do not follow, or the explanation accumulates jargon and noun piles. Recover relevant
  compasses, blueprints, and north-star references the user supplied; otherwise
  reason out what the smallest useful primitives ought to be. This is deeper
  than proofreading or merely shortening text.
---

# Distill

Step back and understand the system before rewriting the sentence. The reader
should be able to picture the things involved, replay what happens, and see why
the concern or proposed change follows. Write for a smart, tired person who
should not have to decode your terminology.

## When correcting the last response

Treat the user's quoted words or snippets as pointers to what went wrong in
the surrounding explanation, not as separate questions needing definitions.
The problem may be wording, framing, a mistaken premise, or an unnecessary
concept. Rebuild the answer around what it needed to explain using the few
existing things the user already understands.

Lead with the corrected explanation. Remove noun piles, unrelated actors, and
invented distinctions rather than defending the previous phrasing. Verify an
uncertain factual connection with a narrow read; do not expand a wording
correction into an unrelated investigation. Explain or justify a new concept
only if it is necessary to make the situation clear.

For a larger explanation or design concern, work through the steps below.
For a narrow correction, use only the steps needed to repair the answer.

**Recover the compass.**

Look back through the relevant conversation and user-supplied design references.
Bring the applicable compass, blueprint, glossary, or north-star principle back
into the explanation. Retrieve the relevant passage when its meaning matters;
do not reload every document or repeat its vocabulary without understanding it.

Use those references to recover the intended jobs of the primitives. Treat them
as guides unless the user made them requirements. Account for later decisions
and implementation changes. Say where today's behavior differs from the guide;
do not describe the intended design as if it were already implemented. If a
reference is unavailable, say so rather than inventing what it says.

**Find the few things that explain the behavior.**

Start with the user's goal. Identify the objects, actors, and decisions needed
to explain it. Prefer established domain words when they still predict what
happens. Give an unfamiliar word a short operational meaning: "A Step is one
prepared operation in this Run."

For each thing you keep, work out what it represents, who acts on it, and what
would become impossible or ambiguous without it. A table, class, endpoint, or
existing status name does not justify a separate concept by itself. Conversely,
do not collapse two things whose distinction changes what may happen next.

If no useful vocabulary exists, propose the smallest set that makes the example
clear. A primitive should name a recognizable thing or decision, have one job,
and combine naturally with the others. Mark proposed primitives as proposed;
do not present them as existing code or invent a framework to explain one case.

**Replay one concrete example.**

Use one amount, object, or request throughout. Establish the normal path, then
change the one event that exposes the concern. Show only the actors that matter:
who asks, who saves, who acts, who checks, and who waits.

At the decisive moment, distinguish what was requested, what was saved, what may
have happened outside the system, and what was actually confirmed. For a restart,
show what survives and which actor is supposed to do the next thing. For a design
choice, show what each alternative changes in the same example. For a working
solution, show why its rule covers the troublesome case.

Name the first missing, mistaken, or duplicated decision. Avoid explanations
such as "both sides are behaving correctly" when one side fails its intended
job. Describe the consequence the user cares about: money remains reserved,
a purchase may repeat, a replacement never starts, or a page never arrives.

**Make the solution follow from the story.**

Explain the changed rule using the same primitives and example. Prefer removing
a duplicated fact or letting the existing responsible actor finish its job.
If the vocabulary cannot express a necessary distinction, identify that gap
before proposing another flag, table, or exception.

Distinguish unclear wording from an actual design problem. A simpler explanation
does not require changing a sound design. Nor does it authorize implementation,
new investigations, or live actions beyond the user's task.

Preserve the limits of the mechanism. A saved request does not prove it was sent.
A failed lookup does not automatically prove absence. A lease coordinates
workers; safe retries also depend on the saved request identity and the
provider's retry rules. Simplifying the prose must not strengthen the guarantee.

**Tell the reader the useful part.**

Lead with the main point. Develop it as a short story or a few ordered steps,
then explain the smallest justified change and any uncertainty that matters.
Use concrete subjects and verbs: "Wallet saves the request, then sends it."
Avoid replacing one noun pile with another or teaching the entire architecture.

Keep technical names and code references when they help someone verify or act
on the explanation; place them after the idea they support. Do not force a
fixed set of headings, a table, or a diagram onto every answer. Brevity is useful
only if the causal chain remains intact. Check that the reader can answer:
"What happened, why does it matter, and what changes next?"

**Example: saved work that never starts.**

Dense: "The durable dispatch handoff has a recovery ownership gap."

Distilled: "The Run has a Step to fund the child with $60. Wallet saves the
request and reserves $60, then crashes before sending. On the next tick, Exec
finds the saved request and waits for Wallet. Wallet's recovery path returns it
unchanged, so no one sends it and the $60 stays reserved. Wallet needs to resume
its own saved request under its existing worker claim and retry rules. Exec can
keep waiting for the result."

This example illustrates how to explain responsibility and progress. It does
not establish the current behavior of a repository; verify that separately
when the user's question depends on it.

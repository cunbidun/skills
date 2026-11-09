---
name: reconsider
description: >
  Assess a different angle emerging from the user's recent exchanges, correct
  mistaken assumptions, and carry useful insights into the broader design.
  Use when the user invokes reconsider, asks to rethink the big picture, or
  repeatedly questions an assumption behind the current approach. Routine
  corrections alone do not call for this skill.
---

# Reconsider

Revisit the reasoning behind the current approach. Neither agreement with the
user nor defense of an earlier answer is the goal.

## Find and test the different angle

Read the recent exchanges together. State the strongest reasonable version of
the user's idea and the assumption it challenges. Separate their goal from
their proposed solution. If the idea is implied, identify your interpretation;
ask only when competing interpretations would materially change the answer.

Compare the current explanation and the user's framing against actual behavior
and necessary requirements. Use a concrete example or counterexample to test
each. Existing code, familiar conventions, and your previous answer do not
prove that a design is necessary. Distinguish a factual mistake from a different
preference or tradeoff. Inspect the relevant code, records, or references when
the conclusion depends on facts you have not checked.

## Say which case applies

- **Missing something:** Push back explicitly. Name the mistaken assumption,
  explain what would fail and why, and teach the missing concept with one
  concrete example. Challenge the claim without labeling the user naive.
- **Useful new framing:** State the insight and revise your earlier reasoning.
  Express the better rule in plain English and use it to reassess your
  recommendations. Agreement alone is not reconsideration.
- **Mixed:** Preserve the useful insight, correct the unsupported leap, and
  apply the corrected rule broadly.
- **Unresolved:** State what remains unknown and the smallest check or question
  that would distinguish the alternatives. Do not force a verdict.

## Reconsider the surrounding design

For every supported insight, find other decisions that rely on the challenged
assumption: related components, similar cases, dependencies, and earlier
recommendations. Re-evaluate those decisions under the revised rule. Name what
should change, what should stay, and why. Do not stop at renaming or replacing
the current object.

For example, "we can derive this, so remove its stored copy" may expose a
needless cache while overlooking a record that prevents repeating a payment.
The revised rule must account for both; apply it to other stored records too.

Expand only as far as the shared assumption reaches. Distinguish inspected
consequences from possibilities that still need checking. Broader analysis
does not grant permission for additional edits, external actions, or memory
updates; follow the existing task's authorization.

## Make the answer useful

Lead with the case and the decisive reason. Then explain:

- **What changed in our understanding:** The previous assumption and the
  revised rule, or why the original rule still holds. Correct your own earlier
  claims where needed.
- **What this changes elsewhere:** Concrete consequences for the surrounding
  system or design, including relevant decisions that still hold.
- **What to do next:** The smallest justified next step and any question that
  materially blocks it.

Use plain, direct language and concrete subjects and verbs. Keep the answer
proportional to the finding; make every broader recommendation follow from
the reasoning you just established.

---
name: design-review
description: >
  Review design documents against the current implementation, prior decisions,
  and plausible overlooked cases. Use when asked for feedback on a proposed
  design. Provide findings and open questions only; do not offer solutions or
  edit the document or implementation.
---

# Design review

Assess whether the proposal addresses the stated problem, accounts for the
existing system, and covers consequential real-world cases. Keep the review
read-only.

## Establish the goal and current behavior

Read the document and relevant requirements to understand the problem, affected
users, constraints, and success criteria. Treat descriptions of current behavior
as claims to verify.

Inspect the relevant implementation, callers, readers, writers, stored state,
and interfaces. Consult existing designs and recent changes when they establish
an important decision. Distinguish what the system does today from documented
intent and the proposed behavior.

Check whether existing capabilities already address the proposed work, whether
multiple components would own the same decision, and whether the proposal
conflicts with established behavior or reintroduces deliberately removed paths.
Support duplication claims with the actual responsibilities and code involved;
similar terminology alone is insufficient.

## Explore overlooked cases

Walk one concrete scenario through current and proposed behavior using the same
inputs. Identify who decides, acts, saves state, recovers, and observes the
outcome. Look for missing ownership, contradictory promises, and gaps between
an acknowledged operation and its actual effect.

Go beyond the document's examples. Follow plausible interactions and sequences
that its author may have missed: concurrent actions, retries, reordered events,
partial completion, dependency failures, restarts, and changing state. Consider
interactions with existing features, permissions, data constraints, scale, and
transitions between old and new behavior where relevant.

Use these as prompts, not a mandatory checklist. Ground each concern in a
supported use case or stated constraint. Explain the triggering conditions and
consequence; do not invent requirements or treat every hypothetical as a flaw.

## Deliver feedback only

Lead with consequential findings, ordered by impact. For each, identify the
scenario, the gap or contradiction, its consequence, and supporting document
sections and code references. Keep existing problems separate from problems
introduced or worsened by the proposal.

Distinguish demonstrated contradictions from supported risks and unresolved
questions. Ask for a missing fact or decision when it affects the assessment.
If implementation or evidence is unavailable, state the limitation; missing
access does not establish that a capability is absent. Missing detail in a
document is not by itself proof of a broken implementation.

Do not propose fixes, simpler alternatives, replacement designs, implementation
steps, or rewritten document text. Questions must expose missing information
or decisions without disguising a suggested solution. Challenge duplication
or complexity by explaining its consequence, without prescribing a replacement.

Close with material open questions and review limits when needed. If no
consequential issues were found, say so without implying exhaustive proof.

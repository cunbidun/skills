---
name: grill-me
description: Stress-test a plan, design, implementation, or PR through consequential trade-offs. Use an interview when asked to be grilled, or a written review when asked for important design questions; record decisions and compromises for sign-off.
---

# Grill me

Help the user decide which compromises to accept. Focus on unresolved choices
where every viable option has a meaningful upside and downside, and the best
answer depends on the user's priorities or constraints.

## Choose the format

- **Interactive interview:** ask one decision question at a time, then record
  and confirm the answer before advancing.
- **Written design review:** when the user asks for important questions or a
  review they can share, present the smallest useful set together. Use the same
  background, question, and recommendation structure for each. Do not force
  an interactive interview or imply that the author has accepted a proposal.

For a design, read the plan and the implementation it will change. For an
implementation or PR, inspect the actual diff, callers, and relevant recent
decisions. Pin a PR's head and merge-base, or reuse a comparison already
established in the conversation. Check what the design or code currently does
in each proposed scenario and cite the relevant evidence.

## Use the existing primitives and framing

Always map each decision to the concepts and mental model already established
in the conversation, project, or design. Recover the relevant definitions and
user-supplied references before framing the question.

- Describe the alternatives through the same existing actors, objects, states,
  operations, and relationships so their consequences can be compared directly.
- Use the project's established names and the user's framing. Do not introduce
  a new abstraction or rename a concept just to formulate the question.
- Connect each recommendation back to that model: what changes, what stays the
  same, and which existing rule or responsibility it affects.
- If the existing model cannot express the case, identify the precise gap and
  frame the decision as a proposed change to that model. Do not silently switch
  to a different framing or present a new primitive as already established.

## Choose questions worth asking

- Read the plan and relevant evidence first. Resolve factual questions from the code or documentation when possible.
- Keep a question only when the alternatives involve real costs: for example, faster delivery versus flexibility, simpler operation versus availability, or less retained data versus easier recovery. Explain the consequence in this particular situation, not just the abstract trade-off.
- Skip questions with an obvious best answer or no meaningful downside. State the recommendation briefly and move on; do not invent a disadvantage to make a question seem balanced.
- Follow dependencies: settle a decision before asking questions that depend on it. Do not reopen settled choices without new evidence.

## Frame each decision

Use this order for each question:

**Background:** Explain the relevant current behavior, goal, and constraint using the existing primitives and framing. Describe the viable alternatives and what each gains and gives up. Include only enough context to make the compromise understandable.

When it makes the comparison clearer, include a compact table with one row per
option and columns for its benefits, downsides, and when it fits. Use concrete
consequences; skip the table when a short paragraph is easier to understand.

**Question:** Ask one concrete question about which compromise the user prefers. Make the alternatives clear without hiding several independent decisions in one question.

**Recommendation:** Choose an option and explain why it fits the known priorities. Name the downside the user would be accepting and, when useful, what different priority would change your recommendation.

In an interview, wait for the answer, then document the decision before
advancing. In a written review, leave the questions for the author to answer.
Present obvious improvements as direct recommendations, not artificial
trade-off questions.

## Record decisions and get sign-off

Keep a running record as the discussion progresses. For each decision, capture:

- The question and chosen option, using the existing primitives and framing.
- The compromises accepted: what is gained, what is given up, and the practical consequences.
- The rationale, assumptions, and any unresolved conditions that could change the choice.
- Whether the record is awaiting sign-off or has been explicitly approved.

Use the current plan or design document when updating it is part of the
authorized task; otherwise keep the record in the conversation. Do not create
an external document or change unrelated files just to maintain the log.

After each interview answer, show the proposed record and ask the user to sign
off on the decision and its compromises before moving to the next question. A chosen
option alone does not approve compromises the user has not yet seen. If the
user corrects the record, revise it and present the corrected version. If they
have already explicitly approved the displayed record, do not ask again.

For a written review, record the author's replies as they arrive and show the
resulting decisions for sign-off before advancing to implementation. Until
then, keep recommendations marked as proposals.

Use the signed-off decisions to update the plan's assumptions and frame later
questions. When the consequential trade-offs are resolved, show the accumulated
record, distinguish signed-off choices from open items, and obtain sign-off on
any remaining items before moving to the next phase.

An interview does not authorize implementation, publishing, or changes to external systems.

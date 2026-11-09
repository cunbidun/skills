---
name: cr
description: Review a proposed change against its base, checking correctness and whether it preserves the architectural decisions and simplicity established by recent development. Use when asked for CR or a history-informed code review in any review system.
---

# CR

Understand what the change tries to accomplish, then judge it against both the existing behavior and the architectural
direction of the affected code. Recent redesigns matter more than superficial similarity to older code.

Keep the review read-only.

## Establish the exact comparison

Accept a review identifier, review URL, or remote feature branch. Identify the repository and review system from the
supplied reference and checkout, using the available tools and access.

- Resolve the review's actual target and proposed revision, including changes from forks or stacked reviews. Do not
  assume `main` or use a synthetic merge result as the proposed revision.
- For a remote branch, look for an associated review to establish its target. Without one, use the remote's default
  branch as an explicitly stated assumption only when the intended target is unambiguous. Ask when a stacked branch or
  several plausible targets would materially change the review.
- Verify the current target and proposed revision through the review system or version control before relying on cached
  refs. Record the repository, target, proposed revision, and comparison base. Pin the review to those revisions so
  incoming updates do not silently change the comparison.
- For Git, compute `git merge-base <target-sha> <head-sha>` and review `git diff <merge-base-sha> <head-sha>`; do not
  substitute a diff between the two branch tips. For another version control system, establish the equivalent base and
  proposed revision. Exclude uncommitted work. If the base or history cannot be established with available access,
  explain the gap and limit the review accordingly.

## Understand what the change is about

Read the review description and commit messages, then inspect the actual diff and surrounding code. Treat descriptions
as claims to verify.

Look for a linked tracking ticket in the review metadata, description, and commit messages. Read it when accessible to
understand the requested behavior and acceptance criteria. If no ticket is linked, flag the missing tracking ticket to
the user and continue with the available context. If a linked ticket is inaccessible, report that gap. Use the project's
tracking system.

Explain the intended change through a concrete event: what triggers it, which component acts, what it reads or receives,
what it writes or returns, and what the user observes. Compare before and after.

Map the touched services and modules to their callers, downstream readers, saved records, and external interfaces where
relevant. Read enough outside the diff to understand the behavior being changed. Identify which parts are
implementation, migrations, cleanup, tests, or generated output, and whether the change completes its stated goal.

## Recover the architectural direction

For the affected services and modules, study recent development on the target branch, especially substantial refactors,
redesigns, and removals. Use scoped version control history and inspect the meaningful patches; commit titles alone do
not establish a design decision. Follow renamed files or moved responsibilities when necessary.

Start with the most recent relevant changes and follow their rationale far enough to understand the design. Avoid an
arbitrary calendar cutoff or a repository-wide history dump. Consult associated review discussions and design documents
when they explain a consequential choice.

Distinguish three things:

- What the code does at the comparison base: the behavior this change starts from.
- What subsequent target-branch work changes: potential integration concerns, separately labeled rather than attributed
  to this change.
- What the recent history establishes: the simpler primitives and rules the affected code is moving toward.

Write a short, supported account of the important decisions. For each, say what a component does, why the work lives
there, and which commit, review discussion, document, or current code supports that reading. Mark inferred intent as
inference. Do not turn a one-off implementation detail into a universal rule.

Look particularly for decisions about who chooses, who writes, who retries, where shared calculations live, and which
duplicate paths were deliberately removed. These are questions to investigate, not a mandatory checklist for every
review.

## Review correctness and cleanliness together

Trace the changed behavior through its actual callers and readers. Look for concrete failures, compatibility breaks,
incomplete migrations, and missing cleanup. Use existing tests or small non-mutating checks when they can settle an
uncertainty; report what was actually verified.

Compare the change with the recovered decisions:

- Does it reuse the established simple primitives, or create another way to do the same job?
- Does it reintroduce responsibilities or code paths that recent work deliberately removed?
- Does a new flag, fallback, cache, scheduler, or helper solve a necessary problem, or hide a mismatch in the existing
  design?
- When replacing behavior, does it remove obsolete callers and adjacent pieces?
- Can the new behavior be explained in plain English without inventing a pile of abstractions?

### Duplicated responsibility

List the jobs the change does: decide, compute, size, price, value, name, set up, exit. For each job, find every place
in every touched service that does it, including the UI, APIs, workers, and storage, and code that existed before the
change. Copied code doesn't need to match line for line; two places own the same job if both answer the same question.

Report each job that has more than one owner, in plain language: "this logic here and that logic there are both
responsible for X", with `file:line` for each copy. Look for inputs where the copies disagree, and say what the user
sees when they do, such as a card that promises one thing while the order does another. If no input makes them disagree
yet, say it is a maintenance cost only.

### Environment variable changes

Highlight added, removed, or changed environment variables, settings, and feature flags, including defaults and changes
in deployment files or docs. Say what behavior changes and whether deployment needs a manual step.

Flag unjustified configuration with the smallest simpler alternative. Prefer constants for values that never vary and
expand/contract releases for compatibility. Keep deployment-specific values and justified operational switches;
temporary switches need a removal plan.

### Database migrations

Highlight schema and data changes and their affected readers and writers. Check deployment order and compatibility while
old and new code overlap; use expand/contract when needed.

Check existing data against new constraints, backfill completeness, locks and runtime at the expected data volume, and
safe retries after partial failure. Identify destructive operations and rollback limits, including whether old code can
read newly written data. Report concrete failure cases and missing evidence.

An intentional redesign may reasonably depart from prior work. Judge its rationale and completeness rather than
enforcing history mechanically. Keep existing problems separate from issues introduced or worsened by the reviewed
changes. Do not report stylistic preferences as defects.

For each actionable finding, identify the triggering case, the affected behavior, the consequence, and the smallest
useful correction. For an architectural concern, identify the prior decision and the concrete duplication, conflicting
behavior, or maintenance burden the change introduces. If the concern depends on an unresolved design choice, state the
question rather than claiming a proven bug.

## Deliver the review

Lead with consequential findings, ordered by severity, with precise file and line references to the pinned revision and
relevant supporting history. Keep line ranges narrow. Include only issues supported by inspected code or observed
behavior.

Then briefly explain:

- What the change does and why.
- Which recent architectural decisions matter, and whether the change preserves or intentionally changes them.
- The reviewed revision and comparison base, checks performed, and material gaps or assumptions.

If no actionable issues were found, say so explicitly without implying exhaustive proof. A clean correctness review can
still contain an explicitly labeled unresolved architectural question. Keep the explanation readable by someone who has
not followed the development history.

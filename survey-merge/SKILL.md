---
name: survey-merge
description: Survey a proposed merge and explain how to combine both branches without losing their intended behavior or architectural improvements. Use when asked to assess merge difficulty, map conflicts, or plan integration before merging. The survey does not perform the merge.
---

# Survey Merge

Give the person doing the merge a map of the intended combined behavior, the places that need deliberate translation, and the checks that will establish success.

## Keep the survey separate from execution

Inspect committed trees without changing the checkout, index, branch, or source files. A survey does not authorize conflict resolutions, commits, pushes, deployments, or data resets. Preserve dirty work and distinguish it from the committed comparison.

Honor the task's mutation limits when choosing a preview. Fetch missing history only when permitted. In a strictly read-only task, verify remote tips with `git ls-remote` or the hosting API; use locally available objects or explain what is missing. Do not call a cached remote reference current without checking.

`git merge-tree --write-tree` writes Git objects even though it leaves the index and working tree alone. A disposable checkout or temporary object directory also writes files. Use those only when the task permits them. A read-only `git merge-tree --trivial-merge BASE OURS THEIRS` can provide an initial text-conflict survey, but its rename handling and conflict counts are not equivalent to a full merge.

## Pin the comparison and recover intent

Record the repository, merge direction, both commit SHAs, merge-base, and working-tree status. If several merge-bases exist, do not silently choose one and present its preview as definitive. Keep using the pinned SHAs throughout the analysis.

Inspect each side from the common base:

```text
base → current branch: what this work deliberately changed or removed
base → incoming branch: everything the merge will bring in
```

Read the relevant commit messages and PR descriptions, then verify their claims in code. A large headline PR may be only part of the incoming work. Identify other changes to user behavior, storage, approval, account creation, APIs, or retry policy that affect the merge.

Read the latest supplied design documents and recent decisions. Separate durable principles from outdated examples and superseded implementation details. An updated page can still contain old sections. Use the user's current decisions to resolve that ambiguity; do not restore removed concepts merely because an older diagram names them.

## Map text conflicts and behavioral conflicts separately

Inventory files changed on both sides, text-conflict regions, modified/deleted overlaps, and renamed or moved responsibilities. Distinguish implementation, tests, generated files, and deployment configuration.

Be precise about counts:

- A file changed on both sides is not necessarily a conflict.
- A file deleted on one side is not a modified/deleted conflict unless the other side changed it.
- Conflict hunks, conflict files, and behavioral issues are different counts.
- Label an approximate preview as approximate. Report the method used.

Inspect files that merge without conflict too. An incoming helper or caller can refer to a removed type, assume an obsolete response shape, or depend on persistence that the new branch no longer supplies. Passing a parser or resolving every marker does not establish compatible behavior.

Follow the consequential paths through callers, writers, stored records, and readers. In particular, check:

- New request or object fields have matching database writes and reads; adding a model field alone may silently discard it on reload.
- Both sides agree on what an identifier names and what "approved," "ready," "settled," or "finished" proves.
- New execution steps participate in completion, recovery, accounting, and refresh logic.
- Retried work keeps its exact request identity and uses its own results, rather than unrelated historical results or the current total balance.
- A late receipt cannot overwrite newer configuration or be rejected merely because configuration changed after submission.
- Incoming readers preserve intentional limits on payload size and historical database work.

Use these as prompts where relevant, not as an excuse to audit unrelated systems.

## Choose a coherent result

For each affected area, say what behavior comes from each branch and what must be adapted or removed. Explain the choice through a concrete event and its consequence. Avoid blanket "take ours" or "take theirs" recommendations for files that contain multiple responsibilities.

For example, one branch may ensure a completed transfer's receipt survives a later permission change, while the other prevents that receipt writer from overwriting newly added wallet members. The merged function needs both behaviors.

Translate incoming features into the chosen primitives. Do not recreate obsolete types, storage paths, or compatibility aliases just to make imports pass. When the user authorizes a clean break, omit historical readers, backfills, and migration machinery; still preserve whatever resources the user explicitly retains and recover interruptions in the new system.

Keep the core merge distinct from bundled policy changes. A new retry limit, broader permission, changed funding rule, or different failure outcome deserves an explicit recommendation even if Git merges it cleanly. Distinguish preparation failures from an already-submitted operation whose result remains unknown.

Keep pre-existing defects separate from merge-induced risks. Recheck remembered findings against the pinned incoming code before repeating them. An unresolved question should name the specific behavior that needs a decision, not ask the user to choose an entire architecture again.

## Deliver a usable merge plan

Lead with feasibility and the governing integration choice. Include a compact map such as:

| Area | Preserve from current work | Bring from incoming work | Adapt or remove |
| --- | --- | --- | --- |
| Execution, storage, approval, or UI | Concrete behavior | Concrete behavior | The actual merge work |

Then identify the few hardest interactions, with links to the pinned code and relevant design decisions. Recommend a dependency-aware order: settle shared types and persistence before their callers, and regenerate API files after the hand-written definitions agree. Do not prescribe the same order for every repository.

Propose tests that exercise the combination, not merely each branch independently: new behavior after restart, old accepted work after a configuration change, concurrent edits and receipts, repeated activations, or bounded reads as history grows. Name the expected observable result. Do not run expensive or mutating tests merely to decorate a survey.

Finish with what was inspected, what remains uncertain, and whether any files or Git state changed. Make clear whether the result is a static survey, an approximate text preview, or a merge that was actually built and tested. Stop at the survey unless the user also requested implementation.

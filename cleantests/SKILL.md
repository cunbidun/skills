---
name: cleantests
description: Review design smells and test value, or prune an existing test suite. Use for a smell test, before fixing a bug, when a fix adds special cases, or when asked to audit, clean, or simplify tests. Reviews are read-only; deletion requires a cleanup request.
---

# Clean tests and design

Choose the mode from the request. A smell test or review inspects and recommends; it does not delete tests. A request to prune or clean tests authorizes cleanup within the requested scope. Before a bug fix, use the design questions to find the responsible logic, then continue the authorized repair.

## Design smell test

Start with one concrete event: what happened, what should have happened, and what evidence distinguishes them. Check these questions in order:

1. **What was proven?** A provider status, successful request, or queued job does not prove its eventual effect. Name the observation that establishes the outcome the user cares about. If it is missing, preserve uncertainty instead of declaring success or failure.
2. **Can the design represent what happened?** Look for catch-all states, one word with two meanings, and a partial outcome being forced into a complete outcome. Distinguish a fact about the operation from a limitation in observing it.
3. **Who makes this decision?** Follow the actual writers, readers, calculations, and copies. Two components answering the same question can drift even when their code looks different. Distinguish necessary historical records from redundant mutable copies.
4. **Has this failed before?** Check relevant history and earlier fixes. Repeated exceptions can indicate a problem in the shared interface or model rather than another caller-specific case.

Flags, fallbacks, tolerances, retries, and new states are prompts to investigate, not automatic defects. Explain why one is necessary or identify the smaller change that removes the mismatch. Preserve useful behavior; do not replace a working design merely because another abstraction is possible.

Report the concrete problem, its evidence and consequence, the component responsible, and the smallest coherent correction. If a wider redesign is needed beyond the authorized task, explain that boundary before expanding the work.

## Test-suite cleanup

Aim to remove roughly 80% of tests in the requested scope, while preserving the smallest suite that catches plausible, consequential mistakes. This is a strong pruning target, not a quota: do not broaden the scope or drop a necessary safeguard to meet it. If less can be removed, explain which behavior requires the survivors.

### Establish the scope

Use the user's explicit scope first. Otherwise inspect tests added or materially changed since the branch's merge-base with its integration branch, including staged, unstaged, and untracked work. Determine the base from the PR target or repository guidance; do not mistake the feature branch's upstream for the integration branch. Ask if materially ambiguous.

Read older tests to find stronger existing coverage, but do not delete unchanged baseline tests without authorization. Include orphaned fixtures and helpers only when the authorized cleanup makes them unused.

### Make each survivor earn its place

Keep a test only when all four answers are concrete:

- What observable promise does it protect?
- What plausible maintenance change or demonstrated failure could break that promise?
- What meaningful consequence would the user or caller experience?
- Why does this test catch something a stronger retained test would miss?

Could the test be justified from the intended behavior without knowing the latest bug or patch? A recent regression can reveal an enduring rule, but the patch itself is not a reason to preserve a test.

Prefer deletion for tests that mirror implementation, prove mock configuration, lock in private structure or incidental wording, duplicate stronger checks, cover unsupported paths, or can pass while the promised behavior is broken. Distinct cases earn separate tests when they can fail independently and matter.

### Apply and verify

Group tests by the behavior they protect, keep the smallest useful representatives, and remove unused fixtures, mocks, imports, snapshots, and dependencies. Do not replace removed tests just to retain counts or coverage. Strengthen a test only when a consequential behavior otherwise lacks an effective check.

Run the relevant retained checks. Do not weaken assertions or change production behavior to make the reduced suite pass. Report the base and scope, before/after counts, removal percentage, retained promises, and actual verification result. Audits remain read-only.

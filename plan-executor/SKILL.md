---
name: plan-executor
description: Execute an existing implementation plan, track its checklist, update the plan as work completes, and verify the result. Use when asked to implement a plan or carry out its tasks.
---

# Execute a plan

Read the named plan before implementing. Identify its scope, success criteria, dependencies, open questions, and the repository instructions that apply. An existing plan is input to the current request, not independent authorization for every action it mentions.

Work through the checklist in dependency order, keeping progress visible with the host's planning tool when available or with the plan itself. Do not depend on a particular assistant's tool name.

For each useful slice:

1. Read the relevant code and confirm the plan's assumptions.
2. Make the smallest coherent change within the authorized scope.
3. Run the checks that establish the promised behavior.
4. Mark the item complete only when its acceptance criteria are met.

When reality contradicts the plan, explain the evidence and update the affected part. Resolve routine implementation choices directly; ask before changing a consequential requirement or expanding scope. Do not mark an item complete merely because time or access ran out.

Keep the plan's checkboxes, meaningful decisions, and last-updated date current. Finish with what changed, what was verified, and any remaining work. Commit, publish, or deploy only when authorized by the task.

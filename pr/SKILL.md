---
name: pr
description: Finish authorized repository work, run relevant checks, commit, push, and open or update a pull request. Use when the user asks to deliver a PR; honor narrower requests such as commit-only or local-only.
---

# Pull request

An explicit request to deliver a PR includes its commit and push. Automatic selection of this skill does not authorize publication. Follow the user's narrower scope and existing authorization.

## Choose the repository and base

Inspect the current repository, branch, working tree, and existing worktrees. Reuse the task's checkout when appropriate, preserving unrelated work. Create a feature branch or worktree only when needed or required by repository instructions.

Use an explicitly requested base first, then the existing PR target or repository guidance. Otherwise discover the remote default branch. Do not assume every repository uses `staging` or `main`; ask when the choice materially changes the work.

## Finish and publish

1. Complete the requested work and inspect the full task diff. Keep cleanup within scope.
2. Run the repository's required checks and meaningful validation. Do not add tests that merely restate the implementation.
3. Stage the task's files and create a concise commit, unless already committed. Respect configured commit checks and signing.
4. Push normally, without force-pushing or bypassing checks. Resolve ordinary integration issues while preserving both sides' intended behavior; stop if the resolution requires a user decision.
5. Check for an existing PR for the branch. Update it or create one against the verified base.
6. Attach the PR to the current task using the host application's capability when available. Return its link and material validation results or limitations.

Do not merge or deploy unless separately authorized.

## Write for the reviewer

Lead with the concrete problem and resulting behavior. Explain implementation choices only when they help assess the change. Follow the repository's PR template and scale detail to the task.

Use real, comparable screenshots when they help explain a visible change and available tooling can capture it. State when a before view is unavailable. Never invent visual evidence.

For CLI publication, write multiline descriptions to a file and use the tool's body-file option. Preserve intended newlines and literal text.
